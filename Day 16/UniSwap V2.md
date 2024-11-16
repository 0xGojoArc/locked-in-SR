##### [[UniSwap V2]]

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
- To swap [[ERC20]](==WETH==) to [[ERC20]](==DAI==) token *SwapExactTokensForTokens* is called.
- To swap ==ETH== to [[ERC20]](==DAI==) token *SwapExactETHForTokens* is called.