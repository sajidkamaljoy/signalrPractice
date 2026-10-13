# signalrPractice
[![CI](https://github.com/sajidkamaljoy/signalrPractice/actions/workflows/ci.yml/badge.svg)](https://github.com/sajidkamaljoy/signalrPractice/actions/workflows/ci.yml)

A real-time chat built to practice ASP.NET Core SignalR, with a web client and a desktop client talking to the same hub.

## Projects

| Project | Description |
|---|---|
| `signalr` | ASP.NET Core Razor Pages app (.NET 8) that hosts the SignalR hub at `/chatHub` and a browser chat page (`wwwroot/js/chat.js`) |
| `WPFClient` | WPF desktop client that connects to the hub with `Microsoft.AspNetCore.SignalR.Client` and automatic reconnect |

## Hub features (`ChatHub`)

- Announces new connections to everyone.
- `SendMessage` broadcasts a message to all connected clients.
- `SendPrivateMessage` sends a message to a specific user.
- `JoinGroup`, `LeaveGroup` and `SendGroupMessage` provide group chat rooms.
- Clients receive messages through a single `ReceiveMessage(user, message)` callback (typed in `IChatClient`).

## Run it

1. Install the .NET 8 SDK.
2. Start the server:

```bash
cd signalr
dotnet run --launch-profile https
```

   It listens on `https://localhost:7187`.
3. Open that URL in one or more browser tabs to chat.
4. To use the desktop client (Windows only), keep the server running and, in a second terminal:

```bash
cd WPFClient/WPFClient
dotnet run
```

   `WPFClient` is not part of `signalr.sln`; you can also open `WPFClient/WPFClient/WPFClient.csproj` in Visual Studio. The client expects the hub at `https://localhost:7187/chatHub`, so change the URL in `MainWindow.xaml.cs` if your port differs.

## Notes

- `SendPrivateMessage` uses `Clients.User(...)`, which targets authenticated user IDs. With no authentication configured, it won't reach anyone. Add authentication or use connection IDs to make private messages work.
