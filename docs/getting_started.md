# Getting Started with HippieDB 🌈
---

Welcome to HippieDB, the free-spirited, cloud-based database that’s all about simplicity, sharing, and good vibes.
Whether you’re a seasoned developer or just starting your journey, HippieDB is here to make your life easier, no complex setups, no permissions, no storage hassles. Just pure, unadulterated data sharing. Let’s dive in!

---

## **Connecting to HippieDB**
HippieDB is as easy to connect to as a handshake at a music festival. All you need is a TCP client, and you’re good to go. Here’s how:

Open a TCP connection to the HippieDB server at **hippiedb.groovy.ch** on port **1970**.

Start sending commands. No authentication, no users, no passwords, just connect and vibe.

### Netcat

    nc hippiedb.groovy.ch 1970
    set mykey myvalue
    get myKey
    stop

### Example in Python:

```python
import socket

# Connect to HippieDB
client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect(('hippiedb.groovy.ch', 1970))

# Send commands and receive responses
client_socket.send(b'set mykey myvalue\n')
response = client_socket.recv(1024)
print(response.decode())

# Close the connection when done
client_socket.send(b'stop\n')
client_socket.close()
```

You can find more [client examples here.](clients.md)    

---
## **Commands**
HippieDB keeps it simple with just a handful of commands. Here’s the full list:

###  **SET key value**
Stores a value under a key.

Keys and values must not contain spaces (keep it simple, man).

Example: 

    set groovy peace
    Response: OK

###  **GET key**
Retrieves the value stored under a key.

Example: 

    get groovy
    Response: peace

###  **DEL key**
Deletes a key and its value.

Example: 

    del groovy
    Response: OK

###  **INCR key [value]**
Increments the numeric value of a key by the specified [value]. If the [value] is not provided, it defaults to 1. If the key doesn’t exist, it starts at 0.

Examples:

Increment by 1 (default):

    incr counter
    Response: 1

Increment by a specific value:

    incr vibe 10
    Response: 10

###  **DECR key [value]**
Decrements the numeric value of a key by the specified [value]. If the [value] is not provided, it defaults to 1. If the key doesn’t exist, it starts at 0.

Examples: 

Decrement by 1 (default):

    decr counter
    Response: -1

Decrement by a specific value:

    decr vibe 2
    Response: 8



###  **STOP**
Closes the session and disconnects from HippieDB. Use this when you’re done sharing the vibes.

Example: 

    stop
    Response: BYE (and the connection closes)

---

## **Rules of the Road**

- No spaces in keys. Keep it simple, man. Use underscores or dashes if you need separation.
- No permanent storage. HippieDB is ephemeral, like a sunset or a campfire. Enjoy the moment.
- Be cool. HippieDB is shared by everyone, so don’t abuse it. Respect others’ keys and keep the vibes positive.

### Example Session

Here’s what a typical HippieDB session might look like:

```
> set vibe peace
OK

> get vibe
peace

> incr counter
1

> incr counter
2

> decr counter
1

> del vibe
OK

> stop
BYE
```

---

## **Tips for Groovy Development**
- Keep it simple. HippieDB is designed for lightweight, fast, and fun use cases. Don’t overcomplicate it.
- Share the love. If you build something cool with HippieDB, share it with the community. Spread the good vibes!
- Embrace impermanence. If your data disappears, it’s a reminder to live in the now. Let it go, man.

---

## **Running locally**
You can run your own database on your computer or host your own HippieDB Server. See the [hosting guide](host.md) for details.

---

## **Need Help?**
If you’re feeling lost, just take a deep breath and remember: HippieDB is all about simplicity and good vibes. If you still need help, send some positive energy into the universe, and the answers will come. Or, you know, check the FAQ for more groovy guidance.

Now go forth and build something amazing with HippieDB! Whether it’s a collaborative art project, a game, or just a place to store your favorite quotes, HippieDB is here to help you keep the vibes alive. Peace, love, and happy coding! ✌️