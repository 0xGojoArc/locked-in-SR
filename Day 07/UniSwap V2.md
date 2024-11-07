##### Graph
- Constant product AMM
- A graph of constant product function shows all possible combinations of tokens in the pool.
- The function used for the constant product AMM	 
	 $x * y = L ^ 2$
	x - amount of token X
	y - amount token Y
	L - liquidity

- the liquidity of an AMM pool is determined by the shape of its graph.
- Bigger the curve, the better price you will get when you trade
- When L is large amount, traders get better deal for the same amount of token that they put in.

##### Contracts
- There are three important contracts
	1. Factory
	2. Router
	3. Pair
###### 1. Factory Contract
- It deploys Pair contracts
###### 2. Pair Contract
- it holds pair of ERC20 tokens
- allow users to add or remove liquidity
- allows users to trade ERC20 tokens
- each ERC20 pair will have a pair contract
	==ETH/USDT==, ==DAI/ETH==. ==DAI/MKR==
- user can directly call into pair contract, but this is error prone may accidentally lock their token inside pair contract
###### 3. Router Contract
- It is an intermediate between user and pair contracts
- main job is to add/remove liquidity from the pair contracts and also safely swap tokens with the pair contracts
- swap can interact with either single pair contract or multiple pairs, Router contract will help by calling respective pair contracts

#### Swap

- Before swap:
	$x * y = L ^ 2$
- After swap 
	$(x0 * dx) + (y0 - dy) = L ^ 2$
- Y tokens we get is, we get;
$$
	dy = \frac{(dx * y0)}{(x0 + dx)}
	$$
##### Swap Fee
- swap fee = $f * dx$
- dy = dx * y0 / (x0 + dx)
- we $(1 - fee)$ from dx, 1 represents whole amount (or 100%) and by subtracting the fee, we get effective amount that will be used in the swap 
$$
	dy = \frac{dx(1 - f) * y0 }{(x0 + dx(1 - f))}
	$$

##### Swap Contract Call
- To swap ERC20(==WETH==) to ERC20(==DAI==) token *SwapExactTokensForTokens* is called.
- To swap ==ETH== to ERC20(==DAI==) token *SwapExactETHForTokens* is called.

