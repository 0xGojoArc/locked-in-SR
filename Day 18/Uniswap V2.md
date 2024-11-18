[[Uniswap V2]]

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
	$x_0 * y_0 = L ^ 2$
- After swap 
	$(x_0 + dx) * (y_0 - dy) = L ^ 2$
		- consider `dx` is swapped for `dy`
		- `dx` is added to the pool and `dy` is taken out of the pool hence we add `dx` to `x0` and subtract `dy` from `y0`
		![[swap-curve.png]]
		
	
- Use before swap and after swap equation to get equation for Y token;
  
>**Before Swap**                  **After Swap**
  $x_0 * y_0 = L ^ 2$                 $(x_0 + dx) * (y_0 - dy) = L ^ 2$
  $x_0 * y_0 =$                        $(x_0 + dx) * (y_0 - dy)$
  $$dy = y_0 - \frac{x_0\:y_0}{x_0 + dx}$$   ---
  We get:   
> $$dy = \frac{y_0\:dx}{(x_0 + dx)}$$
 --- 

##### Swap Fee
- Swap fees are charged on token in. A fraction of token comes in is taken out from the calc of $dy$ 
- $0 \leq f \leq 1$
	- $f$ will be number between 0 and 1.
	- 1 means 100% of $dx$ and 0 means 0% of $dx$.
- Swap fee = $f dx$ `(the amount of token comes in, i.e. dx)`
$$Swap\;Fee = fdx$$
**$dy$ with fee:**
- since $f$ times $dx$ is the swap fee, the amount used to calc $dy$ will be $1-f$ times $dx$
- 
 $$dy = \frac{y_0\:dx}{(x_0 + dx)}$$

- we $(1 - fee)$ from $dx$, 1 represents whole amount (or 100%) and by subtracting the fee, we get effective amount that will be used in the swap 
$$
	dy = \frac{dx\:(1 - f) * y_0 }{(x_0 + dx\:(1 - f))}
	$$

##### Swap Contract Call
###### Single Hop Swap
- A user will call a function on **Router Contract **to swap ==WETH== to ==DAI==
- To swap [[ERC20]](==WETH==) to [[ERC20]](==DAI==) => *SwapExactTokensForTokens*.
	- To swap ==ETH== to [[ERC20]] => *SwapExactETHForTokens*.
- WETH is transferred over to the **Pair Contract* *
- After WETH is transferred to **Pair Contract**, **Router Contract** will call `swap` function
	- `swap` function will check the balances of tokens inside the pair contract and calculate amount of ==DAI== to send over to the user and transfer it.
	
	![[single-hop-swap.png]]
	

###### Multi Hop Swap
- If user wants to swap ==WETH== to ==MKR== and there's no contract for that pair, so the user will have to first from ==WETH== to DAI and then ==DAI== to ==MKR== 
- User calls *SwapExactTokensForTokens* in the Router Contract.
	- Router Contract will transfer ==WETH== over to the **DAI/WETH** Pair Contract
	- Now Router will call `swap` function on **DAI/WETH** Pair Contract.
	- The Pair contract will calculate the amount of ==DAI== that will go out and then `transfer` ==DAI== to **DAI/MKR** Pair Contract
	- Now Router will call the `swap` function on DAI/MKR pair contract.
	- Now Pair contract calculates the amount of ==MKR== to send back to user and `transfer` to user.
	
	![[multiple-hop-swap.png]]

- **NOTE:** 
	- The **Pair contracts** focus on token reserves and the mathematical mechanics of swaps.
	- The **Router contract** focuses on orchestration, ensuring multi-hop swaps are executed atomically and in the correct sequence.
	- ==DAI== was sent to the **DAI/MKR** contract, not to the user or Router Contract.
	- The `transfer` to the next Pair Contract is not done by the Router but by the **DAI/WETH** Pair Contract directly after the `swap` function is executed.
	- The Router orchestrates the entire operation but doesn't handle the intermediate tokens (e.g., DAI). It ensures that intermediate outputs from one swap are routed directly into the next pair.
	- When the Router calls the `swap` function on a Pair Contract, it does so with specific parameters, including the amount of tokens to swap and the recipient address. The Pair Contract executes its logic, calculates the output amount based on current reserves, and then transfers tokens accordingly. Once this is done, control returns to the Router Contract.

##### Code Repo
- UniSwap V2 code is split into two repos
	1. V2 core: 
		-  Actual Contract that adds liquidity and swap tokens is here.
		- It's called *UniswapV2Pair.sol*, 
		- This Pair contract manages pairs of tokens and also swap tokens.
		- If we want to do Multi Hop swap, then we will need to use Router Contract
		- Users are not meant to directly interact with this code.
	1. V2 periphery: 
		- Router Contract - add/remove/swap tokens
		- This is used as Utility, it facilitates functions.
		- It automates some of the function calls that we need to make to the Pair contract.

##### Router Swap Functions
- 

