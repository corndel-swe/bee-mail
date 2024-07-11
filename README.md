<p align="center">
  <img src="logo.png" style="width: 200px; border-radius: 16px;" alt="Bee Mail Logo">
</p>

# BeeMail

Welcome to BeeMail, the buzziest messaging service in the world!

There's one problem, though... none of the code has been written yet.

Your challenge, if you choose to accept it, is to use design patterns to
implement BeeMail.

## Getting started

This project can be completed in any language that supports classes.

To get started:

1. Create a project in your language's project framework

2. Read through the project requirements - it's up to you how to implement them

## Requirements

To begin with, you should create three classes: `App`, `User` and `Message`.

### App

```mermaid
classDiagram
  class App {
    -User[] users
    +addUser(user: User)
    +findUser(userId: str)
    +deliverMessage(senderId: str, recipientId: str, content: str)
  }
```

- `users` is an array of all users who have logged into the app

- `addUser()` handles adding a new user to the `users` array

- `findUser()` gets the user with the given id from the `users` array, if
  possible

- `deliverMessage()` uses the given information to create a new instance of
  message, and delivers it to the recipient

### User

```mermaid
classDiagram
  class User {
    -string id
    -string username
    -Message[] inbox
    -App app
    +login(app: App)
    +sendMessage(recipientId: str, content: str)
    +receiveMessage(message: Message)
    +readMessage(idx: int)
  }
```

- `id` is a unique string which identifies the user

- `username` is the user's username

- `inbox` is a list of messages the user has received

- `app` is the instance of `App` the user is logged into

- `login(app)` adds the user to `app.users` and also sets `user.app` to `app`

- `sendMessage()` creates a new message and adds it to the `messages` array of
  the recipient user

- `receiveMessage()` handles adding an incoming message to the `messages` array

- `readMessage()` logs the message at the given index in `messages` to the
  console, and marks the message as read

### Message

```mermaid
classDiagram
  class Message {
    -str id
    -datetime timestamp
    -str content
    -User from
    -User to
    -bool delivered
    -bool read
    +log()
    +markDelivered()
    +markRead()
  }
```

- `id` is a unique id which identifies the message

- `datetime` is the time at which the message was created

- `content` is the text content of the message created by the sender

- `from` is the instance of `User` that sent the message

- `to` is the instance of `User` that receives the message

- `delivered` is true once the recipient has received the message

- `read` is true one the recipient has read the message

- `log()` prints the message details to the console in a readable format

- `markDelivered()` sets `delivered` to true

- `markRead()` sets `read` to true

## Sequence diagram

This diagram should give you a better idea of how the app is intended to work:

```mermaid
sequenceDiagram
    participant User
    participant App
    participant Recipient as Recipient User
    participant Message

    User ->> App: login(app)
    App ->> User: set app

    User ->> App: addUser(user)
    App ->> App: add user to users array

    User ->> User: sendMessage(recipientId, content)
    User ->> App: deliverMessage(senderId, recipientId, content)

    App ->> App: findUser(recipientId)
    App ->> Recipient: deliverMessage(content)
    App ->> Message: createMessage(sender, recipient, content)

    Message ->> Recipient: receiveMessage(message)
    Message ->> Message: markDelivered()

    Recipient ->> Recipient: readMessage(idx)
    Recipient ->> Message: markRead()
    Recipient ->> Message: log()
```

## Extensions

- **Group messaging**: Add a feature which allows user groups to be created. A
  user can send a message to the group, and it will be delivered to all
  recipients in that group.

- **Sent messages**: Create a "sent messages" folder which allows users to keep
  a copy of all the messages they have sent.

- **Read receipts**: When the recipient reads a message, can you ensure that the
  message in the "sent messages" of the sender is also updated to `read: true`?

- **Different message protocol**: Let's suppose BeeMail wants to integrate with
  a different messaging service, and so we have an `ExternalMessage` class which
  has `body` instead of content, no `timestamp` and the `markRead` method is
  actually a `toggleRead` method which flips the `read` boolean. Implement this
  class, then create an adapter class which wraps it to make it compatible with
  the regular `Message` interface.

## Questions

- Can you make use of the singleton pattern to ensure only one `App` can be
  created?

- Is the mediator pattern being used here? Explain how.

- What would BeeMail look like if we didn't use the mediator pattern? What would
  be the pros and cons?

- Are any other patterns being used in BeeMail? Explain.

- Think back through bootcamp and any work you've done for your employer. Have
  you seen or used any patterns? Explain.
