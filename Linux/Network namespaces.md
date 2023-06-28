### Create

```bash
ip netns add red_namespace
```

### List network namespaces

```bash
ip netns
```

### List interfaces

```bash
ip list
```

### Execute CMD inside namespace

```bash
ip netns exec red_namespace ip link
# or
ip -n red_namespace link
```

### Add devices and IPs for NSes

```bash
ip link add type veth peer name veth-blue

ip link set veth-red netns red

ip link set veth-blue netns blue

ip -n blue addr add 192.168.15.2 dev veth-blue

in -n blue link set veth-blue up

ip netns exec red arp
```

### Create bridge and connect NSes through it

```bash
ip link add v-net-o type bridge

ip link

ip link set dev v-net-e up

ip -n red link del veth-red

ip link add veth-red type veth peer name veth-red-br

ip link add veth-blue type veth peer name veth-blue-br

Sip link set veth-red netns red

ip link set veth-red-br master v-net-o

ip link set veth-blue netns blue

ip link set veth-blue-br master v-net-Ø

ip -n red addr add 192.168.15.1 dev veth-red

ip -n blue addr add 192.168.15.2 dev veth-blue

ip -n red link set veth-red up

ip -n blue link set veth-blue up

ip addr add 192.168.15.5/24 dev v-net-e # can ping these NSes from host now

ping 192.168.15.1
```

### Give NS external internet access

```bash
ip netns exec blue ip route add 192.168.1.0/24 via 192.168.15.5

iptables -t nat -A POSTROUTING -s 192.168.15.0/24 -j MASQUERADE

ip netns exec blue ip route add default via 192.168.15.5

iptables -t nat -A PREROUTING --dport 80 --to-destination 192.168.15.2:80 -j DNAT
```

