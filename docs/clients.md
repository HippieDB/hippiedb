# Clients

Example clients in several languages

## Netcat (nc)
Netcat is a versatile networking tool that can be used to send and receive data over TCP/UDP.

**Send a command and get the response:**

This sends the command SET mykey myvalue to the server and prints the response.

    echo -e "SET mykey myvalue" | nc localhost 1970

**Interactive session:**

This opens an interactive session where you can type commands and see responses in real time.

    nc localhost 1970

**Send multiple commands:**

This sends multiple commands at once and prints the server's responses.

    echo -e "SET mykey myvalue\nGET mykey\nDEL mykey" | nc localhost 1970


---

## Python
Here’s a simple Python script to interact with the server:

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

---

## Node.js

Here's an interactive client in Javascript

    const net = require('net');

    const args = process.argv.slice(2);

    const client = new net.Socket();
    const host = args[0] || 'hippiedb.groovy.ch';
    const port = parseInt(args[1]) || 1970;

    client.connect(port, host, () => {
        console.log('Connected to server');
        console.log("Usage: STOP, GET key, SET key value, DEL key, INCR key [value], DECR key [value]");
        process.stdin.on('data', (data) => {
            console.log("Sending: " + data.toString().trim());
            client.write(data.toString());
        });
    });

    client.on('data', (data) => {
        console.log('Received: ' + data);
    });

    client.on('close', () => {
        console.log('Connection closed');
        process.stdin.destroy();
    });

---

## Kotlin

Here's an interactive client in Kotlin/JVM

    package main

    import kotlinx.coroutines.runBlocking
    import org.slf4j.LoggerFactory
    import java.io.*
    import java.net.*

    object SocketClient {
        private val log = LoggerFactory.getLogger("SocketClient")
        private const val DEFAULT_PORT = 1970

        @JvmStatic
        fun main(args: Array<String>) = runBlocking {
            val host = args.getOrNull(0) ?: "hippiedb.groovy.ch"
            val port = args.getOrNull(1)?.toIntOrNull() ?: DEFAULT_PORT

            try {
                Socket(host, port).use { socket ->
                    log.info("Connected to server")
                    log.info("Usage: STOP, GET key, SET key value, DEL key, INCR key [value], DECR key [value]")

                    val reader = BufferedReader(InputStreamReader(socket.getInputStream()))
                    val writer = PrintWriter(socket.getOutputStream(), true)

                    BufferedReader(InputStreamReader(System.`in`)).use { consoleReader ->
                        while (true) {
                            val input = consoleReader.readLine()
                            if (input == null) {
                                log.warn("Closing connection...")
                                break
                            }
                            writer.println(input)

                            val data = reader.readLine()
                            if (data == null) {
                                log.warn("Connection closed by server")
                                break
                            }
                            log.info("Received: $data")
                        }
                    }
                }
            } catch (t: SocketException) {
                log.info(t.message)
            } catch (t: Throwable) {
                log.error("Error: ", t)
            }
        }
    }

---

## Ruby
Here’s a simple Ruby script to interact with the server:

    require 'socket'

    client = TCPSocket.new('hippiedb.groovy.ch', 1970)
    client.puts "SET mykey myvalue"
    response = client.gets
    puts response
    client.close

---

## Go
Here’s a simple Go code to interact with the server:

    package main

    import (
        "bufio"
        "fmt"
        "net"
        "os"
    )

    func main() {
        conn, err := net.Dial("tcp", "hippiedb.groovy.ch:1970")
        if err != nil {
            fmt.Println("Error connecting:", err)
            return
        }
        defer conn.Close()

        reader := bufio.NewReader(os.Stdin)
        fmt.Print("Enter command: ")
        command, _ := reader.ReadString('\n')

        fmt.Fprintf(conn, command)
        response, _ := bufio.NewReader(conn).ReadString('\n')
        fmt.Print("Response: " + response)
    }

---

## PowerShell
If you're on Windows, you can use PowerShell to interact with the server.

    $socket = New-Object System.Net.Sockets.TcpClient('hippiedb.groovy.ch', 1970)
    $stream = $socket.GetStream()
    $writer = New-Object System.IO.StreamWriter($stream)
    $reader = New-Object System.IO.StreamReader($stream)

    $command = "SET mykey myvalue`n"
    $writer.Write($command)
    $writer.Flush()

    $response = $reader.ReadLine()
    Write-Host "Response: $response"

    $writer.Close()
    $reader.Close()
    $socket.Close()

---

## Bash with /dev/tcp
Bash has a built-in way to interact with TCP servers using /dev/tcp.

    exec 3<>/dev/tcp/hippiedb.groovy.ch/1970
    echo -e "SET mykey myvalue" >&3
    cat <&3
    exec 3>&-

This script:

- Opens a connection to the server.
- Sends a command.
- Reads the response.
- Closes the connection.

---

## Socat
Socat is a powerful networking tool similar to Netcat.

Send a command:

    echo -e "SET mykey myvalue\n" | socat - TCP:hippiedb.groovy.ch:1970

Interactive session:

    socat - TCP:hippiedb.groovy.ch:1970

---