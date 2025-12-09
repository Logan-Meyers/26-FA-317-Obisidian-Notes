Alan Turing was the father of compute science and AI.

Turing machine intuition: a machine does computations based on rules without understanding why, it just does it. It is controlled by a finite set of rules.
- piece of paper = memory
- r/w and move by pen = cpu
- rules = program

Turing-Church Thesis (roughly speaking): Humans can't build more powerful machines than Turing Machines.
TMs are mother of all machines
TMs can simulate/describe out human's logical reasoning with one TM or program. Why? 

For thousands of years, humans thought of 'computing' as performing operations on numbers using math and things to get a result. Take addition for example. In your life, many years ago- let's say you were 8, in elementary school- you learned how to do addition. You're given the problem "Calculate 123+457." You got through and number by number, you do addition on your fingers, do the carry over, and arrive at a result.
But in 1936, Alan Turing took a different view on computing and changed the world. How did he think of computing? Well, take that math example. A computer would go through the numbers and perform computations, following a strict set of rules. i.e. "3+7=10; write 0, write 1 in carry over spot" ... "2+5+1=8; write 8, do nothing for carry over" ... and so on.
So a Turing Machine is defined as such:
1) It requires something to perform on (in addition example, piece of paper)
2) It reads and writes to the paper, moving around locally (in addition example, think of pen)
3) It is controlled by a finite set of rules (in addition example: "you see 3, you see 7, you write 0 in answer and 1 in carry spot")

A human _understands_ how to do things. They understand _why_ things are done. "why is 3+7 = 10? Because 3 fingers along with 7 more adds up to 10 total fingers. The 0 digit is written in the answer, and the 1 digit is written in the carry over."
But a computer does not understand. It only _does_. It does not think or know "why 3+7=10", it just knows based on the rules it follows.

And the Turing-Church Thesis states that humans will never be able to create a computer more powerful than a Turing machine. Not even with quantum computing. To create a more powerful machine, you need to make _global_ writes, (think fish in tank flock to presented food)

L is *recognized* by TM M if M is al alg and L = L(M)
L is *accepted* by TM M if L = L(M)

When TM M runs on input word w, one of the following will happen:
1. M says yes on w (M enters an accepting state)
2. M crashes (says no)
3. M runs forever without saying yes

1 & 2 are halting (algorithm).

Observe: L is recognized by TM M iff
- For all w, w in L => M says yes on w;
- w not in L => M says no on w

L is accepted by TM M iff
- For all w, w in L => M says yes on w
- w not in L => M doesn't say yes on w (could run forever)

L is recursively enumerable if L is accepted by a TM M
L is recursive if L is recognized by a TM M

**Recursively enumerable** means that you can infinitely take things out of a bag of infinite things
**Recursive** means that things are taken out in some order

Suppose that L is r.e., then there is a TM M accepting L s.t. for every w, if w in L => M says yes on w; if w not in L => M doesn't say yes on w
Suppose that L is recursive, then there is a TM M recognizing it: for every w, if w in L => M says yes on w; if w not in L => M says no on w

Observe: By def, if L is recursive then L is r.e. However, not every r.e. language is also recursive.

$\text{Problems that can be solved in poly time} \in \text{Recursive Problems} \in \text{R.E. Problems} \in \text{All problems}$

Show that if L_1 and L_2 are r.e., then so is L_1 U L_2
Proof:
- Since both L_1 and L_2 are r.e. there are TMs M_1 and M_2 to accept each respectively.
- NOTE: Can't run if/else because one TM cannot say no (runs forever) so can't branch to other TM
- We construct TM M to accept L_1 U L_2 as follows:
- M : input x
- Run M_1 and M_2 in parallel on x
- if one of them says yes, return yes.
- NOTE: cannot say 'return no', since they could run forever

[FINAL POSSIBLE PROBLEMS]
- Show if L_1 and L_2 are recursive, then so is L_1 U L_2
- """ so is L_1 $\cap$ L_2

[FINAL] Hard proof: Thm: Let L be recursive. Define L' = {x ; Exist y . xy in L}. Show L' is r.e.
- Since L is recursive, L is recognized by an algorithm M.
- To show that L' is r.e., we need only to construct a TM M' to accept L'.
- [AUTOMATA TO AUTOMATA CONST. FOR TM]
	- LHS:
		- There is a y s.t.
		- M says yes on xy
	- LHS iff RHS
	- RHS:
		- M' is to work on x s.t.
		- M' says yes on x
	- REMARK: M never runs forever because it is an algorithm.
	- PSEUDOCODE: for TM
		- M' input x.  // I enumerate all strings y (infinite many) from short to long
		- while (1) {  // n = 0
			- for each word y with length n
				- run M on xy.
				- if M says yes, return yes (from M')
				- increment n
		- }
		- clearly, M' accepts L'.$\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\quad\blacksquare$

Remark: Nondet. TMs are == det. TMs

[FINAL EXAM] if L is recursive, then so is L' = {x ; Exists y . xy in L and |x| = |y|}
[FINAL EXAM] if L is r.e., then so is L' = {x ; Exists y . xy in L}

[CONCEPT] A problem (language) that has an alg to solve is called "decidable" ("solvable"). A problem that has no alg to solve is called "undecidable" ("unsolvable"). All in the scope of TMs.

A string can be encoded into a number, 1:1
- "12345" $\rightarrow 2^1 \cdot 3^2 \cdot 5^3 \cdot 7^4 \cdot 11^5$
- "11" $\rightarrow 2^1 \cdot 3^1$

A TM (or C program) can be uniquely encoded into a number called $i$.
We use $M_i$ to denote the TM when number is $i$.
Along with $w_i$.

Define L self = {i ; i in L(M_i)}
- this is the set of all number i s.t. when i runs on i, you will see "yes"
- program i is a string, so you run program i as string into the program i as input, and you will see "yes"

Define L \~self = {i ; i not in L(M_i)}
- L \~self is not r.e. (not accepted by any turing machines)
- prove by contradiction:
	- we have a TM, say, $M_j$ for some j, to accept it.
	- So, L \~self = L($M_j$)
	- $j \notin$ L \~self iff $j \notin L(M_j)$
	- ?????

$L_u = \{<M,w> ; M\text{ says yes on w}\}$ is not recursive. That is, there is not alg that can solve the question: given a TM M, word w, does M says yes on w? Undecidable.

Real world undecidable problems:
- [halting problem] Given a C-program P and input x, will P exit nicely running on input x?
- [halting problem] Given a C-program P with no input, will P halt? (not run forever)
- Hilbert's 10th problem:
	- given a multivariable polynomial, can you find an integer solution?

Thm: There is a true statement (in general) in math that can't be proven in math

math proving thm can be implemented by a TM M running on a string T (thm). M says yes on T if math can prove T
This can be proven by contradiction proof. For each theorem t, $t$ or $\overline{t}$ (negation of t) is true and there is a TM M to solve said proof. TM M will be run as follows: input t; run M on $t$ and M on $\overline{t}$ in parallel; either M says yes on $t$ or $\overline{t}$. "M halts on w" is decidable, which causes a contradiction.

Examples of which cannot be proven:
1. "Math has no contradiction"
2. CH (continuous hypothesis). is there a set in between natural numbers and real numbers?
3. Axiom of choice

