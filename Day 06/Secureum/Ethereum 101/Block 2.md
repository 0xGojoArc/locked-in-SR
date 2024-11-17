##### [[Block 2]]

#21
###### Public key
- It not secret, derived from private key
- It's a point of the elliptic curve calculated from the private key using elliptic curve multiplication
- public key can't be used to derive private key

###### Ethereum State
- It's basically made of an objects called, Accounts
	- State -> Accounts
- Each ETH Acc ha 20 byte address
- Addr Used to talk to each other
- Used in transfer of value and info

###### Ethereum Account
- Each acc has 4 field
	1. Nonce -> Count
	2. Balance -> Ether
	3. Contract Code  
	4. Contract Storage 
###### Account Types
- Two Account types
	1. EOA -> Externally Owned Account
	2. Contract Account
###### EOA
- EOA is controlled by private key
- It does not have associated smart contract Code or Storage fields
	- ###### EOA Ownership
		- controlled single private key
		- private key used to create signatures used to control the ether in the acc
###### Contract Acc
- Contract acc controlled by *Code* that is contained in that acc.
- It has associated Smart contract Code and Storage
- Smart contracts, when contract acc receives a msg, it triggers a code in contract acc and that runs the code access any internal storage associated with it
- when the code runs, it can send msg to other accs and even create contracts
###### Smart Contracts
- autonomous agents, 
- always present in execution env of eth chain, 
- always ready to be triggered by tx/msg sent to them -> results in execution code r=present in contract code.
- they have access to ether balance and contract storage.
- execution of code results in manipulation pf this balance and storage.
###### Keccak-256
- cryptographic hash function
- closely related SHA-3 -> Secure Hash Function
	- standardized version of keccak
- very critical to functioning of eth and smart contracts
###### EOA Address
- controlled by private key, public key is derived from private key
- public key is hashed sing keccak-256 hash function
- output of the this hash is taken, and only last 20 bytes, least significant bytes of the hash of the public key are taken as address of AOA account.
##### Transactions
- signed msgs, originate outside eth chain
- triggered by EOA's digital signature derived from private key
- they transmitted by eth network and trigger state change on the chain
- only tx that can trigger change of state

	###### Transaction Properties:
	-  They atomic, -> All or nothing
	- tx can not divided or interrupted, no partial changes reflected in the chain
	- tx are serial, one after another, sequentially done
	- tx to be included in the chain is decided by miners
	- miners run node and they decide which tx to include in the block, depends on congestion and gasPrice
	  
	###### Transaction Components:
	- Tx are serialised binary msgs
	- They contain 7 components
		1. Nonce: seq number, used to prevent msg replays, replaying the same tx. 
		2. gasPrice: amount of ether in wei paid by sender
		3. gasLimit: max gas unit sender willing to pay
		4. recipient: destination add
		5. Value: amount of ether in wei, sender is sending
		6. data: its a payload. 
		7. Signature(v,r,s) ->  ECDSA signature
	
	Nonce:
	- Number Used Only Once
	- Sequence no, as per protocol its incremented and to prevent replay
	- In EOA -> it is no of txs sent
	- In Contract Acc -> no. of other contracts created 
	
	###### gasPrice:
	- sender willing to pay
	- its measure in wei/gas unit
	- Higher the gasPrice -> faster block inclusion by miner
	- gasPrice is not set, it depends on demand for the block space at point of time
	  
	###### gasLimit:
	- max gas unit a sender willing to pay for the tx
	- depends on type of tx being sent,
		- simple ether transfer -> costs 21000 gas units
		- contract tx, like targeting a func in a contract -> more gas, if there's less gas for the tx to occur then results in Out Of Gas Error (OOG Error)
		- etimated gas should be sent, if there's excess then unused is sent back to sender(it doesnt really send back or anything, the gas is used only during execution so the gas unused from the gasLimit simply remains with the user)
	  
	###### recipient:
	- 20-byte address 
	- it can be EOA addr or Contract addr
	- any addr on eth protocol/chain, there's no protocol validation
	- recipient is really the target address and there is no from address that is a component of the transaction. the reason is because this address can be derived from the ECDSA signature components v, r, and, s. they can be used to derive the public key and which in-turn can be used to derive the `from` address.
	  
	###### value:
	- value of ether sent to recipient
	- If it's EOA -> balance increase and sender's decrease
	- Contract acc -> 
		- depends on the data present part of tx, 
		- the data represents the contract function that is being targeted
		- value of ether sent depends on the impl of that particular func
		- if there's no data in tx, 
			- it triggers contracts receive/fallback function and what happens to sent ether depends on their impl.
			- if receive/fallback func is absent, then tx results in exception. ether remains in originator acc.
	  
	###### data:
	- sent from sender to recipient
	- it's relevant only when recipient is contract acc
		- in this case, it contains contract function and function args
	  
	###### ECDSA Signature - 
	- Its a ECDSA Signature, 65 bytes, three components: V, R, S:
	- r, and s represents signature components, 32 bytes each total 64 bytes
	-  v, recovery identifier, its 1 byte.
		- its value can be 27/28 
		- or it can be chainID * 2 + 35/36, chainID is indentifier of the blockchain, for eth mainnet is 1.
