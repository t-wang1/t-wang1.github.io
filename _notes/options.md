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

**queue priority**: the priority with which your order is filled - ranked by price, and then by time of entry
**spread**: difference between bid and ask price
**theo**: current value a market maker believes an option is worth
**tick increment**: minimum increment between a price and the next price

### Liquidity

Generally speaking, the tighter the spread and the larger the volume of contracts the more liquid an asset is.

### Forward Prices 

Options are based on the foward prices of the underlying assets. To calculate the forward price, we use: $$F = Se^{rt}$$ where $F$ is the forward value of an instrument $S$, $t$ is time (in years) and $r$ is the interest rate

### Another Look at Options

An **in-the-money(itm)** option has some _intrinsic value_ (and extrinsic value) based on the current location of the underlying compared to the strike of the option. The intrinsic value of an option can be calculated as follows:

Intrinsic value (call): $$Max(0, U-K)$$ where $$U-K = Underlying Price - Strike Price$$
Intrinsic value (put): $$Max(0, K-U)$$ where $$K-U = Strike Price - Underlying Price$$

An **out-of-the-money(otm)** option has no intrinsic value. It does however have _extrinsic value_, attributed to the optionality of the option contract itself.

What influences extrinsic value?

1. Distance to ATM: how far strike is from the forward
2. Time: time to expiry
3. Volatility: higher volatility -> higher option prices

**Put call parity** is the mathematical link between puts and calls on the same strike. A simplified put-call-parity formula is as follows:

Call Price - Put Price = Underlying Price - Strike which can be otherwise simplified to $$C-P = U-K$$