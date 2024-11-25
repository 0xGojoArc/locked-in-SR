##### [[Foundry]]

- Forge, anvil, and cast are the main commands of the foundry.
    
- Forge is used for compiling and interacting with our contracts
    
- Cast is used for interacting with our contracts that have already been deployed.
    
- Anvil is used to deploy a local blockchain, it spins up a local anvil blockchain 
    
- Use cast command to convert hex to numbers
    
- Nonce: a number that’s only used once. It is the count of tx, including contract deployment(this is also a tx). It increments with each tx.
    
- Whenever we send metamask tx, we make an HTTP post req to RPC URL(node provider)
    
- We can write script in [[Day 02/Cyfrin Updraft/Solidity]] to deploy our contracts
    
- Private key should not saved anywhere, use keystore.
    
- Running forge test filename –vv command gives visibility of what’s happening
    
- –vv logs any console log statements in the test functions
    
- -vvv shows the stack trace
    
- When we run a test without specifying the chain, the foundry will spin up the anvil chain and destroy it afterward. This may cause the test to fail bc it will make a call to that chain and return nothing, so always specify some node provider RPC URL eg; alchemy
    
- When you are working with addresses outside the system, we can provide –fork-url or –rpc-url for the unit test part of the code, so anvil will spin up but it will take a copy the sepolia rpc-url and will simulate all our tx as if they are running on sepolia chain as opposed to a blank chain of anvil.
    
