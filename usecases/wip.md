# Workflows
> [Back](../specification.md)

(This probably should be nice graphics but well ... here are some lists ...)

## Party creation and joining
> A User creates a new party, invites friends which join the party and starts the game.

- User1 sends `CreatePartyEvent`
    - Server creates a new Party (Local container in which all websocket connections are hold which belong to a party)
    - Server sends `PartyCreatedEvent` to User1
- User1 receives `PartyCreatedEvent` 
    - User1 stores JWT locally
- User1 shares partyId with User2
- User2 joins party with `JoinPartyEvent` with partyId and a username
    - Server recvieves event and creates a JWT for the user
    - Server puts the connection into the container assicated with the party
    - Server sends `PartyJoinedEvent` to User2
- User2 recvieves `PartyJoinedEvent`
    - User2 stores JWT locally
    - User2 waits for the game to be started
- Server sends a `PartyPlayersListEvent` event to all players in a party



## Start a new Game for the Party
> Continuuation of "Party creation and joining" with User1 and User2

- User1 start the game with `StartGameEvent`
    - Server creates a new GameState for the current party.
    - Server sends a `VoteForEvent` to all Players with the Options to vote for each player
- Players recieve a `VoteForEvent` 
    - Player selects an options
    - Player sends the option to the server via the `VoteEvent`
- Server recieved a `VoteEvent` from each user
    - Server decides which user is allowed to start or triggers a new vote (Exact mechanism TDB)
- Server sends a `GameStateEvent` for the new Game with the voted player

# Player turn
> 

- Player recieves a `GameStateEvent` where the "activePlayer" object is the Player
- Player selects cards to put on the stacks
    - Player sends `PlayCardsEvent` with the cards and the corresponding stacks
- Server receives `PlayCardsEvent`
    - Server validates the JWT and checks if the player has the turn
    - Server calculates new GameState
    - Server sends new `GameStateEvent` with new active player
- Player recvieves new GameState and waits ...


# Player has no cards left
- Player recieves a `GameStateEvent` where the "activePlayer" object is the Player
- Player selects cards to put on the stacks
    - Player sends `PlayCardsEvent` with the cards and the corresponding stacks
- Server receives `PlayCardsEvent`
    - Server validates the JWT and checks if the player has the turn
    - Server calculates new GameState
        - Server notices empty drawStack and no cards left in player hand
        - Server send `PlayerWonEvent` to all Players
    - Server sends new `GameStateEvent`


# Player cant play anymore
- Player recieves a `GameStateEvent` where the "activePlayer" object is the Player
- Player selects cards to put on the stacks
    - Player sends `PlayCardsEvent` with the cards and the corresponding stacks
- Server receives `PlayCardsEvent`
    - Server validates the JWT and checks if the player has the turn
    - Server calculates new GameState
        - Server notices Player cant play any cards
        - Server send `PlayerLostEvent` to all Players
    - Server sends new `GameStateEvent`

# Player makes an invalid move
 Player recieves a `GameStateEvent` where the "activePlayer" object is the Player
- Player selects cards to put on the stacks
    - Player sends `PlayCardsEvent` with the cards and the corresponding stacks
- Server receives `PlayCardsEvent`
    - Server validates the JWT and checks if the player has the turn
    - Server notices an invalid move (eg. only one card played while drawStack > 0)
    - Server sends a `InvalidPlayError` to Player
    - Server waits for new Play from user ...


# Player sends a chat message
- Player sends a `SendMessageEvent`
- Server receives a `SendMessageEvent`
    - Server validates the JWT
    - Server checks if the message contains any illigal characters (see #2)
    - Server creates a `MessageEvent` with the username in the JWT
    - Server sends the event to all users in the party


