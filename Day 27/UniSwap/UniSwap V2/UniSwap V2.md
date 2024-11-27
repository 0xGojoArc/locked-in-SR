###### **function `swapExactTokensForTokens`** 
**Internal swap function**

![[UniSwapV2-function-internal-swap.png]]
- the inputs for the function are:
	- `amounts` : it is an array same as the array we get from line 245. - input amount, intermediate amount and last amount
	- `path` : it's the addresses of the tokens to swap and this path will determine which Pair Contract to call
	- `_to` : receiver of the final output token
	  
- line 219, **For loop**:
	- It gets the address of the pair contract and then it calls the function `swap` => line 228
	- loop iterates from `i = 1` to `i < path.length - 1` 
	![[UniSwapV2-internal-swap-foor-loop.png]]
	- when `i = 0`, the token address stored inside `path[0]` and `path [1]`  will be determine the address of  **first Pair Contract**
	- when `i = 1`, the token address stored inside `path[1]` and `path [2]`  will be determine the address of  **second Pair Contract**
	  
	- line 218, let's assume length of the path is `n` , then we will iterate loop til `n-2`(last but one) , at `n-1`(last one) we will access path of `[n-2]` and `[n-1]`. `path[n-1] `store the address of the last token.
		- this is why for loop loops from `i = 1` to `i < path.length - 1` or put it another way `i` will less than or equal to `n - 2` i.e.  $i \leq path[n-2]$
		- The last pair in the path involves tokens at indices `path.length - 2` and `path.length - 1`.
		  
	- line 220, for each iteration it will call access `path[i]` and `path[i+1]` , input token is `path[i] `and output token will be `path[i+1]`
	  
	- line 221, it will sort the tokens to determine which is `token0` that is stored in Pair contract
		- `token0` will used to figure out how much `amount0Out` and `amount1Out` to pass to the function `swap` that's on line 228.
		- `amountOut` is stored in `amounts[i+1]`
		- ==**NOTE**==: `amountIn` was already directly transferred tokens(line 250) to the Pair contract and called internal `_swap`.
		- so the first `amountIn` was already sent before we call `swap` on the Pair contract.
		What about `amountIn` for the rest of swap? what if we have to call two pair contracts?
		- line 226, 
			- we know when `i = path.length - 2` this is the last swap, so the address of `to` will be the receiver address which is specified in the input of the function `_to` .
			- when `i < path.length - 2` we need to pass the output to the next pair contract, the address of the next pair contract is `output`(it's the `path[i+1]` line 220) and `path[i+2]`
	
	- Once the to address is determined, last part of the for loop is to call the function swap on the pair contract
###### **function `getAmountsOut`** 

![[UniSwapV2-getAmountsOut-steps.png]]
- It calculates `amountOut` for each swap and return an uint array of amounts.

![[UniSwapV2-function-getAmountsOut.png]]
- line 94,  first element in the array, `amounts[0]` stores `amountIn` it is the first token that came in.
- rest of the elements store the amount of token that came out for each swap.
  
- line 102, **for loop**:
	- it loops through each token path address and calls function `getReserve` on Pair contract
	- ==**NOTE:**== Reserves is an internal balance of the tokens that is locked inside pair contract
	- By calling `getReserves` function, we get `reserveIn` and `reserveOut` 
	- line 106, we pass `reserveIn` and `reserveOut` as in input to function `getAmountOut` and this function calculates the amount of token that will come out(`amount[i+1]`) with respect to amount of token goes in(`amounts[i]`),  the `reserveIn` and `reserveOut`

**function `getAmountOut`**

![[UniSwapV2-function-getAmountOut.png]]
- first it checks if `amountIn`, `reserveIn` and `reserveOut` are greater than `0`
- it basically calculates how much token including fees:
  $$
	dy = \frac{dx\:(1 - f) * y_0 }{(x_0 + dx\:(1 - f))}
	$$

###### **function `swapTokensForExactTokens`**
- It calculates the `amountIn` needed to get the `amountOut`

**function `getAmountsIn`**

![[UniSwapV2-function-swapTokensForExactTokens.png]]
- The element in the `amounts` array contains the `amountOut` specified by the user
- line 123, it calculates `amountIn` for the elements less than last one by running a for-loop and calling a function `getAmountIn`
- For-loop runs from `n-1` to `1`, 
- line 133, for each iteration it calls `getAmountIn` with inputs amount out(`amounts[i]`), `reserveIn` and `reserveOut`
- `getAmountIn` function calculates the amount of token that needs to come in (`amounts[i-1]`) to get back out amount out (`amounts[i]`)
- line 123, we initialize the last element of the array to be equal to `amountOut`
- The three columns table from line 125 to 130 explains what's happening in the each iteration of the loop
	- the first column is for-loop indexes
	- the middle column is `amountOut`
	- **NOTE:** the last column is `amountIn`, this will be the output of function `getAmountIn`
- On the first iteration, line 126 -> given the amountOut (`amounts[n-1]`) which we set in line 123, will be the input to the function `getAmountIn`. It will calculate amount of token in needed to get amount out.
- The amount of token out that we already know is stored in `amounts[n-1]` it will call getAmountIn function to calculate token in needed and will be stored in `amounts[n-2]`
- In the next iteration, we know output amount that is needed is stored in `amounts[n-2]` and given that, we want to calculate amount of token in that is needed, call the function getAmount
- we keep looping until we get to `amounts[0]`, once we know the amountOut for the first pair then we can calculate amountIn for the first pair and store in `amounts[0]`

**function `getAmountIn`**

![[UniSwapV2-function-getAmountIn.png]]
- 