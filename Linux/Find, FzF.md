## Fzf, the Fuzzy Finder in Linux

Fzf is a fuzzy search tool available for Linux, where you can search for files interactively.

To install `fzf` in Ubuntu, open a terminal and run:

```
sudo apt install fzf
```

While `fzf` itself works properly, it is wise to use it in conjunction with other tools to make most out of it.

### Using fzf

Open a terminal and run:

```
fzf
```

This will open a prompt of `fzf` where you can search for files in the current working directory.

[![Running fzf command in terminal](https://itsfoss.com/content/images/2023/06/fzf-default.svg)](https://itsfoss.com/content/images/2023/06/fzf-default.svg)

Default `fzf`

#### Apply a border to fzf

You can use the `--border` option of fzf. There are several options like rounded, sharp etc.

```
fzf --border=rounded
```

[![Running fzf command wth border option set to rounded and sharp](https://itsfoss.com/content/images/2023/06/fzf-border.svg)](https://itsfoss.com/content/images/2023/06/fzf-border.svg)

`fzf` with border

#### Apply background and foreground color

Using the color property, you can set ANSI colors to `fzf` either as background, foreground or both.

[![Colored output for fzf, where the colors are specified by the user](https://itsfoss.com/content/images/2023/06/fzf-colored.svg)](https://itsfoss.com/content/images/2023/06/fzf-colored.svg)

fzf colored

```
fzf --color="bg:black,fg:yellow" 
```

You can concatenate the options to make `fzf` visually pleasing.

Now, let me show some practical usage of the fuzzy search with fzf.

### Use fzf to search within bash history

Of course, there is CTRL+R reverse search in the bash history. But if you want to use `fzf` to get a better look, run:

```
history | fzf
```

[![Using fzf fuzzy search to search within the bash history](https://itsfoss.com/content/images/2023/06/search-within-bash-history.svg)](https://itsfoss.com/content/images/2023/06/search-within-bash-history.svg)

Use `fzf` to search within bash history

### Use fzf with tree command

[Tree command](https://linuxhandbook.com/tree-command/?ref=itsfoss.com) lists files and directories along with their hierarchical connection.

Using `fzf` with `tree` command can help you find the absolute path of a particular file.

```
tree -afR /home/$USER | fzf
```

[![Running Tree command and piping the output to Fuzzy search](https://itsfoss.com/content/images/2023/06/tree-afr.svg)](https://itsfoss.com/content/images/2023/06/tree-afr.svg)

Tree and FZF command

💡

The above command will invoke `tree` and list all files (-a) including hidden ones in a recursive fashion (-R). Also, the `-f` option tells tree to list the full path.

### Preview files in fzf

Sometimes, it will be helpful if you get a small preview of the file you are searching.

Luckily, `fzf` provides a preview option. You can access it by using `--preview`. I am here using `find`command to make it even more useful.

```
find /home/$USER -type f | fzf --preview 'less {}'
```

Here, while you scroll through the result, it will display the text files using less.

🚧

If you are using other commands like `ls`, etc. do not use options like `-l`, that will display added details (file permissions). These additional details will break the required format needed for `fzf` preview. the hile using preview feature, the input to `fzf` should only be the filename.

If you have `bat` installed, you can use it for previewing files as well.

```
find /home/$USER -type f | fzf --preview 'bat --color always {}'
```

[![Using bat as the text viewer for FZF preview function](https://itsfoss.com/content/images/2023/06/bashrc-preview-in-fzf.png)](https://itsfoss.com/content/images/2023/06/bashrc-preview-in-fzf.png)

FZF file preview using bat editor

For Ubuntu users, bat is available as `batcat`. So run:

```
find /home/$USER -type f | fzf --preview 'batcat --color always {}'
```

💡

[Create an alias](https://linuxhandbook.com/linux-alias-command/?ref=itsfoss.com) for these commands, so that you don't want to type these again and again.

### Use fzf to cd into any directory from anywhere (advance)

This is a bit trickier than the previous. Here, you cannot just directly pipe `fzf` and `cd` together, because both are different processes.

You can create an alias use the command like:

```
cd $(find /home/$USER -type d | fzf)
```

Or, you can follow the method explained below.

To do this, you may need to add a function to your bashrc. Let me call this function as `finder`. Now add the following lines to your bashrc.

```
finder() {
  local dir
  dir=$(find required/location/to/search/and/enter -type d | fzf)
  if [[ -n "$dir" ]]; then
    cd "$dir" || return
  fi
}
```

Now, you should [enter the location](https://itsfoss.com/change-directories/) where the directories you want to search and enter are present.

For example, I have replaced that part with `/home/$USER` to indicate that I have to `cd` into any directories in my Home from anywhere.

Once you saved your bashrc, either restart the terminal or run:

```
source ~/.bashrc
```

After this, you can run finder from the terminal and once you located the directory you want to enter, press Enter key.

[![Use fzf command to enter into any directory with the help of cd command](https://itsfoss.com/content/images/2023/06/fzf-cd-combo.svg)](https://itsfoss.com/content/images/2023/06/fzf-cd-combo.svg)

### Copy the selection to Clipboard

Till now, you have seen using `fzf` and in all cases, it gives either a search result or preview.

Now, if you want to copy the location of an item, you don't necessarily need to do it manually. There is a solution for that too.

First, make sure you have Xclip installed.

```
sudo apt install xclip
```

Now pipe it to xclip like this:

```
fzf | xclip -selection clipboard
```

This will copy whatever lines you have pressed the enter key, on to your clipboard.

### Other Uses

Like I said earlier, you can use any command that involves a significant amount of text, and you want to search for a particular thing interactively.

- `cat ~/.bashrc | fzf` - Search Inside Bashrc
- `lsblk | fzf` - Search inside the list of lock devices
- `ps -aux | fzf` - Search inside process list

## Another choice: Fzy, the Fuzzy Selector

Unlike `fzf`, `fzy` is a fuzzy selector, where you will be provided a menu to select, depending on the input.

For example, if you are using `fzy` in conjunction with `ls` command, it will give you a menu like interface.

[![FZY command with ls](https://itsfoss.com/content/images/2023/06/fzy-ls.svg)](https://itsfoss.com/content/images/2023/06/fzy-ls.svg)

`fzy` command

By default, it will show you ten entries in view.

### **Enter into a directory using fzy**

Similar to fzf, fzy can also be used to enter into a directory in the current working directory using:

```
cd $(find -type d | fzy)
```

[![](https://itsfoss.com/content/images/2023/06/fzy-cd.svg)](https://itsfoss.com/content/images/2023/06/fzy-cd.svg)

### Open a file using any editor

Or open a file using your favorite editor by:

```
nano $(find -type f | fzy)
```

[![](https://itsfoss.com/content/images/2023/06/fzy-nano.svg)](https://itsfoss.com/content/images/2023/06/fzy-nano.svg)

## Bonus: A Customized file and image preview

The below command will open a dedicated customized prompt in **Ubuntu** for fuzzy search, where you can preview text files by scrolling through them.

```
find /home/$USER -type f | fzf --color="bg:black,fg:yellow" --preview 'batcat --color always {}' --preview-window=bottom
```

Create an alias for this in your bashrc for easy access.

Or preview an image in fzf while scrolling using `timg` command line image viewer. Install it using:

```
sudo apt install timg
```

🚧

Remember that the image viewer will not display a proper image, as that is not the primary purpose of fzf preview

```
fzf --preview 'timg -g 200x100 {}' --preview-window=right:90
```

For those who are tinkerers, try to make this part by refining.