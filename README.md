# Sixty Four Game

SixtyFourGame is a global, on-chain game between 64 GameSquare NFT holders, distributed in an auction. Players belong to 1 of 4 teams and battle to win the pooled SOL from the auction ("The Prize"), winning team take all.

[https://www.sixtyfourgame.com](https://www.sixtyfourgame.com)

- Game board consists of 64 squares
- 4 teams (16 sq/team initially) - Red, Blue, Green & Orange
- Owners of squares can battle neighbouring squares
- If the attacker wins, they are transferred ownership of the losing square NFT,
and the losing square now joins the attacking team
- If the attacker loses, vice versa^
- Players get 100,000,000 'health' before NFT transfer, can attack with any amount
- After square is transferred to new team, gets 100,000,000 'health' restored
- Once any team acquires all squares: ALL funds from auction are shared evenly across all owners of remaining squares

![Sixty Four Game Board](64board.png?raw=true "Sixty Four Game Board")

# Auction
Game board squares will be distributed in an auction, where the 64 highest bidders receive the NFT tokens. Higher ranking is more valuable, due to the game play advantage they have. If you did not receive one of 64 NFTs, you will receive your bid SOL amount back on resolve. GameSquares 1 to 64 must be resolved in order and a tie bid amount goes to whoever submitted their bid first.

![Auction](AUCTION.png?raw=true "Sixty Four Game Board")

![Auction Resolve](AUCTION-RESOLVE.png?raw=true "Auction Resolve")

# Game Play
To begin gameplay, a player must deposit their NFT to the program to 'activate' attacking. A player can withdraw their NFT at any time as long as they are still the current owner. All gamesquare NFTs are not attackable unless 'active'.

Players can battle neighboring squares (north, east, south, west only). Each square has an associated rank/location on the game board. When one square attacks another, the program randomly determines a number from 1 to 100: if the number returned is below a certain defeat-threshold, the defender player's health decreases by the attack-amount. If above the defeat-threshold, the attacker player's health decreases by the attack-amount.

GameSquares have different defeat-thresholds as follows:
- #1 - 4% advantage - Rollunder number = 55
- #2-18 - 3% advantage - Rollunder number = 54
- #19-33 - 2% advantage - Rollunder number = 53
- #34-49 - 1% advantage - Rollunder number = 52
- #50-64 - 0% advantage - Rollunder number = 51

![Game Board](GAMEBOARD-FRESH.png?raw=true "Gameboard")
![Attack](ATTACK.png?raw=true "Attack")

# Goal
Claim all squares of the game - 64 owners of the winning team are returned deposited SOL from auction.

![Game Board Mid](MIDGAME.png?raw=true "Gameboard Mid")

# Showdown (TODO)
To prevent sabotaging the game, ALL squares must become active after 6,000,000 blocks from the end of the auction (~27 days). Otherwise, the square can be purged from the game. Once in Showdown mode, players cannot withdraw their NFT incentivising ending the game.

# Marketplace / Serum DEX
Players can buy or sell game square NFTs in the marketplace for SOL using the Serum DEX: https://dex.sixtyfourgame.com/

![Marketplace](markets.png?raw=true "Marketplace")
![Serum DEX](dex.png?raw=true "Serum DEX")

# SFG Program Instructions:
```
SFGInstruction::InitiateAuction(auction_end_slot)
- Saves the auction_end_slot
- Can only be set once

SFGInstruction::Bid(amount)
- Creates a BidEntry into the AuctionList (SOL lamports amount, pub key)  
- Can only be done if before auction_end_block_number
- Holding SOL funds in treasury

SFGInstruction::CancelBid()
- Cancels a BidEntry in the AuctionList (pubkey of signer only)
- Can only be done if before auction_end_block_number
- Send back SOL funds

SFGInstruction::MintNFT(bidEntryNumber)
- Mints NFT for a BidEntry and sends to saved pubkey
- Can only be done if auction_end_block_number is passed
- Can be done by anyone

SFGInstruction::InitiatePlay(square)
- Deposit NFT to initiate play
- Can only be done by square owner
- Once complete, play is enabled - owner can attack or get attacked

SFGInstruction::EndPlay(square)
- Withdraw NFT from play
- Can only be done by square owner
- Once complete, play is disabled - owner cannot attack or get attacked

SFGInstruction::Attack(fromSquare, toSquare)
- Attacks neighboring square
- Can only be done by fromSquare owner
- If a square's health goes below 0, transfer ownership of NFT to opposing square

SFGInstruction::ClaimPrize(square)
- Once the game is over, send 1/64 of the treasury to the current active player
- Can be done by anyone

```

# Instructions

```
// CLIENT

// install & start client
npm install
npm run start

// PROGRAM

// install program
cd program/
npm install

// remove current config
rm store/config.json
mkdir store

// rebuild program
./do.sh build

// deploy (to testnet)
npm run start

// SERVER

cd server/
npm install
// set mongodb in .env:  ie: `DB = ‘mongodb://localhost:27017/sixtyfourgame’`
node index.js


```
