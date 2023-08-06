---
layout: archive
title: 'SBUX Analysis'
permalink: /projects/financial-data-analysis
date: 2023-08-5
author_profile: true
---

### Introduction

Most people who know me know that I’m a coffee enthusiast. In fact, you can probably find me at a cafe two to three times a week. Unfortunately for me (and my wallet) lattes aren’t exactly inexpensive. Most personal finance gurus, notably Graham Stephen, are proponents of 20 cent iced coffees (read: ditch the coffee shop and make your own coffee). While I’m certainly not giving up my coffee anytime soon I decided to see just how much my ROI would be if I were to invest my money into SBUX instead. 

### Dataset

Plotting Starbuck’s adjusted close price for the past five years, there’s been a 117.87% increase despite some dips. 

<iframe title = "Embedded cell output" src = "https://embed.deepnote.com/9f7975cc-612b-4e50-88af-2babcbae5f16/709b9a9f37c84e5a81cbae41680a7c9f/c3748bbe75864995891999ec305b821a?height=554.8316345214844" height = "554.8316345214844" width = "100%"></iframe>

According to [Quartz](https://qz.com/195631/what-people-order-at-starbucks-around-the-united-states), the most popular drink ordered in New York is Pike place which comes in at 3.45 for a grande. Assuming the average person orders a Pike place each weekday morning, we can translate that into a person dollar cost averaging into SBUX with a 3.45 investment. Here are some additional values we’re calculating: 

* **principal**: yesterday’s principal + $3.45
* **fractional shares per day**: current share price / 3.45
* **total shares**: today’s shares + yesterday’s shares
* **portfolio value**: total shares * today’s share value
* **total gain**: portfolio value - principal

Using these calculations, investing 3.45 each market day for 5 years would’ve brought a principal investment of 4340.10 to 5328.12. That’s a 22.76% gain or 988.02.

<iframe title = "Embedded cell output" src = "https://embed.deepnote.com/9f7975cc-612b-4e50-88af-2babcbae5f16/709b9a9f37c84e5a81cbae41680a7c9f/80217183033c4fb09d285505981d9b43?height=509.5364685058594" height = "509.5364685058594" width = "100%"></iframe>

How does investing into SBUX stack up against a an index fund? Applying the same DCA strategy to the S&P 500 (SPY) the same principal investment of 4340.10 would’ve resulted in a portfolio balance of 5800.12, a gain of 33.64%. 

<iframe title = "Embedded cell output" src = "https://embed.deepnote.com/9d38d4f1-0114-43fe-97ea-cfb021651a21/d01c660a1c644e098474d1487a1923c1/22f9a86b3caa4a3da52bb489326f2172?height=509.5364685058594" height = "509.5364685058594" width = "100%"></iframe>

Does investing in SBUX have any diversification benefits if you already have your money in the S&P 500? 

<iframe title = "Embedded cell output" src = "https://embed.deepnote.com/9d38d4f1-0114-43fe-97ea-cfb021651a21/d01c660a1c644e098474d1487a1923c1/ffd5f936e0054cb1b1c3b4b9e1210564?height=731.87158203125" height = "731.87158203125" width = "100%"></iframe>

With a correlation coefficient of 0.82, it seems slightly risky to put all your eggs in one basket. But there’s no doubt that whether you’re putting your money in SBUX or SPY you would’ve made returns at a far greater rate than simply drinking your morning coffee. 