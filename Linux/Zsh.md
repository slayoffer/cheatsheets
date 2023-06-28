### Change shell

```bash
chsh

/bin/zsh

# for Windows Git Bash add to ~/.bash_profile
bash -c zsh
```

### Edit Zsh config

```bash
vim ~/.zshrc
```

### Install Oh My Zsh

```bash
sh -c "$(wget https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh -O -)"
```

### 10k theme

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
# replace current theme with powerlevel10k/powerlevel10k in ~/.zshrc
# restart zsh and config theme
p10k configure
```

### Highlighting addon

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
# add zsh-autosuggestions to ~/.zshrc plugins section
```

### Auto-suggestions

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
# add zsh-syntax-highlighting to ~/.zshrc plugins section
```

### Add Meslo font to Windows

```bash
bash -c "$(curl -fsSL https://gist.githubusercontent.com/romkatv/aa7a70fe656d8b655e3c324eb10f6a8b/raw/install_meslo_wsl.sh)"

# refer to https://gist.github.com/romkatv/aa7a70fe656d8b655e3c324eb10f6a8b
```

### Add Meslo font to VSCode

```bash
"terminal.integrated.fontFamily": "MesloLGS NF",
```

### Remove Zsh from VSCode terminal but keep in Windows Terminal

```bash
# comment out this line in ~/.bash_profile
# bash -c zsh

# Add a switch-to-zsh.sh script into user directory
#!/bin/bash
bash -c zsh

# replace with this in Git Bash commandline section of Windows Terminal settings.json: 
"C:/Program Files/Git/bin/bash.exe --login switch-to-zsh.sh"
```

