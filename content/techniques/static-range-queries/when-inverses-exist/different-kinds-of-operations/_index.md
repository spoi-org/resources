---
draft: false
title: 'Different kinds of operations'
weight: 4
---

So far, all the problems we have covered had "addition" as their main point. Could we do the same with other operations? Build prefix product arrays? What about prefix maximum?

What enabled us to calculate the sum of numbers in a range so efficiently was the fact that addition has a very simple inverse: subtraction. After building the prefix sum array, we could easily "undo" any prefix we didn't want. The same goes for multiplication, but undoing a $\text{max}$ operation isn't so simple. Purely from the information that $\max(a,b) = b$, it's impossible to know the value of $a$. This is why although we can create a prefix maximum array, we can't readily use it for general range maximum queries. 

However, as long as we can efficiently undo a prefix, there isn't really any other barrier to using prefix *anything*s for range _anything_ queries. 