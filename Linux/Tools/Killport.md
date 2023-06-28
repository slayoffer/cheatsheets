## Installation

### Using Homebrew

Run the following command to install killport using Homebrew.

```
brew tap jkfran/killport
brew install killport
```

### Using install.sh

Run the following command to automatically download and install `killport`:

```
curl -sL https://bit.ly/killport | sh
```

Don't forget to add `$HOME/.local/bin` to your `PATH` environment variable, if it's not already present.

### Using cargo

Run the following command to install killport using cargo. If you don't have cargo, follow the [official Rust installation guide](https://www.rust-lang.org/tools/install).

```
cargo install killport
```

### Binary Releases

You can download the binary releases for different architectures from the [releases page](https://github.com/jkfran/killport/releases) and manually install them.

## Usage

```
killport [FLAGS] <ports>...
```

### Examples

Kill a single process listening on port 8080:

```
killport 8080
```

Kill multiple processes listening on ports 8045, 8046, and 8080:

```
killport 8045 8046 8080
```

### Flags

-v, --verbose Increase the verbosity level. Use multiple times for more detailed output.

-h, --help Display the help message and exit.

-V, --version Display the version information and exit.