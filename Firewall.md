# a. Setup rules on router to block all access into it except ping

First, we need to set the rule to block all incoming traffic:

```
sudo iptables -P INPUT DROP
```

This will set the default incomming traffic (INPUT) to block (DROP).

Then will we set ping (icmp) request to ACCEPT:

```
sudo iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT
```

After all, we will save the rule to ensure it persist even after reboot:

```
sudo iptables-save > /etc/iptables/rules.v4
```

# Setup rules on router to prevent computers on subnet 10.9.0.0/24 from accessing the internal web server (iweb).

First we need to identify the current internal websever IP which is 172.16.10.0/24.

Then we identify the target subnet to be blocked, in this case, it's: 10.9.0.0/24.

Create an iptable rule to block the target subnet from entering the webserver:

```
sudo iptables -A INPUT -s 10.9.0.0/24 -d 172.16.10.0/24 -j DROP
```

After all, we will save the rule to ensure it persist even after reboot:

```
sudo iptables-save > /etc/iptables/rules.v4
```

# Setup rules on router to stop computers on subnet 172.16.10.0/24 from accessing the badsite.

Identify the source, which is computer from subnet 172.16.10.0/24.

Destination is subnet 10.9.0.0/24

We create a rule to block traffic from internal network to the badsite:

```
iptables -A FORWARD -s 172.16.10.0/24 -d 10.9.0.0/24 -j DROP
```

Then save the rule just as the previous.
