[[Day 19/UniSwap/UniSwap V2/UniSwap V2]]
##### Router Swap Functions
###### **function `swapExactTokensForTokens`** 
- Swap all the input for the max output
	- `in` = 1000 ==DAI==
	- `out` = max ==WETH==
- This function is from Router Contract.
![[UniSwapV2-function-swapExactTokensForTokens.png]]
- It calls the UniswapV2Library function called `getAmountsOut`, it returns an array of uint called `amounts`
	- `amounts[0]` => amount of *input token*, i.e. `amountIn` that's passed to the function
	- `amounts[last]` => output of the last swap,
	- if it's multihop swap, then this array will also contain outputs for the intermediate swaps

- Line 246,  we check the amount stored(last amount) in the array is greater or equal to `amountOutMin`, it is the user specified minimum amount wants from this swap.

- Line 250, Now the input token is transferred over to the Pair Contract.
- The input token will be stored inside `path[0]`, the Pair contract is computed by taking `path[0]` and `path[1]` and amount to send is stored inside `amounts[0]`
- it uses **`CREATE2`** to compute Pair contract address
- Now internal function `_swap` will call the function `swap` on the Pair contract.
- **NOTE:** This Router Contract's function has automated two steps;
	1. Directly sending tokens into Pair Contract
	2. And then calling `swap` function on the Pair Contract

