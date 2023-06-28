Run the following command to update the package list:

```bash
sudo apt-get update
```

Run the following command to install the `iproute2` package:

```bash
sudo apt-get install iproute2
```

Once the package is installed, you can use the `ip` command to check the IP address of your Debian system as described in my previous answer:

```bash
ip addr show
```

or

```bash
ip addr show | grep inet
```