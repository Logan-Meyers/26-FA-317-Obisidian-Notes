Consider L = {0^n 1^n : n >= 1}.
String/word pattern for L: 0...0 1...1
Example: 000111

Using 1 pointer to compare two strings:
- Given two strings
- doesn't have to be input as one string followed by another
- second string right under

C.F. Grammar:
- S → 0 S 1 | 01
- Example of running:
	- S → 0 S 1
	- → 0 0 S 1 1
	- → 0 0 0 1 1 1

Mathematical definition of c.f. grammars:

A c.f. grammar G is defined as <v,$\Sigma$,S,P>
where V : nonterminal symbols
and $\Sigma$: terminal symbols
and $V \cap \Sigma$ = $\emptyset$
lower case symbols for terminal, upper case symbols for nonterminal symbols
$S \in V$ is the start symbols
$P$ (punctuation rules): each rule is in the form: $A \rightarrow \alpha$ where $A \in V$, $\alpha \in (V \cap \Sigma)^*$

Example:
- $S \rightarrow AB$
- $A \rightarrow 0A1 | \Lambda$
- $B \rightarrow 1B0 | \Lambda$
Where
- $V = {S,A,B}$
- $ $
- $ $

we write alpha can generate grammar to beta if ther are n>= 1 and beta_0 to beta_n in (v union sigma)* s.t.
1. beta_0 = alpha
2. beta_n = beta
3. beta_i = generate grammar to beta_i+1 for each i=0...n-1
Where (one-setup derivation)
	alpha generate grammar to beta if there is a rule A-> r ........

The language generate by the grammar G is defined as L(G) = {w in sigma* : S generates G\*W} (only terminal symbols)

Example: my grammar S -> AB
a -> 0A1 | ^
B -> a

derivation:
alpha = AABA => AAB0A1 // use A->0A1
=> AAa0A1 // use B -> a
=> ... cont...

Hard problems in writing grammar
L = {1^2n 0^3m 1^2m 0^3m : n >= 1, m >= 0}
Grammar (**think outside to inside**):
- find related blocks: n and m
- S -> 11 S 000 | 11 A 000  // to make sure n is MINIMUM 1
- A -> 000 A 11 | $\Lambda$  // m is MINIMUM 0

Another exercise:
L = {w # w^r : w in {a,b}\*}  // w^r is reverse of w
ex: aaba # abaa
Grammar (**OUTSIDE TO INSIDE!**):
- S -> a S a | b S b | #

[FINAL EXAM] Grammar for:
L = {ww^r : w in {a,b}\*}

Example:
L = {w in {a,b}* : w = w^r}
Grammar:
- ex: aabaa
- S -> a S a | b S b | a | b | ^

Example (Super-Hard) (NOT in final):
L = {w : w in {a,b}\*, # a(w) = # b(w)}
- ex: aababbbabbaa in L
	- aababbbabbaa: 6 a's, 6 b's
	- ababbbabbaa: 5 a's, 6 b's
	- babbbabbaa: 4 a's, 6 b's
	- abbbabbaa: 4 a's, 5 b's
- Disaster case: aaaaabbbbb: 5 a's, 5 b's ... 4 a's, 5 b's ... 0 a's, 5 b's
- S : # a = # b
- B : # b = # a + 1
- A : # a = # b + 1
- BB : # b = # a + 2
- AA : # a = # b + 2
- ...
Grammar:
- S -> aB | bA | ^
- B -> aBB | bS | b
- A -> bAA | aS | a

[FINAL EXAM] CNF (Chomsky Normal Form):

all rules are in the form of A -> BC  // one nonterminal to two nonterminals
or A -> a  // or one terminal to one terminal

How to do the translation:
1. Eliminating ^-production rules (A -> ^) by figuring out what info is lost; what transitions going to lambda are lost? Simply deleting A -> ^ from grammar is not okay. Patiently remove states that could reach lambda in all possible combinations, except for identities
2. Elimination of unit production rules; single nonterminals to single nonterminals (i.e. A → B). Find what info was lost. Replace single nonterminals with their new definition after removing. (A → a; B → BA | b; A → BA | b)
3. CNF conversion! [15% in final]

Example:
- S -> AASB | AS | BS | ab
- A -> AB | a | ^  // lambda production here
- B -> BA | A | b
- Translation:
	1. After ^ -elim:
		- S -> AASB | ASB | SB | AAS | AS | BS | ab  // S -> S is meaningless, don't include
		- A -> AB | B | a
		- B -> BA | A | b
	2. 

Example:
- S -> aS | bS | AS | BS
- A -> aAbS | AB | a
- B -> Ba | b
- Convert to CNF:
	1. Step one:
		1. E -> a
		2. F -> b
		3. S -> ES | FS | AS | BS
		4. A -> EAFS | AB | a
			- Handle/fix EAFS:
			- replacing it with:
			- A -> EH
			- H -> AI
			- I -> FS
			- So A -> EH
		5. B -> BE | b
	2. Done!

Example:
- S -> AaBB | ab
- A -> aBB | a
- B -> bAb | b
- Convert to CNF:
	1. Step 1:
		1. E -> a
		2. F -> b
		3. S -> AEBB | EF
			- Work on AEBB:
			- S -> EH
			- H -> EI
			- I -> BB
		4. A -> EBB | a
			- Work on EBB:
			- A -> EI
			- B -> b
			- B -> FJ
			- J -> AF
			- So A -> AEI
		5. B -> FAF | b

"This called capitalism!"

Summary:
- previously: reg lang (set of strings) $\leftrightarrow$ FA (program) by kleene thm,
- now: c.f. languages () $\leftrightarrow$ FA by pushdown automata

Pushdown Automata (PDA) = FA + Pushdown Stack

Pushdown Stack:
- z_0 is bottom of stack, don't pop
- push pop operations on top element
- dynamic array operated at one end
- unbounded storage (no upper bound)

We know {a^n b^n : n >= 1} is c.f.
We construct a PDA to accept it
- M is FA, reading input in one direction in read-only
- pushes and pops input to stack
- if stack empty, yes!
- a push and b pop?
- crash if try to pop bottom off (i.e. aabbbb for prev. lang?)

Try {a^n b^(n+1) : n >= 1} is c.f.
- e.x. aaabbbb
- at last b when you see top of stack is z_0, you can simply move right one and check if end to see if num a's = num b's + 1

His construction of PDA in {a^n b^n : n >= 1}:
- Constructs PDA M to accept the language as follows:
	- On input w, M keeps reading a's.
	- For each a read, M pushes an a onto the stack.
	- Then, M keeps reading b's.
	- For each b read, M pops an a from the stack.
	- At the end of input w (ancient times \$), M says yes on w if the stack is empty (z_0 is the top symbol on the stack)
	- Clearly, M accepts the language

His construction of PDA in {a^n b^n+1 : n >= 1}:
- Constructs PDA M to accept the language as follows:
	- On input w, M keeps reading a's.
	- For each a read, M pushes an a onto the stack.
	- Then, M keeps reading b's.
	- For each b read, M pops an a from the stack.
	- When M sees that the top item in the stack is z_0, M reads exactly one more b. If this is the end of the input, then M says yes.
	- Clearly, M accepts the language.

PDA is of infinite states, so don't try drawing state machines

PDA is nondet. So you can guess. DPDA is deterministic version.
Reminder: nondet. alg never say no, only crash.

Construct a PDA accepting {ww^r : w in {a,b}\*}
- Always draw machine with stack and example.
	- abaab baaba
	- Push all until middle, then start comparing (read and pop, compare).
	- You have to guess where middle. Your guess is never wrong.

In formal definition of PDA, $\delta$ is the only thing that matters; it's our program

Example: 
- $\delta$: Q x ($\Sigma$ $\cup$ {$\Lambda$}) x $\Gamma$ → $2^{Q x \Gamma^r}$
- $\delta$(q,a,b) = {(p, ab)}
- In English: if current state is q, we are reading input symbol a, top of stack is b, then the next state is p and we push a onto the stack.

Example:
- delta(q,a,b) = {(p, lambda)}
- In English: if current state is q, we are reading a is input and top of stack it b, then next state is p and replace top of stack (b) with nothing (pop)

Example:
- delta(q,a,b) = {(p,b)}
- In English: if current state is q, we are reading a as input and top of stack is b, then we switch state to p without doing anything to the stack

Example:
- delta(q,a,b) = {(p, ab), (q, lambda)}
- In English: if current state is q, we are reading a as input and top of stack is b, then we make nondet. choice to change state and modify stack

M runs on input word w from initial state and with empty stack (stack top = $z_0$). If there is a run ending with an accepting state, we say M accepts (says yes on) w.
L(M) = {w : M accepts w} is **the** langauge accepted by M.

Writing code (explicit instruction):
- For a PDA accepting L = {a^n b^n : n >= 1}
- delta(q_0, a, z_0) = {(q_0, a z_0)}  # Push a onto the stack
- delta(q_0, a, a) = {(q_0, aa)}  # push another a onto the stack
- delta(q_0, b, a) = {(q_1, lambda)}  # pop an a from stack when reading b and top is a
- delta(q_1, b, a) = {(q_1, lambda)}  # keep popping the a's while reading b's
- delta(q_1, lambda, z_0) = {(q_happy, z_0)}  # q_happy is accepting state
- [INSTRUCTIONS ABOVE NOT ORDERED, can happen in any order]

Difficult example (too much for final exam):
- Construct a PDA to accept L = {w in {0,1}* : # a (w) = # 1 (w)}  # so the number of 0's = number of 1's
- Two modes:
	- mode 1:
		- See 0: push
		- See 1: pop
	- mode 2:
		- See 0: pop
		- See 1: push
	- When to switch current mode: empty stack
- delta(q_0, lambda, z_0) = {(q_1, z_o), (q_2, z_0)}  # guess one of the two modes to go
- delta(q_1, 1, a) = {(q_1, lambda)}  # pop top of stack when reading 1 in mode 1
- delta(q_1, 0, \*) = {(q_1, a*)}  # push a on top of stack when reading 0 in mode 1, with any symbol atop stack
	- \* in {z_0, a}
- delta(q_1, lambda, z_0) = {(q_1, z_0)}  # stack is empty, go back to q_0 to guess
- delta(q_2, 1, \*) = {(q_2, a*)}, \* in {z_0, a}
- delta(q_2, 0, a) = {(q_2, lambda)}  # pop from stack when reading 0
- delta(q_2, lambda, z_0) = {(q_0, z_0)}  # go back to q_0 to guess mode
- q_0 is accepting state

Final exam question:
1. Write explicit construction (code) of PDA accepting L = {a^2n, b^3m : n,m >= 1}
	1. ...
2. Write ... accepting L = {a^2n b^3n : n >= 1}
	1. ...
3. Write ... accepting L = {w in {0,1}* : w = w^r}
	1. Example word: 00111011100
		1. push until middle `00111`
		2. skip odd middle if needed `0`
		3. then pop until empty stack `11100`
	2. Code:
		1. delta(q_0, \*1, \*2) = {(q_0, \*1 \*2)}, \*1 in {0, 1}, \*2 in {0, 1, z_0}
		2. delta(q_0, lambda, \*) = {(q_even, \*), (q_odd), \*}, \* in {0, 1}  # we guess the length of input
		3. delta(q_odd, \*1, \*2) = {(q_even), \*2}  # skip the middle
		4. delta(q_even, \*, \*) = {(q_even, lambda)}, \* in {0, 1}  # input matches top of stack
		5. delta(q_even, lambda, z_0) = {(q_happy, z_0)}  # accepting state

