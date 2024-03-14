A Dusa Quine
============

Dusa is a language in the vein of Datalog, deriving sets of solutions from rules. It's very simple, and doesn't have any notion of output beyond the set of facts that match the rules given.

So what does a Dusa quine look like? One natural choice is to try to make one of the output facts the string representing the program.  With the recent update allowing escaped characters in strings, this is possible!

(Before escaped characters were allowed, we could have used a different encoding; e.g., a giant number whose base-128 encoding represents the ASCII characters making up the program.  This should be theoretically feasible since Dusa uses bigints, but the limited builtin math operations and full-solution generation seems to make math for numbers beyond a few digits prohibitively slow.  See the `experiments` directory for thoughts on that.

Building a quine (in python)
----------------------------

To get a sense of the Dusa (near) quine here, we build it up in Python.

### Using repr

A standard quine construction is to assign a representation of the text of the program to a variable, then print it once to assign it, then once to print the program.  That's not too clear, but the following python quine hopefully is:

```python
prog = 'print("prog = " + repr(prog) + "\\n" + prog)'
print("prog = " + repr(prog) + "\n" + prog)
```

It prints out the literal string `"prog = "`, then the string representation of prog.  Then (after a newline) it prints out prog, which is exactly the second line.

Unfortunately, this heavily leans on python's `repr` function to do all the hard work of encoding the string.  Dusa has no such convenience, so we have to do a bit more work.

### Manually escaping the string

Luckily, it's not that hard to print an escaped string.  In this case, we can use mixed single and double quotes to avoid most escaping; we just need to escape the backslash for the newline.

```
[insert python code here]
```

### Manually escaping the string

But that requires string manipulation, which is lacking in dusa.  However, Dusa does support a sort of array; by expanding out the string into an explicit array, we can do effectively the same thing:

```
[insert python code here]
```

This is what the Dusa quine does.

Building a quine in Dusa
-----------------------

Fundamentally, we do the same thing.  We can create an "array" in Dusa by using a simple rule mapping numbers to characters:

```
array 0 is "h".
array 1 is "e".
array 2 is "l".
array 3 is "l".
array 4 is "o".
```

We can then "loop" over this by using a recursive-ish rule:

```
str 5 is "".
str N is (concat C Rest) :- str (plus N 1) is Rest, array N is C.
```
Whoops, that doesn't work because of how Dusa infers; having a (plus N 1) on the right hand side doesn't work.  So switch the math to the LHS.  Also, we need to define the builtins... :
```
#builtin INT_MINUS minus
#builtin STRING_CONCAT concat
str 4 is "".
str (minus N 1) is (concat C Rest) :- str N is Rest, array N is C.
```
This produces, among other facts, `str -1 is "hello"`.  By tweaking this a bit, we can do escaping and other fun things to make the whole quine.

Notably, we need to be able to convert an integer to a string.  This took a bit of fun math, which is currently left as an exercise to the reader.
