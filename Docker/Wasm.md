### Possible Bugs You May Encounter in Docker + Wasm Technical Preview 2

The current technical build might have some bugs. It's technically "beta software". Which means it's very new, so it's not entirely polished. It will take time to fix all the bugs.

Bugs I've encountered when running this on a Linux operating system:

-   First of all, the link provided on the [Docker + Wasm Technical Preview 2 page](https://kodekloud.com/blog/r/f6eecc44?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab) seems to be broken for the Windows build. At the time of writing this blog, nothing happened when I clicked on that link. If you encounter the same issue, here's the fix: use this link instead to [download Docker + Wasm Technical Preview 2 for Windows](https://kodekloud.com/blog/r/15bb1fd8?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab).
-   Sometimes, after starting a Wasm container, it refused to exit normally. So the command hanged. I wasn't able to type anything else. Couldn't exit with CTRL+C either. Docker Desktop was not able to stop the container either.
-   When trying to run Wasm under the _spin_ runtime, I got an error. Seems to be a misconfiguration in Docker Desktop somewhere.
-   When trying to run Wasm under the _slight_ runtime, I got no result. Nothing happened, no text was displayed.

It's possible you might encounter none of these bugs in the technical build you get. But just in case you do, I've mentioned these so you know it's not an issue on your side. It's just a problem with this current technical preview, and most bugs will certainly be fixed in future builds.

Now let's continue.

![How to enable containerd image store support in Docker Desktop](https://lh6.googleusercontent.com/pPJxcrI8P_QUAUQclrIT8ikjVfTTUOjNh-12gXjRZRysMvnP93SNbQW_Sb1cBNFaz-MGCpphijM_wTztrgHVNiWPoj98OyXWyIoAKNGKRDtLErp1FFLUuN4Q_fDHgjdscZH87Ci17o36rrFrzesEuG0)

To get started, first activate this feature to "**Use containerd for pulling and storing images**" then click on the "**Apply & restart**" button.

In the blog about [how to use Wasm with Docker](https://kodekloud.com/blog/r/42d8f2fc?m=33f1e68f-509a-4d82-9ddb-920cb216d4ab), we covered how to use the WasmEdge runtime included in the first Docker + Wasm technical preview. The command used was:

```
docker run --rm --runtime=io.containerd.wasmedge.v1 --platform=wasi/wasm32 sl1ck/wasm-test
```

But now that we have three additional runtimes available, how do we use those?

To **run Wasm with the** _**wasmtime**_ **runtime**, we use this command:

```
docker run --rm --runtime=io.containerd.wasmtime.v1 --platform=wasi/wasm32 sl1ck/wasm-test
```

We can see that the key parameter here is:

> io.containerd.**wasmtime**.v1

So if we replace that highlighted keyword above, we can figure out how to use the other two runtimes.

To **run Wasm bytecode under** _**spin**_:

```
docker run --rm --runtime=io.containerd.spin.v1 --platform=wasi/wasm32 sl1ck/wasm-test
```

And to **run Wasm bytecode under** _**slight**_:

```
docker run --rm --runtime=io.containerd.slight.v1 --platform=wasi/wasm32 sl1ck/wasm-test
```

## Conclusion

We hope this blog was helpful in understanding these latest changes. What do you think about the Docker + Wasm combo? Are you using Wasm in any way? Do you see any interesting use-cases for running WebAssembly workloads in Kubernetes clusters?