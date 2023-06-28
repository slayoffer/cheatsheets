The `nc` or [Netcat](https://netcat.sourceforge.net/) utility is a simple, but ever so useful tool to have in your troubleshooting arsenal. Using this program you can open up sockets, establish network connections and send data over a network. It is crazy easy to use, too.

Let’s say you wanted to listen on a specific port for some incoming traffic. All you need to do is run `nc` using the following commands for either TCP or UDP protocols:

```
# TCP
nc -l 50000

# UDP
nc -u -l 50000
```

This will open up a listener server on the port that you specify. In this case we’ve selected port `50000`. Once the server is started you won’t see anything, but can test it out using… you guessed it, more `nc` commands.

Keep your listener running and in another window send some data to it using the following commands:

```
# TCP
echo "hello" | nc localhost 50000

# UDP
echo "hello" | nc -u localhost 50000
```

You should see a nice “*hello*” pop up in the listener window. It works!

But what is this useful for? Many, many things. You can troubleshoot socket connectivity issues, investigate payloads in raw incoming traffic or just send basic data and objects across the network.

Netcat even supports establishing secure connections using TLS. You can specify all kinds of additional certificate parameters. If you’re working with any kind of network protocol, having familiarity with `nc` will definitely save you some headaches.