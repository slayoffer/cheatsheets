## Install via apt (DEPRECATED!)

```bash
sudo apt install ansible
```

## Installing and upgrading Ansible via Pip

### Locating Python

Locate and remember the path to the Python interpreter you wish to use to run Ansible. The following instructions refer to this Python as `python3`. For example, if you’ve determined that you want the Python at `/usr/bin/python3.9` to be the one that you’ll install Ansible under, specify that instead of `python3`.

### Ensuring `pip` is available

To verify whether `pip` is already installed for your preferred Python:

```
python3 -m pip -V
```

If all is well, you should see something like the following:

```
python3 -m pip -V
pip 21.0.1 from /usr/lib/python3.9/site-packages/pip (python 3.9)
```

If so, `pip` is available, and you can move on to the [next step](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#pip-install).

If you see an error like `No module named pip`, you’ll need to install `pip` under your chosen Python interpreter before proceeding. This may mean installing an additional OS package (for example, `python3-pip`), or installing the latest `pip` directly from the Python Packaging Authority by running the following:

```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python3 get-pip.py --user
```

You may need to perform some additional configuration before you are able to run Ansible. See the Python documentation on [installing to the user site](https://packaging.python.org/tutorials/installing-packages/#installing-to-the-user-site) for more information.



### Installing Ansible

Use `pip` in your selected Python environment to install the Ansible package of your choice for the current user:

```
$ python3 -m pip install --user ansible
```

Alternately, you can install a specific version of `ansible-core` in this Python environment:

```
$ python3 -m pip install --user ansible-core==2.12.3
```



### Upgrading Ansible

To upgrade an existing Ansible installation in this Python environment to the latest released version, simply add `--upgrade` to the command above:

```
$ python3 -m pip install --upgrade --user ansible
```

### Confirming your installation

You can test that Ansible is installed correctly by checking the version:

```
ansible --version
```

The version displayed by this command is for the associated `ansible-core` package that has been installed.

To check the version of the `ansible` package that has been installed:

```
python3 -m pip show ansible
```

## Adding Ansible command shell completion

You can add shell completion of the Ansible command line utilities by installing an optional dependency called `argcomplete`. `argcomplete` supports bash, and has limited support for zsh and tcsh.

For more information about installation and configuration, see the [argcomplete documentation](https://kislyuk.github.io/argcomplete/).

Installing `argcomplete`

```
$ python3 -m pip install --user argcomplete
```

Configuring `argcomplete`

There are 2 ways to configure `argcomplete` to allow shell completion of the Ansible command line utilities: globally or per command.

Global configuration

Global completion requires bash 4.2.

```
$ activate-global-python-argcomplete --user
```

This will write a bash completion file to a user location. Use `--dest` to change the location or `sudo` to set up the completion globally.