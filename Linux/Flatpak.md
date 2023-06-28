## Install Flatpak

To install Flatpak on Ubuntu 18.10 (Cosmic Cuttlefish) or later, simply run:

```bash
sudo apt install flatpak 
```

## Install the GUI Software Flatpak plugin (only for GUI OS)

The Flatpak plugin for the Software app makes it possible to install apps without needing the command line. To install, run:

```bash
sudo apt install gnome-software-plugin-flatpak
```

## Add the Flathub repository

```bash
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```

### List installed apps

```bash
flatbak list
```

### Update apps

```bash
flatpak update

# with app name
flatpak update package_name

# update from gui
sudo apt install gnome-software-plugin-flatpak
```

### Remove apps

```bash
flatpak uninstall <app name>

# remove unused apps
flatpak uninstall --unused
```

