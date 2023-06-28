[ripgrep](https://github.com/BurntSushi/ripgrep?ref=linuxhandbook.com) is an excellent outcome of the RIIR (re-write it in Rust) effort going on in the open source community. It is intended to be a superior replacement for the classic [grep command](https://linuxhandbook.com/what-is-grep/).

The syntax to use ripgrep is as follows:

```bash
rg <pattern> [files/directories]
```

With ripgrep, there is no need to mention a file name. When a file name is not provided, all files are searched. This is incredibly helpful if you do not know which file contains the pattern you search.

You may also [use grep to search in all files](https://linuxhandbook.com/grep-search-all-files-directories/) but ripgrep does it without any additional effort.

## What is ripgrep?

[ripgrep](https://github.com/BurntSushi/ripgrep?ref=linuxhandbook.com) is a recursive regex pattern matching tool that considers your gitignore. If you have specific files, extensions, or directories in your gitignore, ripgrep will ignore them, speeding up execution time.

A few features that make ripgrep stand out are as follows:

-   Searches for patterns recursively in directories
-   Color highlighting in the output
-   Supports a large variety of encoding formats like UTF-8, SHIFT_JIS
-   Ability to search in compressed zip files
-   Ignores hidden files by default and uses your gitignore file for faster searching

You can think of it as grep, but primarily aimed at searching for files/content of files instead of the raw byte stream that grep deals with.

## Install ripgrep

While grep comes preinstalled on most Linux systems, ripgrep doesn't have that privilege.

However, it is available in the repositories of all major Linux distributions, and you can use the package manager to install it.

If you are a user of Arch Linux, you already know how to install packages :p, but still, here's the command you should use:

```bash
pacman -S ripgrep
```

Gentoo users can install ripgrep with the following command:

```bash
emerge sys-apps/ripgrep
```

If you use Fedora or Red Hat, tip your hat a bit while typing this command in your terminal:

```bash
sudo dnf install ripgrep
```

openSUSE users (15.1 and later) should use the following command in their terminal:

```bash
sudo zypper install ripgrep
```

For users using Debian Buster (v10) or later, use apt. Ubuntu Cosmic Cuttlefish (18.10) or later can also use the distribution's official repositories.

```bash
sudo apt install ripgrep
```

## Using ripgrep command

If you are familiar with [grep command usage](https://linuxhandbook.com/grep-command-examples/), you'll find ripgrep works similarly. You provide it with a search string and a filename, and it will search the file and show you where the input string matched with the file contents.

For this tutorial, I have cloned the [repository of the dust project](https://github.com/bootandy/dust?ref=linuxhandbook.com), and I will be executing the commands inside the cloned repository.

### Basic searches

A sample search of the word description in the Cargo.html file:

```bash
$ rg description Cargo.toml
3:description = "A more intuitive version of du"
53:extended-description = """\
```

As expected, the ripgrep tool searched in the file that I specified and displayed the files with matching text and the line number.

[![demonstration of ripgrep command](https://linuxhandbook.com/content/images/2022/02/01_ripgrep.webp)](https://linuxhandbook.com/content/images/2022/02/01_ripgrep.webp)

If you specify multiple files to search for (if you don't specify any files, it will search for all), ripgrep will also determine the filename whose contents matched.

[![demonstration of ripgrep command with files to search in](https://linuxhandbook.com/content/images/2022/02/02_multiple_files.webp)](https://linuxhandbook.com/content/images/2022/02/02_multiple_files.webp)

Alternatively, you can also use the '--file' option, which contains the pattern you want to match. When you regularly search for a set of patterns to match, you can store it in a file and specify it using the '--file' option.

[![demonstration of ripgrep command with a file containing pattern](https://linuxhandbook.com/content/images/2022/02/03_file.webp)](https://linuxhandbook.com/content/images/2022/02/03_file.webp)

### Contextual search

Sometimes, it is nice to have a context of the matching line, especially when searching in a code repository. The '-C' or '--context' option helps here. This option accepts a numerical value and shows lines before and after the match.

[![demonstration of ripgrep command for a contextual search](https://linuxhandbook.com/content/images/2022/02/04_context.webp)](https://linuxhandbook.com/content/images/2022/02/04_context.webp)

There can be times when you only want to see a few lines above, including the matched line. Sometimes, you want the lines only below, including the matched one.

You can use the option '-A,' short for '--after-context', and a numerical value to show lines after each match.

[![demonstration of ripgrep command using '--after-context' flag](https://linuxhandbook.com/content/images/2022/02/05_after_context.webp)](https://linuxhandbook.com/content/images/2022/02/05_after_context.webp)

And for lines before each match, you can use the option '-B,' short for '--before-context', along with a numerical value.

[![demonstration of ripgrep command using '--before-context' flag](https://linuxhandbook.com/content/images/2022/02/06_before_context.webp)](https://linuxhandbook.com/content/images/2022/02/06_before_context.webp)

### Columns

There are several options regarding columns that ripgrep provides.

You will appreciate the '--column' flag if you are a vim user. It prints the 'line:column' of the matched text in the file.

[![demonstration of ripgrep command using '--column' flag](https://linuxhandbook.com/content/images/2022/02/07_column.webp)](https://linuxhandbook.com/content/images/2022/02/07_column.webp)

Another option related to columns is '-M' or '--max-columns', which takes a numeric value for the maximum number of columns. If the columns of matched lines exceed, it will let you know that a particular line was omitted from being outputted to the terminal.

[![demonstration of ripgrep command using '--max-columns' flag](https://linuxhandbook.com/content/images/2022/02/08_max_column.webp)](https://linuxhandbook.com/content/images/2022/02/08_max_column.webp)

### Misc

There are several options that you can use with ripgrep.

You can use the '-s' or '--case-sensitive' option to match text with case sensitivity.

[![demonstration of ripgrep command using '-s' flag](https://linuxhandbook.com/content/images/2022/02/09_case_sensitive.webp)](https://linuxhandbook.com/content/images/2022/02/09_case_sensitive.webp)

If you want to keep the case insensitive, you can use the '-i' or '--ignore-case' flag.

[![demonstration of ripgrep command using '--ignore-case' flag](https://linuxhandbook.com/content/images/2022/02/10_case_insensitive.webp)](https://linuxhandbook.com/content/images/2022/02/10_case_insensitive.webp)

If you have a huge codebase, you can use multiple threads for pattern matching. You can manually specify threads using the '-j' or '--threads' option; it accepts a numerical value.

```bash
$ rg -j 4 TODO
```

There can be occasions when you want to exclude a pattern from search results. To do that, you can use the '-v' or '--invert-match' to exclude the specified pattern.

[![demonstration of ripgrep command using '--invert-match' command](https://linuxhandbook.com/content/images/2022/02/11_invert.webp)](https://linuxhandbook.com/content/images/2022/02/11_invert.webp)

ripgrep can search for text in a compressed archive (if the compressed file is a text file) using the '-z' or '--search-zip' flag. This flag is usually accompanied by the '-a' flag that treats binary files as text files.