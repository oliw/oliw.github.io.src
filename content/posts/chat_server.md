+++
title = "Building a Chat Server in Ruby"
date = "2021-04-27T00:28:56-08:00"
categories = ["Coding"]
summary = "How to make a Chat Server in Ruby using the TCP protocol"
+++

In this blog post I describe how to build a chat server in under 100 lines of code!

## Background

When two computers want to talk to one another, they do so over a network. How they communicate can be described at different levels of abstraction.

- The highest-level of an abstraction, is the application layer e.g. HTTP or SSH.
- The lowest-level of the abstraction, is the physical layer e.g. how to transfer a single bit of information over a copper wire.

Somewhere in the middle of that stack is the transport layer. The transport layer is responsible for packaging data from the application and ensuring it reaches the counterpart application on the other computer. The transport layer doesn't care whats in the data being sent, just that there is a some data that needs to be delivered to an application.

There are different protocols in the Transport layer, but the two we hear about the most are `TCP` and `UDP`. `UDP` is used for things like video calls, and doesn't guarantee the delivery of a packet. In the case of a video call, this is fine. `TCP` is a more elaborate, 'expensive' protocol, but ensures the in-order delivery of a packet.

For a chat app, we want to use `TCP` because we want our messages to be sent in their entirety with nothing dropped along the way.

## Setting up the Server

Like most languages, ruby comes with a few standard libraries. One of which is the `socket` library, which allows you to start a TCP Server that establishes connections to clients reaching out via a specified port.

As per [the documentation](https://ruby-doc.org/stdlib-2.4.0/libdoc/socket/rdoc/TCPServer.html), here how to build a script that runs a TCP Server.

```ruby
require 'socket'

server = TCPServer.new 8090

loop do
    new_client = server.accept
    Thread.start(new_client) do |client|
        # Do things
    end
end
```

- The `loop` ensures that our script runs forever until interrupted by a CTRL-C signal.
- Our server will begin as a single 'main' thread, whose job will be to initialize a `TCPServer` object and then call `server.accept` repeatedly.
- `server.accept` is a blocking function that will hang until a new client establishes a connection with us. When the connection is established it will return a `TCPSocket` object representing the connection with the client.
- We want multiple clients to be able to talk to our server at the same time, therefore we'll want each client to be handled with its own thread.
- To create a new thread our 'main' thread will call `Thread.start` and pass in the newly instantiated TCP object. The `Thread.start` call will be accompanied with a block.
- The code written inside the block is executed by the new thread and not the main thread.
- The newly started thread will execute the contents of the block and then be destroyed.

## Chatting to a Client

The code for sending and receiving messages happens inside the thread's block.

```ruby
    Thread.start(new_client) do |client|
        # Code for sending and receiving messages to a client goes here
    end
```

To receive a message from a client we call

```ruby
    msg = client.gets
```

This is a blocking call, just like `server.accept`, and will only return when the client has sent a message. `#gets` is a method available on the socket and is available on most IO streams.

`#gets` works by reading a "line"; i.e. a sequence of data up to and including a new-line character e.g. `\n`.

If the client decides to close its connection it will send a special End-Of-File character. When `#gets` is called and that character is read, `#gets` will return `nil` and we'll know that the thread can be destroyed.

To send a message to a client we call `#puts` which works the same way in reverse.

```ruby
    client.puts "Hello!"
```

## Putting it all together

Now we know

- how to set up a server
- how to spawn a thread for each new client connection
- how to receive messages from the client
- how to send messages to the client

With these four pieces we can build a pretty effective chat server!

```ruby
require 'socket'

server = TCPServer.new 8090

connected_clients = []

ConnectedClient = Struct.new(:socket, :username)

def valid_nickname?(nickname, connected_clients)
    connected_clients.none? {|client| client.username == nickname}
end

def speak(msg, client, all_clients)
    all_clients.each do |other_client|
        next if other_client == client
        other_client.socket.puts msg
    end
end

loop do
    Thread.start(server.accept) do |client|
        client.puts "Welcome to my chat server! What is your nickname?"

        nickname = client.gets
        next if nickname.nil?

        # Check if nickname is valid
        nickname = nickname.chomp
        while !(valid_nickname?(nickname, connected_clients)) do
            client.puts "Sorry that username is already taken, please choose another"
            nickname = client.gets
            next if nickname.nil?
            nickname = nickname.chomp
        end

        connected_client = ConnectedClient.new(client, nickname)

        # Tell this client the names of the other clients
        other_client_names = connected_clients.map(&:username)
        client.puts "You are connected with #{connected_clients.length} other users: [#{other_client_names.join(',')}]"

        # Tell the other clients the name of this client
        speak("*#{nickname} has joined the chat*", connected_client, connected_clients)

        connected_clients << connected_client

        # Listen for input
        while line = client.gets
            line = line.chomp
            speak("<#{nickname}> #{line}", connected_client, connected_clients)
        end

        # Tell the other clients that this client has left
        connected_clients.delete(connected_client)
        speak("*#{nickname} has left the chat*", connected_client, connected_clients)
    end
end
```

{{< figure src="/tcp_server_demo.gif" >}}
