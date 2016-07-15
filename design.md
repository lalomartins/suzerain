# Suzerain design

## Requirements

requirements:

1. Web (play anywhere).
2. VASSAL package compatible.
3. A group installs it on their own server, and manages packages by just dropping them on a server directory.
4. Good enough on smartphones (app optional).
5. Persistent (real-time play isn't mandatory; if all players disconnect, the game stays there waiting).
6. Players are tied to server accounts. You don't have a password for each game, just an account on each server. Then there's a dashboard with the games you're in at the moment, the ability to create one, etc.
7. Notifications. (But what and how?)

non-requirements (for now at least):

- Package editing.
- User account management.
- Package management beyond install/uninstall on server.
- Scripting (groups can use Greasemonkey if they want).

open:

- How to create accounts? Can anyone just sign up for any server? Or does an admin have to create the account? If so, how? Web UI or text (or json, or yaml) file on server?
- Can any user join any game before it's started?

## Client

React. Any UI framework? MaterialUI? Which has most momentum ATM?

Redux for data model.

## Client-server

Basically three options:

- REST. Needs a way to get notified of changes by the server; can be polling or persistent connection.
- Real-time API through websockets — but don't invent a protocol, look for a framework with momentum, ideally one that works well with Redux.
- PouchDB replication. My (Lalo's) favourite because it's great for offline (client-side persistence is built-in), but needs thinking whether it's sufficient for talking *to* the server; may need an extra “RPC” layer.
  - If extra RPC is needed, it can be just REST calls.
  - Another option for extra RTC is to use a pouchdb document, or a series of them, as a message queue.

## The server

- Language and framework TBD.
  - Python: fastest dev for @mawkee, @lalomartins has 20ish years experience, everyone else was learning anyway.
  - JS: since the client is in JS, everyone *has* to know (or learn) it already.
  - Rust: shiny new toy, seems worth learning 😉 also crazy good performance/resources ratio.
- Read game and expansion definition packages.
- Create new games.
- Run any player-independent tasks that need to be run in a game, assuming that's a thing.
- Implement client-server communication as above.
  - If REST, provide the REST server and implement endpoints.
  - If websockets, same.
  - If Pouch, control replication so users can't see data they shouldn't (other people's hands, passwords).
