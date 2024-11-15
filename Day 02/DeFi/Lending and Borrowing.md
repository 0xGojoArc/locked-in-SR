##### [[Lending]] and [[Borrowing]]


- It creates money markets for particular tokens such as Eth, DAI, USDC or wrapped BTC
- Users supply their tokens to particular money market to become lenders
	- lenders start receiving interest on their tokens according to current supply APY (Annual percentage yield)

- supply tokens are sent to Smart Contract(SC) and become available for other users to borrow
- In exchange for supply tokens, the SC issues other tokens that repr. the supply token + interest
	- it is called A Token in Aave and C Token in Compound
- All the loans are collateralised(as of 2020), the borrower need to supply token that is worth more than the actual loan that they want to take.

##### Why Collateral? 
- why take a loan by providing tokens worth more than loan itself?
	- users want to cover unexpected expenses without selling their token
	- avoiding or delaying capital gain taxes 
	- or using borrowing funds to increase their leverage in a certain position

##### Limit?
- 1. funds available in the particular market, usually not a problem unless is trying to borrow a big amount of tokens.
- 2. Collateral Factor
	- It determines how much can be borrowed based on quality of collateral
	- DAI and ETH has Collateral Factor of 75% on Compound, which means upto 75% value of value of the supplied DAI or ETH can be used to borrow other tokens.

- 3. The value of borrowed amount must always stay lower than the value of their collateral token times collateral factor.
	- if this condition holds then there's no limit to how a user can borrow funds for.
	- if the collateral falls below required collateral level, then the protocol will liquidated the collateral in-order to pay the borrowed amount.

- Interest paid by the borrowers is the interest earned by the lenders.
- so the borrow APY]is higher than Supply APY

- Interest APYs are calculated per ethereum block.
	- It means the DeFi lending provides variable interest rates, which can depend on the lending and borrowing demand
- Variable Supply/Borrow APY are provided by both Compound and Aave
- But Aave also provides Stable Borrow APY
	- Stable Borrow APY is fixed in the short term but can change in the long term, to adjust the changes in supply demand ratio
	- Aave also offers Flash Loans, Users can borrow funds with no upfront collateral for a short period of time i.e. One Ethereum transaction.

