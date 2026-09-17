# CMPS 2200  Recitation 03

**Name**Lily Goldman  


In this recitation, we will investigate recurrences for work and span of algorithms. Unlike other recitations, you may add your answers directly to this document. You do not need to use an `answers.md`.

## Tree method (9 pts)
Solve the following recurrences using the tree method. 

a) $W(n) = 3W(n/4) + n^2$
**Answer: $W(n) \in O(n^2)$**

- Level 0 (the top): 1 node of size $n$. Work: $n^2$.
- Level 1: 3 nodes, each of size $n/4$. Work: $3 \cdot (n/4)^2 = \frac{3}{16}n^2$.
- Level 2: 9 nodes, each of size $n/16$. Work: $9 \cdot (n/16)^2 = \left(\frac{3}{16}\right)^2 n^2$.
- Level $i$: work is $\left(\frac{3}{16}\right)^i n^2$.

Each level does only $\frac{3}{16}$ of the work of the level above it, so the top level does most of the work. Adding up all the levels:

$$W(n) = n^2\left(1 + \frac{3}{16} + \left(\frac{3}{16}\right)^2 + \dots\right) \le n^2 \cdot \frac{1}{1 - 3/16} = \frac{16}{13}n^2$$


b) $W(n) = W(n/3)+ W(2n/3) + n \log n$
**Answer: $W(n) \in O(n \log^2 n)$**

The two children have sizes $n/3$ and $2n/3$, which add up to $n$. So on every full level, the sizes of all the nodes add up to $n$.

- Level 0: work is $n \log n$.
- Level 1: work is $\frac{n}{3}\log\frac{n}{3} + \frac{2n}{3}\log\frac{2n}{3}$, which is at most $n \log n$.
- Every level: work is at most $n \log n$, because every node is smaller than $n$ and the sizes add up to at most $n$.

How many levels are there? The tree is lopsided. The longest path always takes the $2n/3$ branch, so it has $\log_{3/2} n$ levels, which is $O(\log n)$.




c) $W(n) = 2W(n/2)+ n/ \log n$
**Answer: $W(n) \in O(n \log \log n)$**

- Level $i$ has $2^i$ nodes, each of size $n/2^i$.
- Work at one node: $\frac{n/2^i}{\log(n/2^i)} = \frac{n/2^i}{\log n - i}$.
- Work for the whole level: $2^i \cdot \frac{n/2^i}{\log n - i} = \frac{n}{\log n - i}$.

The tree has $\log n$ levels. Adding up the levels, starting from the bottom:

$$W(n) = n\left(\frac{1}{1} + \frac{1}{2} + \frac{1}{3} + \dots + \frac{1}{\log n}\right)$$

The sum $1 + \frac{1}{2} + \frac{1}{3} + \dots + \frac{1}{m}$ is called the harmonic series, and it is about $\ln m$. Here $m = \log n$, so the sum is about $\log \log n$. The $n$ leaves add only $n$ more work, which is smaller.




## Brick method (6 pts)
Solve the following recurrences using the brick method. First argue
whether they are root-dominated, leaf-dominated, or balanced. Then,
state the resulting asymptotic bound for $W(n)$.

d) $W(n) = 2 W(0.49 n) + 1.01 n$

**Answer: root-dominated, so $W(n) \in O(n)$**

- Level 0: work is $1.01n$.
- Level 1: 2 nodes of size $0.49n$. Work is $2 \cdot 1.01(0.49n) = 0.98 \cdot 1.01n$.
- Level 2: work is $0.98^2 \cdot 1.01n$.

Each level does $0.98$ times the work of the level above it. The work gets smaller as we go down, so the tree is **root-dominated**. Adding up all the levels:

$$W(n) \le 1.01n\left(1 + 0.98 + 0.98^2 + \dots\right) = 1.01n \cdot \frac{1}{1 - 0.98} = 50.5n$$



e) $W(n) = W(n/2) + W(n/4) + 0.999n$
**Answer: root-dominated, so $W(n) \in O(n)$**

- Level 0: work is $0.999n$.
- Level 1: nodes of size $n/2$ and $n/4$. Work is $0.999\left(\frac{n}{2} + \frac{n}{4}\right) = \frac{3}{4} \cdot 0.999n$.
- Level $i$: work is at most $\left(\frac{3}{4}\right)^i \cdot 0.999n$.

Each level does at most $\frac{3}{4}$ of the work of the level above it. The work gets smaller as we go down, so the tree is **root-dominated**. Adding up all the levels:

$$W(n) \le 0.999n\left(1 + \frac{3}{4} + \left(\frac{3}{4}\right)^2 + \dots\right) = 0.999n \cdot 4 \approx 4n$$



## Bonus (3 pts)

Solve the following recurrence.

f) $W(n) = \sqrt{n}W(\sqrt{n}) + \sqrt{n}$
**Answer: leaf-dominated, so $W(n) \in O(n)$**

- Level 0: 1 node of size $n$. Work is $\sqrt{n} = n^{1/2}$.
- Level 1: $n^{1/2}$ nodes, each of size $n^{1/2}$. Each one does $n^{1/4}$ work, so the level does $n^{1/2} \cdot n^{1/4} = n^{3/4}$ work.
- Level 2: $n^{3/4}$ nodes, each of size $n^{1/4}$. Each one does $n^{1/8}$ work, so the level does $n^{7/8}$ work.

The work goes up as we go down ($n^{1/2}$, then $n^{3/4}$, then $n^{7/8}$), so the tree is **leaf-dominated**.

A simple way to write the work of a level: if the nodes on that level have size $s$, there are $n/s$ nodes and each does $\sqrt{s}$ work, so the level does $\frac{n}{\sqrt{s}}$ work.

The node sizes keep taking square roots until they reach 2. Starting from the bottom, the sizes are $2, 4, 16, 256, \dots$, so the level work is:

$$\frac{n}{\sqrt{2}} + \frac{n}{2} + \frac{n}{4} + \frac{n}{16} + \dots$$

These terms get small very quickly, so the total is less than $2n$.


