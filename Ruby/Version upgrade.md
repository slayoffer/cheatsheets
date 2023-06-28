There are a few ways to upgrade Ruby on your MacOS system, but one of the best methods is to use a Ruby version management tool like `rbenv` or `rvm`. These tools allow you to easily switch between different versions of Ruby on your system. Here's how you can do it with `rbenv`:

**Step 1: Install Homebrew**

If you don't have it already, install Homebrew. Homebrew is a package manager for MacOS that makes it easy to install a variety of software tools. To install Homebrew, open the Terminal app and paste in the following command:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

**Step 2: Install rbenv and ruby-build**

Next, use Homebrew to install `rbenv` and `ruby-build`. `rbenv` is the Ruby version manager, and `ruby-build` is a plugin that adds the `rbenv install` command, which simplifies the installation of new Ruby versions.

```bash
brew install rbenv
```

To initialize `rbenv`, add the following to your `~/.bash_profile` or `~/.zshrc` file:

```bash
if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi
```

Then run this command to restart your shell and make these changes take effect:

```bash
source ~/.bash_profile
```

or

```bash
source ~/.zshrc
```

Depending on which shell you are using.

Then, install `ruby-build` as an `rbenv` plugin:

```bash
brew install ruby-build
```

**Step 3: Install a new Ruby version**

Now that `rbenv` and `ruby-build` are installed, you can install a new Ruby version. First, check the available versions:

```bash
rbenv install -l
```

This will list all the available versions of Ruby. Once you have chosen a version, you can install it with:

```bash
rbenv install 2.7.1
```

Replace `2.7.1` with the version number of the Ruby version you want to install.

**Step 4: Set the new Ruby version as the default**

After the new Ruby version is installed, you can set it as the default version for your system:

```bash
rbenv global 2.7.1
```

Again, replace `2.7.1` with the version number you installed.

**Step 5: Verify the Ruby version**

Finally, you can verify that the new Ruby version is being used:

```bash
ruby -v
```

This should print out the Ruby version number you just installed.

With `rbenv`, you can also set different Ruby versions for different projects, or even different versions for different directories. This makes it a very flexible tool for managing your Ruby installations.
