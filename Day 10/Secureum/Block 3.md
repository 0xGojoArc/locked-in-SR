##### [[Block 3]]


 #41
 ###### Signature purpose
 - 3 purposes
	 1. Authorization - to spend ether or execute contract that it's targeted
	 2. Non-repudiation - once sign included, later can't be denied 
	 3. Integrity - Tx data has not been modified or can not be modified after Tx has been signed

###### Contract Creation
- transaction can result in contract creation
- when a contract creation tx is sent to a special address called, zero address. it has zero in all its bits
- it contains data payload, represents bytecode of contract that is being created
- also contains optional ether amount, new contract created will have this as starting balance

###### Transactions Vs. Messages
- Tx originates Offchain, targets OnChain
- Msg originates OnChain, targets OnChain
- Tx produced by EOA
	- It can either trigger Msg to another EOA => transfer of value/ether
	- Ot it can trigger Msg to Contract acc => running contract code 

- Msg can be triggered in two ways
	- externally by Tx => destination could be  EOA or contract acc
	- Internally within the EVM => when smart contract executes the call family of Opcodes => runs code or value transfer to recipient

###### Transactions in Blockchain
- Txs grouped together and included within a Block
- Blocks are linked together, linked Blocks form the Blockchain

###### Block
- set of txs grouped together
- every block container hash of previous block
- change in the block would affect all the following blocks
- to preserve txs history, blocks are ordered
- Txs within the block are also ordered
- these apects is immutability and integrity of the blockchain is maintained.

###### Ethereum Node and Client
- Node is a software application the impl Eth Protocol Specs
- It communicates with other nodes on the network in P2P fashion
- Client is Specific Impl of Eth Node
- Clients like Geth, Nethermind, OpenEth, Erigon
- The txs are sent to nodes, and these nodes broadcast across P2P network

###### Ethereum Miners
- Entities running Eth Nodes
- They receive the txs, validate them, execute them and combine them into blocks
- They provide PoW(before PoS), for all these work if miner's block chose to be part of blockchain
- they get rewarded a block reward.
- they also get txs fees -> ether spent on gas by all txs included in that block

###### Block Gas Limit
- It's different from gasLimit
- It refers to Gas Limit of that block, the total gas spent in all txs in block
- It caps the no. of txs that can be included in block
- The block size is fixed in terms of gas used in all txs, not the fixed no. of txs.
- The reason being every tx can consume different amount of gas
- this block gas limit is set by miners
- This changes depending on miners voting

######  GHOST
- Many miners produce valid blocks at any given point of time, Choosing one Block is done by GHOST protocol
- Greedy Heaviest Observed Subtree Protocol (GHOST)
- This protocol allows stale blocks to upto 7 levels, these stale blocks are called Ommers.

###### Consensus
- It was Nakamoto Consensus before PoS, adapted from Bitcoin. Nakamoto Consensus addresses which miner's block to be included next to create canonical chain. And it depended on PoW(PoS now) and Longest chain rule
- In the current **PoS Consensus mechanism**, it adopts a more collaborative of staking and validator participation.
- In PoS, consensus in achieved through validator attestations. 
- validators must agree on the validity of blocks through voting, requiring at least two-thirds agreement among active validators.

###### Ethereum State
- It's a mapping from Address of Eth Account to the State contained within it
- to store these transactions within a block.
- The data structure impl is modified Merkle-Patricia tree, combination of Merkle and Patricia tree with changes specific to Eth
- Merkle Tree is type of binary tree, 
	- composed of set of nodes, leaf nodes at the bottom of the tree contain the underlying data
	- The intermediate nodes, between leaf nodes and root, contain the combined hash of their two child nodes
	- txs can be accessed by traversing through these hash pointers, if any content is chnged in the tx, it reflects on the hash tored in the parent node, which in turn affect the hash in upper-level node and so on until the root is reached.
	- the merkle root stored in the block header makes txs tamper-proof and validates integrity of data
	![[merkle-tree.png]]

- **Patricia tree** (Practical algorithm to retrieve information coded in alphanumeric) tree, also called prefix tree or radix tree or trie
- more space-efficient because they combine common prefixes.
- This means that if two strings start with the same letters, the Patricia Tree will store that prefix only once, saving memory
- In Ethereum, there are three main types of data stored in these Merkle-Patricia Tree ([[MPT]]) trees:

	1. **State Trie**: Stores account states (e.g., balance, nonce, contract code).
	2. **Transaction Trie**: Keeps records of all transactions in each block.
	3. **Receipt Trie**: Stores transaction receipts, which contain info about the execution of transactions (e.g., gas used, logs).
- ![[merkle.png]]
	- **Store Shared Prefixes Efficiently**: Accounts with common prefixes (`0xab`) share nodes in the tree, saving space.
	- **Branch Only Where Necessary**: Only where accounts differ in IDs does the tree create branches, reducing the total number of nodes.
- In Eth, it uses 16 children fir each node.

###### Block Header
- Every block has Header, Txs, Ommers' Headers
- Block Header has multiple things;
	- parentHash
	- OmmerHash
	- beneficiary
	- stateRoot
	- transactionRoot
	- receiptsRoot
	- logsBllom
	- difficulty
	- number
	- gasLimit
	- gasUsed
	- extraData
	- timeStamp
	- mixHash
	- nonce
- parentHash: it's the hash of the parents blocks header, this is what chains the blocks, it make it immutable
- beneficiary Addr: Addr of eth acc to which the block rewards for mining this blck and txs fees collected from the mining of the txs included in this blocks. addr controleed by miner that produced this block.
-  Roots hashes of [[MPT]] includes; stateRoot, transactionRoot, and receiptsRoot
- number: block number equal to number of blocks that are mined so far
- gasLimit: Block gasLimit, no txs that can be included in this block
- gasUsed: total gas used in all the txs used in this block
- timeStamp: unix time, when was the block mined
###### State Root
- 256-bit hash of modified MPT
- Leaves are Key(Addrs)-Value(Eth Acc State) pairs
- Each acc has nonce, balance, codeHash and storageRoot.
- codeHash and storageRoot 
	- are empty in case EOA, 
	- but in Contract Acc, codeHash has keccak 256 hash of the code and storageRoot has rootHash of another MPT, where leaves repr storage that is associated with that contract.
###### Transaction Root
- MPT, leaves repr the txs
###### Tx Receipt
- The side effects of the tx that are captured on the blockchain besides the account state change that tx makes
- It's a Tuple, contains four items:
	1. **Cumulative Gas used:** Total gas used in the block until the right after this tx happened
	2. **Set of Logs:** 
		- Events generated by any tx that are captured on blockchain, crictical off-chain components like UI monitor what's happening with smart contracts.
		- There's an indexed parameters for every event so that one can query particular parameter of any event much faster manner
	4. **Bloom filter:** The above are enabled by this bloom filter that is associated with those particular logs
	5. **Tx Status Code:** Tx Status
###### Tx Gas
- Beneficiary i.e. Miners gets all the txs fees of the block
- Gas Refund i.e. `Tx gasLimit - Gas Used` unused gas is sent back to sender of tx, done at the same gasPrice that was indicated in the tx.


###### [[Day 04/EVM]]
- Ethereum Virtual Machine
- its a runtime env, all the smart contracts are executed
###### Ethereum Code
- Low level stack based EVM machine code
- This code consists of series of Bytes called Bytecode
- Every Byte is a single operation
- Each Op code is a single byte.
###### EVM Architecture
- Stack-based architecture
- ![[evm-.webp]]
- EVM instruction operate on the stack, operands for those instructions are placed on the stack, and the output of those instructions are also returned on the stack.
- Architecture has four fundamental components
	1. Stack 
	2. Volatile Memory
	3. Non-volatile Storage
	4. Calldata
- volatile and non-volatile refers to persistence of the data that is placed within the memory or store
- word size is 256-bits, it was chosen to facilitate some of the fundamental operations of Keccak-256 hash and [[ECC]]
###### Stack
- 1024 elements in the stack
- Each of those elements are 256-bits in length
- EVM instructions are allowed to operate with top 16 stack elements
- Top 16 elements are stack specific operations like PUSH/POP/SWAP/DUP