# 1. Docker Init 🎂

You probably didn’t know about this command, because it was introduced in Docker Desktop 4.18, which came out yesterday.

```
docker init
```

This command creates a Dockerfile in your repo based on the languages you’re using. Try it out and tweet about the result you got.

Here is the Docker image it created for my Go repo:

```bash
# syntax=docker/dockerfile:1

# Comments are provided throughout this file to help you get started.
# If you need more help, visit the Dockerfile reference guide at
# https://docs.docker.com/engine/reference/builder/
################################################################################
# Create a stage for building the application.
ARG GO_VERSION=1.13
FROM golang:${GO_VERSION} AS build
WORKDIR /src
# Download dependencies as a separate step to take advantage of Docker's caching.
# Leverage a cache mount to /go/pkg/mod/ to speed up subsequent builds.
# Leverage bind mounts to go.sum and go.mod to avoid having to copy them into
# the container.
RUN --mount=type=cache,target=/go/pkg/mod/ \
    --mount=type=bind,source=go.sum,target=go.sum \
    --mount=type=bind,source=go.mod,target=go.mod \
    go mod download -x
# Build the application.
# Leverage a cache mount to /go/pkg/mod/ to speed up subsequent builds.
# Leverage a bind mount to the current directory to avoid having to copy the
# source code into the container.
RUN --mount=type=cache,target=/go/pkg/mod/ \
    --mount=type=bind,target=. \
    CGO_ENABLED=0 go build -o /bin/server ./cmd/server
################################################################################
# Create a new stage for running the application that contains the minimal
# runtime dependencies for the application. This often uses a different base
# image from the build stage where the necessary files are copied from the build
# stage.
#
# The example below uses the alpine image as the foundation for running the app.
# By specifying the "latest" tag, it will also use whatever happens to be the
# most recent version of that image when you build your Dockerfile. If
# reproducability is important, consider using a versioned tag
# (e.g., alpine:3.17.2) or SHA (e.g., alpine:sha256:c41ab5c992deb4fe7e5da09f67a8804a46bd0592bfdf0b1847dde0e0889d2bff).
FROM alpine:latest AS final
# Expose the port that the application listens on.
EXPOSE 8080
# What the container should run when it is started.
ENTRYPOINT [ "/bin/server" ]
# Install any runtime dependencies that are needed to run your application.
# Leverage a cache mount to /var/cache/apk/ to speed up subsequent builds.
RUN --mount=type=cache,target=/var/cache/apk \
    apk --update add \
        ca-certificates \
        tzdata \
        && \
        update-ca-certificates
# Create a non-priveldged user that the app will run under.
# See https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#user
ARG UID=10001
RUN adduser \
    --disabled-password \
    --gecos "" \
    --home "/nonexistent" \
    --shell "/sbin/nologin" \
    --no-create-home \
    --uid "${UID}" \
    appuser
USER appuser
# Copy the executable from the "build" stage.
COPY --from=build --chown=appuser:appuser /bin/server /bin/
```

# 🐳 2. Docker SBOM 🎂

Software Bill of Materials (SBOM) is a comprehensive list of all components used in building software, including third-party libraries.

Docker incorporated SBOM generation into their Command Line Interface (CLI) in April 2022:

```
docker sbom <image>
```

E.g. to run it against my `git-weekly` image:

```
docker sbom gitweekly/git-weekly
```

The result would be:

```
Syft v0.43.0
 ✔ Loaded image
 ✔ Parsed image
 ✔ Cataloged packages      [26 packages]
NAME                               VERSION                             TYPE
alpine-baselayout                  3.2.0-r22                           apk
alpine-baselayout-data             3.2.0-r22                           apk
alpine-keys                        2.4-r1                              apk
apk-tools                          2.12.9-r3                           apk
busybox                            1.35.0-r17                          apk
ca-certificates-bundle             20220614-r0                         apk
git-weekly                                                             go-module
github.com/labstack/echo           v3.3.10+incompatible                go-module
github.com/labstack/gommon         v0.4.0                              go-module
github.com/mattn/go-colorable      v0.1.11                             go-module
github.com/mattn/go-isatty         v0.0.14                             go-module
github.com/sirupsen/logrus         v1.9.0                              go-module
github.com/valyala/bytebufferpool  v1.0.0                              go-module
github.com/valyala/fasttemplate    v1.2.1                              go-module
golang.org/x/crypto                v0.0.0-20221012134737-56aed061732a  go-module
golang.org/x/net                   v0.0.0-20211112202133-69e39bad7dc2  go-module
golang.org/x/sys                   v0.0.0-20220715151400-c0bba94af5f8  go-module
golang.org/x/text                  v0.3.6                              go-module
libc-utils                         0.7.2-r3                            apk
libcrypto1.1                       1.1.1q-r0                           apk
libssl1.1                          1.1.1q-r0                           apk
musl                               1.2.3-r0                            apk
musl-utils                         1.2.3-r0                            apk
scanelf                            1.3.4-r0                            apk
ssl_client                         1.35.0-r17                          apk
zlib                               1.2.12-r3                           apk
```

# 🐳 3. Docker Scout 🎂

Docker introduced its supply chain security solution in the last Docker Desktop version, 4.17. To get a list of all of the CVEs, run:

```
docker scout cves <image>
```

Again, to test it against my own image:

```
docker scout cves gitweekly/git-weekly
```

And the result starts like this:

```
    ✓ Provenance obtained from attestation
    ✓ SBOM obtained from attestation, 27 packages indexed
    ✓ Pulled
    ✗ Detected 3 vulnerable packages with a total of 10 vulnerabilities

...
```

# 🐳 4. Docker Build with Attestation 🎂

As you can see in the Docker Scout report, it says “SBOM obtained from attestation”. It means there is something called “attestation” and SBOM can be obtained from it.

That’s what we’re going to do here. Generate the SBOM during the build phase and embed it into the image as an “attestation”:

```
docker buildx build --sbom=true -t <tag> .
```

# 🐳 5. Docker Scout Recommendations 🎂

All the Docker Scout subcommands except for `cves` are added in Docker Desktop 4.18. One of them is the following:

```
docker scout recommendations <image>
```

It will show recommendations for fixing the image you scanned. Let’s try it on my older `git-weekly` image:

```
docker scout recommendations aerabi/git-weekly
```

And as a part of the response, it suggests changing the base image to the version `3.16` as it will get rid of all of the vulnerabilities.

# 🐳 6. Docker Scout QuickView 🎂

This command will show a quick summary of the image’s vulnerabilities:

```
docker scout quickview <image>
```

Let’s do it on my older image:

```
docker scout quickview aerabi/git-weekly
```

Here is the result:

```
✓ SBOM of image already cached, 31 packages indexed

  Your image  aerabi/git-weekly    │    0C    17H     7M     0L     5?
  Base image  alpine:3             │    0C     1H     3M     0L     2?
  Refreshed base image  alpine:3   │    0C     0H     0M     0L
                                   │           -1     -3            -2
  Updated base image  alpine:3.16  │    0C     0H     0M     0L
                                   │           -1     -3            -2

  │ Know more about vulnerabilities:
  │    docker scout cves aerabi/git-weekly
  │ Know more about base image update recommendations:
  │    docker scout recommendations aerabi/git-weekly
```

# 🐳 7. Docker Scout Compare 🎂

Okay, I have jumped between my old image and the new image a few times now. What’s the difference between them?

```
docker scout compare --to <older-image> <image>
```

This command shows you how two images compare in terms of the base image, packages, and vulnerabilities. Let’s try it out:

```
docker scout compare --to aerabi/git-weekly gitweekly/git-weekly
```

The result is too comprehensive for this article, so I’ll paste this part only:

```
  ## Packages

    +    3 packages added
    -    7 packages removed
    ⎌   21 packages changed (↑ 9 upgraded, ↓ 0 downgraded)
       3 packages unchanged

  ## Vulnerabilities

    + 1 vulnerability added
    - 17 vulnerabilities removed
```

# 🐳 8. Docker Scout’s Hidden Command 🎂

Okay, this is a hidden command. Let’s list all of the available Docker Scout commands:

```
docker scout help
```

And we get:

```
Commands:
  compare         [early preview] Compare two images and display differences
  cves            Display CVEs identified in a software artifact
  quickview       Quick overview of an image
  recommendations Display available base image updates and remediation recommendations
  version         Show Docker Scout version information
```

We have covered all of those commands, didn’t we? Not quite. There is another command that is not listed there:

```
docker scout sbom <image>
```

Although it’s not listed there, it has documentation if you know where to look for it:

```
docker scout sbom --help
```

And it says:

```
Examples:
  Display the list of packages
  $ docker scout sbom alpine
  Only display packages of a specific type
  $ docker scout sbom --only-package-type apk alpine
  Display the full SBOM as json
  $ docker scout sbom --format json alpine
  Write SBOM to a file
  $ docker scout sbom --format json --output alpine.sbom alpine
```

So, basically, Docker Scout SBOM to Docker Desktop is what Reptile was to Mortal Kombat II.

# 🐳 9. Docker Build for Other Platforms 🎂

When building a Docker image, you can build it for different platforms with different architectures. This is especially handy if you have a laptop with ARM CPU on it:

```
docker buildx build --platform=linux/amd64,linux/arm64 -t <tag> .
```

# 🐳 10. Docker Commit 🎂

This command is to create an image out of the running container.

```
docker commit <container> <image-tag>
```