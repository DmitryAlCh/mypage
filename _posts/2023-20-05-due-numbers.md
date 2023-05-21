---
layout: post
title:  "Due numbers"
date:   2023-05-20 00:00:11 +0200
categories: [common-sense]
published: true
last_edit: 2023-05-20
usemathjax: true
---

Being a web-dev in casino makes Me sometimes think on probabilities. Sometimes
I find myself entertaining "win schemes" in my head. There are no such schemes
obviously, but at the start of this article, I had no "math" at hand to break
My "win schemes", or explain to a "child" why it won't work. Had to read about
50 pages of the ["Е.С.Вентцель Теория
Вероятностей"](https://obuchalka.org/20190227107251/teoriya-veroyatnostei-ventcel-e-s-2006.html),
to connect the dots.  
**TLDR**: Read read the wiki on [gambler's fallacy](https://en.wikipedia.org/wiki/Gambler%27s_fallacy).

Alright the mighty "win scheme": 
> The probability of events happening in a row drops exponentially(actually correct claim),
> when it is enough "tails" in a row, We bet on "heads". 

This "win scheme" has special chapter in gamblers fallacy wiki page, 
called [Monte Carlo Casino](https://en.wikipedia.org/wiki/Gambler's_fallacy#Monte_Carlo_Casino),
where "black" was the outcome on one roulette table for **26** times in a row.

Keeping it more simple with the coin, [Wiki](https://en.wikipedia.org/wiki/Gambler's_fallacy#Coin_toss)
actually explains why "heads" won't come after 2 "tails", but such explanations
are the reason people gamble for wrong reasons. 

So a bit of simple math: individual probabilities multiply, 
when we care that some outcome happens a certain number of times. 
I make a correct claim that 3 "tails" is to rare to occur. And immediately develop
a get rich quick scheme: 
I will bet on "heads" every time I see 2 "tails" in a row.
The rare to occur sequence:  
"tails"-"tails"-"tails" = $$ 1/2 * 1/2 * 1/2$$ or  $$1/2^3 = 0.125 $$

Wow, out of 100 games "tails" will happen after 2 "tails" only 12.5 times, so I
am rich. (Meaning that 87 times it will be "heads")  
I cannot be the only one who at least once thought way, right? Just
to make sure, will also write how my "winning sequence" would look like: 

"tails"-"tails"-**"heads"** = $$ 1/2^3 = 0.125 $$ 

Yes probabilities are the same. Any predicted outcome has same probability as
consequtive "tails" outcomes in a row. Hence when there is a "streak" of
"tails" or "black", the probability of next outcome, should be counted together
with whole sequence. Or just as simple as **single outcome probability**, which
is counter intuitive. But these 2 perfectly coexist together, probability of
whole sequence and singular event. Whole streak's probability "settles" the
case for "reality needs to level out the distributions". So as soon as You
think reality should even out something, remember to calculate from the start.
There is no argument that reality will make the distribution 50/50, there is
just no way to know how exactly it will do it.

And a joke before going to simulation results. "A blonde" is asked what is the
probability of Her meeting the dinosaur on the streets - $$1/2$$ She replied,
it is either I meet it or not.

For a simulation I wanted to see 
* that "tails" in a row happen known amount of times, 
* sequence of "tails" happens same amount of times as "heads" sequence or any other "custom" sequence.

With a help of `Math.floor(Math.random()*2)`, in a loop (1000 times), gotten some data: 

|(index)|simulation heads|simulation tails|simulation custom|theory|    
|:---:|:---:|:---:|:---:|:---:|
|1 in a row|508|492|492|500|
|2 in a row|170|166|160|250|
|3 in a row|73|69|72|125|
|4 in a row|36|32|34|62.5|
|5 in a row|19|19|13|31.25|
|6 in a row|10|11|7|15.62|
|7 in a row|8|2|3|7.81|
|8 in a row|4|1|1|3.90|
|9 in a row|2|0|1|1.95|
|10 in a row|1|0|0|0.97| 

Code for simulation should be seen [here](https://github.com/DmitryAlCh/teorver). 
Answering original questions:
* Tails in a row sometimes happen known amount of times.
* Sequence of tails has same rate as any other sequence.

Number of sequences being below mathematical lead Me to double check with a "truly"
random sequence from [www.random.org](https://www.random.org/integers/?num=1000&min=0&max=1&col=1&base=10&format=html&rnd=new)
Here is the table from there:

|(index)|simulation heads|simulation tails|simulation custom|theory|    
|:---:|:---:|:---:|:---:|:---:|
|1 in a row|473|527|527|500|
|2 in a row|157|179|169|250|
|3 in a row|60|82|71|125|
|4 in a row|27|38|22|62.5|
|5 in a row|9|20|12|31.25|
|6 in a row|3|6|10|15.62|
|7 in a row|1|3|8|7.81|
|8 in a row|1|1|5|3.90|
|9 in a row|1|1|1|1.95|
|10 in a row|0|0|1|0.97| 

Was expecting values to be close to theoretical ones. Hope I have not stopped
reading the book too early to miss explanation of "non-linear" connection
between **probability** and **rate of occurance**.  But it started to get too
mathematical, and boring.

Get-rich-quick-scheme failed again.
