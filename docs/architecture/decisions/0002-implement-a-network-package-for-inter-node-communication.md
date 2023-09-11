# 2. Implement a Network Package for Inter-node Communication
Date: 2023-09-11

## Status
Accepted

## Context
We have a distributed system where multiple instances of our application, each running in a separate environment (possibly a Docker container or a Kubernetes pod), need to communicate with each other. The communication should be reliable, and the system should allow users to define their own hooks for handling incoming messages based on the message type.

## Decision
We will implement a Go package network which will contain the necessary components for network communication. This package will have the following key components:

**Message**: This struct will represent a message that can be sent between nodes. Each message will have an 'ID', 'Content' (in bytes) and 'Type' fields. The 'Type' field will be used to determine how the message is processed upon receipt.
```go

type MessageType string

type Message struct {
	ID      string
	Content []byte
	Type    MessageType
}
```
**TCPCommunicator**: This struct will handle the sending and receiving of messages via TCP. It will hold a map of message types to user-defined functions (hooks) that handle incoming messages of each type.
```go
type MessageHook map[MessageType]func(msg Message)

type TCPCommunicator struct {
	LocalAddress string
	MessageHook  MessageHook
}
```

**Node**: This struct will represent a node in the network. Each node will have its own communicator that knows how to send/receive messages.

```go
type Node struct {
	ID      string
	Address string
	Comm    *TCPCommunicator
}
```

Users will define their own hooks for handling incoming messages. These hooks are functions that take a Message as an argument and don't return anything.

Example of defining hooks:

```go

hooks := network.MessageHook{
	"text": func(msg network.Message) {
		fmt.Printf("Received text message with ID %s: %s\n", msg.ID, string(msg.Content))
	},
	"image": func(msg network.Message) {
		// handle image bytes
	},
}

node := network.NewNode("1", ":3000", hooks)
```

## Consequences

Users can define their own logic for handling different types of incoming messages by providing hooks when creating a node.
The use of TCP ensures that messages are reliably delivered.
The use of message types allows different types of messages to be handled differently.
The design is flexible and can be extended with additional functionality as needed.
The use of a message ID will allow for easier debugging and tracing of messages.
The change of message content to bytes allows more versatility in the type of data that can be sent.