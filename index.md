# DFA

### Construct a DFA

<pre>
  	R, m := 256, len(p)
	dfa := make([][]int, R) 
	for i := 0; i < R; i++ {
		dfa[i] = make([]int, m)
	}

	dfa[p[0]][0]] = 1
	for x, j := 0, 1; j < m; j++ { 
		for c := 0; c < R; c++ {
			dfa[c][j] = dfa[c][x]
		}
		dfa[p[j]][j] = j+1
		x = dfa[p[j]][x]

	}
</pre>

### what does x = dfa[p[j]][x] mean?
Few properties to understand X:<br/>
1.X can only increment 1 by 1, but it can drop to 0 or 1 suddenly.<br/>

2.when will X become 1?<br/>
X is initialized to 0<br/>
when we encounter a character in the pattern that is the same as the first character in pattern.

3.when will X increment?<br/>
when character at X+1 is the same as character at p[j]

4.when will X drop to 0?<br/>
when we encounter a character that has not showed up in the pattern before

so for any character (state) in the pattern, its restart state X either be 0 or the index of the same character to the left of it. If there are multiple, it should be the first one.

consider following situations:

<pre>
0 1 2 3 4 5 6 7 <br/>
  A B C E A B C
</pre>

for 1(A), 2(B), 3(C), 4(E), their restart state is 0<br/>
for 5(A), its restart state is 1(A)<br/>
for 6(B), its restart state is 2(B)<br/>
for 7(C), its restart state is 3(C)<br/>

the reason is: if there is a mismatch as following:

s: A B C E A B D<br/>
p: A B C E A B C

the next state should be:

<pre>
s: A B C E A B D<br/>
p:         A B C E A B D<br/>
and then compare D and C
</pre>

