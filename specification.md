# RFC Websocket Specification Proposal - 0.0.1

> A websocket only specification.

# TOC
- [General](#general)
- [Authentication](#Authentication)
- [Event base type](#event-base-type)
- [API](./api/)
    - [Party API](./api/party-api.md)
    - [Vote API](./api/vote-api.md)
    - [Game API](./api/game-api.md)
    - [Chat API](./api/chat-api.md)
    - [Errors](./errors.md)
- [Workflows](./usecases/wip.md)
- [TODOs](#todos)


# General

The API is versioned by the Websocket path by the major version identifier with the prefix `ws/v`. Meaning, since this is Version `0.0.1` the major verion of the specification is 0. Resulting in a websocket path `ws/v0`. This means a valid websocket url for this api version would be something along the lines `ws://super-host.dev/ws/v0`.

## MessageID's
Message IDs are used to identify a Request-Response Pattern for specific events and reference events which caused errors.
All message ids are UUIDs (v4).

Example of a case where the message-id is userfull: 
> TODO: We need a better example case here since this will not happen/lead to problems
- Player plays cards and frontend sents the event and expects a response
- Frontend gets a Message event instead, which it has to handle
- Frontend receives the response for the Play event.

# Authentication
To make sure no user can immitate another user, specific actions are secured by a simple "authentication". This authentication is not permanant and should not require an account or anything else besides a username. To acomplish this we use [JWT](https://jwt.io/introduction). A Token is created when a user joins a party and is used to authenticate chat messages and game actions. The API Calls, which should be checked by the server are marked with `Authentication needed`. 

### JWT Payload
| Property | Type | Description |
| ---      | ---  | ----        |
| username       | string | Username |
| partyId     | string | Identifier of the party |


# Event base type
> The event base type is the form in which all events are sent

| Property | Type | Description |
| ---      | ---  | ----        |
| id       | string | UUID identifier
| type     | string | Identifier of the type of event. An event type consists of the scope of the event and the eventname itself seperated by a `/`. Example: `PARTY/CREATE_PARTY` |
| data     | EventData | `Optional` Containts additional information about the event |

# Timeouts
> TODO

# TODOs
- Workflows / Usecases need overhaul
- Check for typos / inconsistencies
- Timeouts
- Addidtional Error cases
    - Vote Timeout / Invalid vote











