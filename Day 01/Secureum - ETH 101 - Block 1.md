#### Properties

- ###### Permissionless Apps
	- no need register or to get permission from any entities unlike centralised .
- ###### Built-in Economics(crypto economics)
	- incentivises people to run nodes, deploy and use applications.
- ###### High Availability and High Auditability 
	- 24x7 and 365 days up and running. 
	- everything on ethereum can be examined, analyzed and reasoned about.
- ###### High Transparency & Neutrality
	- no centralized entity can change the availability or transparency aspects of the platform or apps built on top of it.
- ###### Censorship resistant and Lower counter-party risk


It is progressive decentralization and there are some elements of centralization and over time by design are removed.

#### Purpose

- not to be just currency or payment network
- native currency is present and i.e. ether. needed for operating any part of infrastructure necessary and integral
- ether is a utility token, to utilize the ethereum platform, 
- to use ethereum you need to pay ether 

#### Programming language

- Bitcoin is scripting lang, script is limited, just evaluates to true/false
- Ethereum, supports EVM, a virtual machine and by design it is meant to be general purpose lang
- Turing Complete, allows SC written here not bound by expressiveness nature of lang. 
	- can be arbitrary in nature
	- can encode arbitrary state (can hold any type of information)
		- simple data types like numbers and strings or more complex data structures like lists, mappings, or even custom data types.
	- can encode arbitrary rules (perform any kind of operations or calculations)
		- from simple arithmetic to complex algorithms.
	- unbounded in complexity and expressiveness
- Bitcoin focuses on ownership of bitcoin
	- state transitions on bitcoin track the transfer of bitcoins and it's referred as UTXO, Unpent transaction Outputs
	- UTXO based.
- Ethereum on the other hand, is general purpose state
	- not only tracking the state of currency ownership of ether but the state of the different smart contracts as tx interact with them
	- it is state or account based

#### Core Components
- Ethereum is built on P2P network, running on TCP port 30303
- Network is known as DevP2P, 
	- the DevP2P specifications define precisely how nodes should find each other and communicate
- It still uses the entire stack, the TCP stack
- But not as clients and server, rather as peer-to-peer that are exchanging msgs at that layer.
- **Transactions:**  Ethereum tx are network msgs that include sender, recipient, transferring some value and data payload.
-  **State machine:** Ethereum state transitions are processed by the Ethereum Virtual Machine ([[EVM]]), a stack-based virtual machine that executes bytecode (machine-language instructions). 
	- EVM programs, called "smart contracts," are written in high-level languages (e.g., Solidity or Vyper) and compiled to bytecode for execution on the EVM.
- **Data structures:** Ethereum’s state is stored locally on each node as a database (usually Google’s LevelDB), which contains the transactions and system state in a serialized hashed data structure called a [[Merkle Patricia Tree]]
- **Consensus Algorithm:** Nodes need to agree upon the global state and that is agreed upon by consensus algorithm.
	- In Bitcoin it is Nakamoto Consensus, i.e. PoW. which uses sequential single-signature blocks, weighted in importance by PoW to determine the longest chain and therefore the current state.
	- In Ethereum it was Nakamoto but that has been transitioned to PoS algorithm in Ethereum 2.0(Serenity).
	- PoW is what provides Economic security, like to dodge 51% attack. Ethereum before 2.0 used PoW algo called Ethash.
- The protocol itself, the consensus algo, data structures and other core components are implemented within what are known as Ethereum clients. 
	- If you are running a Eth node then you are using one of these Eth Clients.
	- Most prominent of which are Go-Ethereum (Geth) and OpenEthereum. The others are Erigon, Nethermind and Turbo-geth.

#### Halting Problem
- Since [[EVM]] can read and write data to memory, it makes it a Turing-complete system.
- The Turing complete systems face the challenge of the halting problem i.e.  given an arbitrary program and its input, it is not solvable to determine whether the program will eventually stop running.
- So Ethereum cannot predict if a smart contract will terminate, or how long it will run. *Therefore, to constrain the resources used by a smart contract, Ethereum introduces a metering mechanism called gas*

#### Gas Metering
- As EVM executes smart contract, it carefully accounts for every instruction(computation, data access, etc.). Each instruction has a predetermined cost in units of gas. 
- the EVM will stop execution if the amount of gas consumed by computation exceeds the gas available in the tx else refunded to the sender.
- Gas is the mechanism Ethereum uses to allow Turing-complete computation
- Ether must be sent along with a tx as a gas price depending on gas units needed for the tx.
- Gas price is not fixed, depends on demand. 


Web3 vs. Web2
- Web2 biz models are freemium/ad oriented
- Web3 is decentralized incentivised participation

Ethereum Triad
- Compute => Ethereum itself
- Storage => Swarm
- Network => Whisper/Waku

Decentralization
	Three types:
		- Architectural => Physical computer/hardware
		- Political => Individuals/Orgs behind physical hardware (WetWare)
		- Logical => Data Structures, software aspects


Native Currency
- Ether => 18 decimals, divisible upto 18 decimals
- Smallest unit => Wei 10 ** 18 Wei = 1 Ether
- 10 ** 3 Wei = 1 Babbage
- 10 ** 6 Wei = 1 Lovelace

Cryptography
- there are two classes; symmetric and asymmetric cryptography
- asymmetric cryptography is also known as public key cryptography, this is what used in Ethereum
- Here there are two key components, public key and private key
- Private key is secret, used to derive public key
- The cryptography used here mostly about Digital signatures, not encryption at a protocol level
- The digital signature algo used same as Bitcoin called ECDSA, Elliptic Curve Digital Signature Algorithm
- Elliptic Curve Cryptography (ECC) approach to public key cryptography and based on algebraic structure of elliptic curves over finite fields
- SECP-256K1 Curve parameters are used in Ethereum and Bitcoin

	- Private key: 256 bit, random number, used to derive public, 
	- Public key is used to derive Address/Account