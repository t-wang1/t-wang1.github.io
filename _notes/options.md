---
layout: archive
title: "Options"
permalink: /notes/options
date: 2023-08-09
author_profile: true
---

These notes are for Akuna Capital's Options course

### General Idea

Options are _hedging_ instruments. **Calls** gives an individual/entity the right, but not the obligation to _buy_ an asset at the strike before a specified expiration date. The buyer effectively makes a profit if the value of the asset increases. Alternatively, **puts** give an individual/entity the right, but not the obligation to _sell_ an asset at the strike price before the expiration date. The seller makes a profit as the value of the asset decreases. 

Why trade a call?

1. Bet on upside move
2. Unlimited upside 
3. Limited downside
4. Increase in volatility
5. Protect short position  

### Definitions

1. Queue Priority: the priority with which your order is filled - ranked by price, and then by time of entry  
2. Spread: difference between bid and ask price  
3. Theo: current value a market maker believes an option is worth  
4. Tick Increment: minimum increment between a price and the next price

### Liquidity

Generally speaking, the tighter the spread and the larger the volume of contracts the more liquid an asset is.

### Volatility

Volatility is the measure of the deviation of an underlying's annual price movement. There are different types of volatility:  

1. Historical volatility: how much an underlying has moved in the past
2. Implied volatility: expected volatility of the underlying asset
3. Forward volatility: expected average volatility between the expiry date of two options with successive maturities  

To calculate the expected standard deviation percent move over a period of $t$ days, we can use the following formula:  

$${\sigma} * \sqrt{t/252}$$ 

### Forward Prices 

Options are based on the foward prices of the underlying assets. To calculate the forward price, we use: $$F = Se^{rt}$$ where $F$ is the forward value of an instrument $S$, $t$ is time (in years) and $r$ is the interest rate

### Another Look at Options

An **in-the-money(ITM)** option has some _intrinsic value_ (and extrinsic value) based on the current location of the underlying compared to the strike of the option. The intrinsic value of an option can be calculated as follows:

Intrinsic value (call): $$Max(0, U-K)$$ where U-K = Underlying Price - Strike Price  
Intrinsic value (put): $$Max(0, K-U)$$ where K-U = Strike Price - Underlying Price

An **out-of-the-money(OTM)** option has no intrinsic value. It does however have _extrinsic value_, attributed to the optionality of the option contract itself.

What influences extrinsic value?

1. Distance to ATM: how far strike is from the forward
2. Time: time to expiry
3. Volatility: higher volatility --> higher option prices

**Put call parity** is the mathematical link between puts and calls on the same strike. A simplified put-call-parity formula is as follows:

Call Price - Put Price = Underlying Price - Strike which can be otherwise simplified to $$C-P = U-K$$

### Greeks

The **delta** of an option is the number of contracts it takes to establish a _neutral hedge_ (i.e. indifferent to directional moves) under current market conditions. A more intuitive way of thinking about delta is the probability that an option will expire ITM. It's represented as a number between 0 and 1 for a call option and a number between 0 and -1 for a put option. 

**Example**: You're trading a $100.00 underlying and a 50 delta call is $1.00. If that underlying moves to $105.00 the call will be worth will be worth $3.50.

**Gamma** is often thought of as the rate of change in delta. The **theta** of an option is how much value that option loses daily. The **vega** of an option is the change in options value with a changed in implied volatility. Formally it's the option value price change per 1 point change in implied volatility. **Rho** is the sensitivity of an option's price to a change in interest rates. 