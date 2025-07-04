# Kima Web3 exercise

This is an exercise for a Web3/full stack interviewee.

Welcome to Kima! If you're asked to do this exercise, it means you passed at least 1 interview successfully, and we're trying to make up our minds 🙂.

If you're the Web3 wizard you claimed to be on the interview, this exercise should be a breeze!

## What are we testing?

This exercise will show us:

1. Your ability to receive and process instructions.
1. Your ability to deploy an app to a cloud platform.
1. Your ability to deliver a projects and explain the results.
1. Your coding capabilities, specifically:
    1. Web2 - frontend, and a little bit of backend.
    1. Web3 - a little bit of Solidity (or Rust - your choice), not too deep.
    1. DevOps - working with a cloud service to deploy your solution.

## What are you building?

You'll be building a Web3 app that allows a buyer to buy a stock with stable coin from a seller, and for a broker to supervise and facilitate the transaction. This scenario simulates one of the most common Kima use cases known as **DvP** (Delivery vs. Payment).

### What will you need

1. Your experience at building web apps.
1. A code editor (VS Code will do, or pick any that you like).
1. A Google Cloud account (you can get one for free with your Google user at [Google Cloud](https://cloud.google.com/)). Google Cloud gives you $300 for free to start, so everything you need will be covered - be sure to take down the app once you've demoed the app, so you won't waste your credits.

### Languages

1. You can use Solidity or Rust for the 2 tokens code.
1. You can use Node.JS with either JavaScript or Typescript for the app.
1. You can use any backend framework you like (like Next, Express, etc.).
1. You can use any frontend framework you like (React preferred).

## Delivery

You'll deliver the exercise in 3 ways:

### Code

1. **Fork this repo**.
1. Write all your code into a single repo (frontend, backend, smart contracts).
1. Feel free to comment, explain what you're doing, or add another README file explaining your approach.
1. Share the repo address once done.

### Deployed app

1. You will deploy the app to Google Cloud.
1. Use Docker and Cloud Run to deploy the app.
1. All 3 routes should be available from the URL you'll share.

### Video

Take a short video fo the app's flow. I recommend using at least 2 different browsers, as both buyer and seller will need to use a wallet to sign a transaction, and wallets like attaching an address to a domain, not a route.

Start by simulating the buyer, then switch to the seller, finally show the broker's dashboard to approve the transaction.

Be sure to record **your entire screen** not just a window, so that the wallet operations will be recorded as well.

## Next steps

When you're ready, share the video, the repo URL and the app URL with the interviewer. We may want to schedule a call and go over the implementation together.

If you have any questions, or comments, please contact your interviewer **BEFORE writing any code** so you won't waste time.

## Preparations

### Blockchain

1. Choose which Testnet blockchain you feel comfortable working with - can be any EVM testnet (Solidity), or Solana DevNet (Rust).
1. Create 2 fungible tokens:
    1. An ERC-20 (or SPL-20) token called "USDX", this will be our "stable coin". Premint million units only.
    1. Another ERC-20 (or SPL-20) token called "MyStock". Premint 5000 units only.
1. 3 blockchain accounts - one for our "buyer", one for our "seller", and a third one we'll call "broker".
    1. Send the "buyer" account a 1,000 units of USDX.
    1. Send the "seller" account all the units of "MyStock".
    1. The broker starts with nothing.
    1. Make sure you add gas tokens to all 3 wallets - you'll need them for the transactions.

### Routes

Our web app has 3 routes:
1. `/buyer` - the buyer interface, allowing a buyer to buy stocks.
1. `/seller` - the seller interface, allowing the buyer to approve sales
1. `/broker` - a sort of dashboard, allowing the broker to see what deals are happening

## Flow

### Personas

We have 3 personas in our demo app. Each will access the app through their own route.

1. A buyer - wants to use stable coins to buy stocks.
1. A seller - wants to sell stocks for stable coins.
1. A broker - facilitates fair transactions by accepting asstes from both buyer and seller, and exchanging them. 

### Buyer flow

1. Buyer logs in to `/buyer` route
    1. Simple login form (fake it - no real functionality needed).
    1. You can use any data store (cloud or local) that works for you for the buyer profile. Keep it SIMPLE.
    1. The buyer will sign his transactions with a web wallet (i.e. MetaMask or Solana wallet), so import the address you chose to MetaMask.
1. The screen shows the buyer the current balance of USDX and MyStock they have in their wallet.
1. The buyer can purchase units of MyStock from the seller.
    1. Assign any value you want to a unit of MyStock - the value is decided in the Web2 the app (no need for Oracles etc.). For example, decide that each MyStock is equal 10 USDX.
    1. The buyer can buy as many units of MyStock as they want, provided they have enough USDX to cover the total.
1. Once the buyer decides how many MyStock units they want, they will click a `submit buy transaction` button.
1. The buyer will use their web3 wallet to send the USDX they owe **to the broker's address**.
1. You'll then show some animation showing that the transaction is executing.
1. At the end of the transaction, both balances (USDX and MyStock) will update for the buyer.

### Seller flow

1. The seller logs in to the `/seller` route (again, fake a login form).
1. They see their Balance of both USDX and MyStock.
1. When the buyer submits a buy transaction (see 4 in user flow), a line appears on the seller's screen showing how many unit the buyer wants, and what's the total USDX value for the transaction.
    1. There are 2 buttons at the end of the row: a red `reject` and a green `accept` buttons.
1. When the seller clicks `accept`, they use their Web3 wallet to send the proper units of MyStock **to the broker's address**.
1. You'll then show some animation showing that the transaction is executing.
1. At the end of the transaction, both balances (USDX and MyStock) will update for the buyer.

### Broker flow

1. The broker logs into the `/broker` route (again, fake login form).
1. They see a list of transactions they need to broker. A transaction will have the following fields:
    1. Buyer address
    1. Buyer offered USDX amount
    1. Buyer funds received?
        1. This field will either show "no", or the hash of the transaction that the buyer used to send USDX to the broker.
        1. The hash should be a link to a blockchain explorer showing the transaction onchain.
    1. Seller address
    1. Seller offered MyStock amount
    1. Seller funds received?
        1. This field will show "no" if no funds were received, or the hash of the transaction that the seller used to send MyStock to the broker.
        1. The hash should be a link to a blockchain explorer showing the transaction onchain.
    1. An `execute` button that is initially disabled, and becomes enabled *only when* both buyer and seller funds have been received.
    1. Transaction status: will be "pending" or "finalized".
1. When the broker clicks `execute`:
    1. The Broker sends the MyStock units they received from the seller to the buyer address.
    1. The broker sends the USDX units they received from the buyer to the seller.
    1. When both transactions finish, the 2 hashes of the transactions are shown (each as a link to a blockchain explorer).
    1. The transaction status changes to "finalized".

### End result

At the end of a transaction, the expected result is this:

1. The buyer has less USDX and more MyStock (the right amounts, of course)
1. The seller has less MyStock and more USDX (again, right amounts)
1. The broker sees a `finalized` transaction, and links to 4 transactions in their dashboard:
    1. Buyer to broker USDX
    1. Seller to broker MyStock
    1. Broker to buyer MyStock
    1. Broker to seller USDX


**Good luck!**
