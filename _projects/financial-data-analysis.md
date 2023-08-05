---
layout: archive
title: 'SBUX Analysis'
date: 2023-08-27
permalink: /projects/financial-data-analysis
---

### Introduction

Most people who know me know that I’m a coffee enthusiast. In fact, you can probably find me at a cafe two to three times a week. Unfortunately for me (and my wallet) lattes aren’t exactly inexpensive. Most personal finance gurus, notably Graham Stephen, are proponents of 20 cent iced coffees (read: ditch the coffee shop and make your own coffee). While I’m certainly not giving up my coffee anytime soon I decided to see just how much my ROI would be if I were to invest my money into SBUX instead. 

### Dataset

Plotting Starbuck’s adjusted close price for the past five years, there’s been a 117.87% increase despite some dips. 

<iframe title = "Embedded cell output" src = "https://embed.deepnote.com/9f7975cc-612b-4e50-88af-2babcbae5f16/709b9a9f37c84e5a81cbae41680a7c9f/c3748bbe75864995891999ec305b821a?height=411.4114685058594" height = "411.4114685058594" width = "500">

According to [Quartz](https://qz.com/195631/what-people-order-at-starbucks-around-the-united-states), the most popular drink ordered in New York is Pike place which comes in at $3.45 for a grande. Assuming the average person orders a Pike place each weekday morning, we can translate that into a person dollar cost averaging into SBUX with a $3.45 investment. Here are some additional values we’re calculating: 

* **principal**: yesterday’s principal + $3.45
* **fractional shares per day**: current share price / 3.45
* **total shares**: today’s shares + yesterday’s shares
* **portfolio value**: total shares * today’s share value
* **total gain**: portfolio value - principal

Using these calculations, investing $3.45 each market day for 5 years would’ve brought a principal investment of $4336.65 to $5416.16. That’s a 24.89% gain or $1079.51.

<iframe title = "Embedded cell output" src = "https://embed.deepnote.com/9f7975cc-612b-4e50-88af-2babcbae5f16/709b9a9f37c84e5a81cbae41680a7c9f/80217183033c4fb09d285505981d9b43?height=1013.4861450195312" height = "1013.4861450195312" width = "500">

How does investing into SBUX stack up against a mid-size company or an index fund? Applying the same DCA strategy to Dutch Bros (BROS) and to the S&P 500 (SPY), we see investing into Dutch Bros with a principal investment of $1638.75 would’ve resulted in a 22.55% drop resulting in a portfolio value of $1269.16.