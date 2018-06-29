Functional programming is an approach based on function calls. It forms a bridge between formal methods in computing and their application.

#### Names and values in imperative / functional languages

Imperative languages are based in the idea of variables as an association between a name and values. Imperative in this sense means sequence of commands.

    <cmd1>
    <cmd2>
    <cmd3>
    <cmd4>

Through assignments, variables can change their values.

    <name> := <expr>

In functional languages, function calls form the basis. One expression (function call) calls other functions in turn:

    <func1>(<func2>(<func3> ...))

The above is known as function composition or nesting.

<u>In imperative languages, the same name may be associated with different values.</u>

<u>In functional languages, a name is only ever associated with one value.</u>

#### Execution order in imperative and functional languages

In imperative languages, the order of commands matters. If the order is changed, the behavior of the whole program may change as well. The output of:

```pascal
T := X
X := Y
Y := T
```

is completely different than:

```pascal
X := Y
T := X
Y := T
```

<u>In general, imperative languages have fixed command execution orders.</u>

Pure functional languages lack assignment so their values never change. Thus, there are no side effects and no fixed execution order.

Repetition in imperative and functional languages

In imperative languages, names are often reused for multiple assignments. Example:

```pascal
SUM1 := A[1]
SUM2 := SUM1 + A[2]
SUM3 := SUM2 + A[3]
```

Instead of create N sum variables, we'd use a loop and reuse one single variable, called `SUM`

```pascal
I := 0;
SUM := 0;
WHILE I < N DO
BEGIN
  I := I + 1
  SUM := SUM + A[I]
END
```

In functional languages, no reuse of variables is possible. So, we'd need to use mechanisms like recursive function calls to repeatedly create new versions of names associated with values. Example:

```pascal
FUNCTION SUM(A: ARRAY [1..N] OF INTEGER; I, N: INTEGER): INTEGER;
BEGIN
  IF I > N THEN
    SUM := 0
  ELSE
  	SUM := A[I] + SUM(A, I + 1, N)
END
```

#### Data structures in functional languages

<u>Functional languages provide explicit representations for data structures</u>

These are based on recursive notations where operations on a whole structure are described in terms of recursive operations on substructures.

Imperative languages can manipulate global structures, without passing them as parameters. In functional languages, there is no assignment, so it is not possible to change substructures of global data structures. Instead, entre data structures are passed, changed, and the entire data structure is returned.

#### Functions as values

In functional languages, functions may construct new functions and pass them on to other functions. This is commonly illegal in imperative languages.

#### λ calculus

It is based on function abstraction and application: the former is used to generalize expressions through names and the latter evaluates generalized expressions by giving names particular values.