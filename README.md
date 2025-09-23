# Casino Management DSL

## Domain Description

This Domain Specific Language (DSL) is designed for managing casino operations. The main entities in this domain are:

- **Players**: Casino patrons who place bets and manage their account balances
- **Games**: Different types of casino games (Roulette, Blackjack, Poker, etc.)
- **Tables**: Physical or virtual gaming tables where games are played
- **Bets**: Wagers placed by players on various outcomes
- **Rounds**: Game sessions that can contain multiple bets and sub-rounds
- **Dealers**: Casino staff who operate the games

The domain includes recursive elements through:
1. **Bet recursion**: Complex bets can have parent-child relationships (e.g., main bet with side bets)
2. **Round recursion**: Gaming rounds can contain sub-rounds (e.g., main round with bonus rounds)

## BNF Grammar

```bnf
<command> ::= <add_player> | <add_game> | <add_table> | <place_bet> | <add_round> | 
              <resolve_bet> | <add_dealer> | <deposit> | <withdraw> | <set_limit> | 
              <show_command> | <remove_player> | <dump_examples>

<add_player> ::= "add" "player" <integer> <string> <double>

<add_game> ::= "add" "game" <integer> <string> <game_type>

<add_table> ::= "add" "table" <integer> <string> <integer> <double> <double> ["dealer" <integer>]

<place_bet> ::= "place" "bet" <integer> "player" <integer> "table" <integer> 
                "amount" <double> "type" <bet_type> ["parent" <integer>] "round" <integer>

<add_round> ::= "add" "round" <integer> "table" <integer> ["parent" <integer>] ["status" <round_status>]

<resolve_bet> ::= "resolve" "bet" <integer> <bet_outcome>

<add_dealer> ::= "add" "dealer" <integer> <string> "table" <integer>

<deposit> ::= "deposit" "player" <integer> "amount" <double>

<withdraw> ::= "withdraw" "player" <integer> "amount" <double>

<set_limit> ::= "set" "limit" "player" <integer> <limit_type> <double>

<show_command> ::= "show" ("players" | "games" | "tables" | "bets" | "rounds")

<remove_player> ::= "remove" "player" <integer>

<dump_examples> ::= "dump" "examples"

<game_type> ::= "Blackjack" | "Roulette" | "Poker" | "Baccarat" | "Slots"

<bet_type> ::= "Straight" | "Split" | "Corner" | "Red" | "Black" | "Odd" | "Even" | "Pass" | "DontPass"

<bet_outcome> ::= "win" <double> | "lose" | "push"

<round_status> ::= "Active" | "Finished" | "Cancelled"

<limit_type> ::= "DailyLimit" | "WeeklyLimit" | "MonthlyLimit"
```

## Command Examples

### 1. Basic Player and Game Setup
```
add player 1 "John Smith" 1000.0
add game 1 "European Roulette" Roulette
add dealer 1 "Maria Garcia" table 1
add table 1 "High Roller Roulette" 1 100.0 5000.0 dealer 1
```

### 2. Complex Betting with Recursion
```
add round 1 table 1 status Active
place bet 1 player 1 table 1 amount 500.0 type Red round 1
add round 2 table 1 parent 1 status Active
place bet 2 player 1 table 1 amount 100.0 type Odd parent 1 round 2
place bet 3 player 1 table 1 amount 200.0 type Straight parent 1 round 1
```
This example shows recursive betting where bet 2 and 3 are components of the main bet 1, and round 2 is a sub-round of round 1.

### 3. Bet Resolution and Account Management
```
resolve bet 1 win 1000.0
resolve bet 2 win 200.0
resolve bet 3 lose
deposit player 1 amount 2000.0
set limit player 1 DailyLimit 10000.0
```

### 4. Administrative Operations
```
show players
show tables
withdraw player 1 amount 500.0
remove player 1
dump examples
```

## Recursive Elements Demonstration

The most interesting recursion case is the **compound betting system**:

1. A player places a main bet (e.g., Red in roulette)
2. They can place side bets that are children of the main bet (e.g., betting on Odd numbers while the main bet is on Red)
3. Additional component bets can be placed (e.g., straight number bet as part of the same betting strategy)
4. Rounds can also be recursive, with sub-rounds containing related bets

This recursion allows for complex betting strategies where multiple related bets form a hierarchical structure, making the casino system more realistic and flexible.
