#### Abstraction

It involves generalization from concrete instances of a problem so that a general solution may be formulated.

Imagine that you buy 9 items for 10 cents each:

```
10*9
```

Now imagine that you buy 11 items at 10 cents each:

```
10*11
```

We can abstract this with:

```
10*items
```

Next, we can make the abstraction explicit with:

```
REPLACE items IN 10*items
```

We have made a function from a formula by replacing an object with a name and identifying the name that we used.

If the price goes up to 11 cents:

```
REPLACE items IN 11*items
```

And if drops to 9 cents:

```
REPLACE items IN 9*items
```

So, we can abstract again:

```
REPLACE cost IN
  REPLACE items IN cost*items
```

We abstracted over two operands in the formula. To evaluate, we need to supply both values. So 12 items at 32 cents will be:

```
REPLACE cost WITH 32 IN
  REPLACE items WITH 12 IN cost*items

...

REPLACE items WITH 12 in 32*items

...

32*12
```

If we decide to calculate a different problem, say how much each item costs given the total cost and the number of items:

```
REPLACE cost IN
  REPLACE items IN cost/items
```

However, the above abstraction is similar to the original one, except for the operation (* and /). We can then abstract the operand as well:

```
REPLACE op IN
  REPLACE cost IN
    REPLACE items IN cost op items
```

<u>Abstraction is based on generalization through the introduction of a name to replace a value and specialization through the replacement of a name with another value.</u>

#### λ expressions

A λ expression may be a name to identify an abstraction point, a function to introduce an abstraction or a function application to specialize an abstraction:

```
<expr> ::= <name> | <func> | <application>
```

A name may be any sequence of non-blank characters.

A λ function is an abstraction over a λ expression:

```
<func> ::= λ<name>.<body>

where

<body> ::= expression
```

Example:

```
λx.x
λfirst.λsecond.first
λf.λa.(f a)
```

The '.' separates the name from the expression in which the abstraction takes place.

A function application has the form:

```
<application> ::= (<func expr> <arg expr>)

where

<func expr> ::= <expr>
<arg expr> ::= <expr>
```

Example:

```
(λx.x λa.λb.b)
```

#### Simple λ functions

##### Identify function

```
λx.x
```

The identity function returns the argument to which it is applied. Its bound variable is `x` and its body expression is the name `x` .

If you pass the identity function to itself:

```
(λx.x λx.x)
```

Then the bound variable `x` for the expression `λx.x` is replaced by the argument expression `λx.x` in the body expression `x` resulting in `λx.x` .

In arithmetic, adding or subtracting zero or multiplying or dividing by one are identity operations.

##### Self-application function

Consider the following:

```
λs.(s s)
```

Which applies its argument to its argument. The bound variable `s` and the body expression `(s s)` which has the name `s` as function expression and the same name `s` as argument expression.

If we apply the identity function to it:

```
(λx.x λs.(s s))
```

The function expression is `λx.x` and the argument expression is `λs.(s s)`. When this application is evaluated, the function expression bound variable `x` is replaced by the argument `λs.(s s)` in the function body `x` giving `λs.(s s)` which is the original argument.

Now let's apply the self-application function to the identity function:

(λs.(s s) λx.x)

The function expression is `λs.(s s)` and the argument expression is `λx.x` . When this application is evaluated, the function expression bound variable `s` is replaced by `λx.x` in the function expression body `(s s)` giving a new application `(λx.x λx.x)` with function expression `λx.x` and argument expression `λx.x` which results in `λx.x`.

If we apply the self-application function to itself:

```
(λs.(s s) λs.(s s))
```

This application has function expression `λs.(s s)` and argument expression `λs.(s s)`. The expression bound variable `s` is replaced by the argument `λs.(s s)` in the function expression body `(s s)` giving `(λs.(s s) λs.(s s))` with function expression `λs.(s s)` and argument expression `λs.(s s)` . 

This continues on and on, without ever completing.

##### Function application function

Consider this:

```
λfunc.λarg.(func arg)
```

The bound variable is `func` and the body expression is `λarg.(func arg)` which has the bound variable `arg` and the body expression `(func arg)`. This in turn has the function name `func` and `arg` as argument expression.

The above function returns a second function which then applies the first function's argument to the second function's argument.

If we apply the identity function to the self-application function using the above:

```
((λfunc.λarg.(func arg) λx.x) λs.(s s))
```

The function expression is `(λfunc.λarg.(func arg) λx.x)` which must be evaluated first. The bound variable `func` is replaced by `λx.x` in the body `λarg.(func arg)`  resulting in `(λarg.(λx.x arg) λs.(s s))`. 

Next, the bound variable `arg` is replaced by `λs.(s s)` in the body `(λx.x arg)` resulting in `(λx.x λs.(s s))` which, as demonstrated earlier, results in `λs.(s s)`.

##### Notations for naming functions and **β** reduction

We'll use the following to name functions:

```
def <name> = <function>
```

Examples:

```
def identity = λx.x
def self_apply = λs.(s s)
def apply = λfunc.λarg(func arg)
```

In addition, we'll also use `==` to indicate replacement of a `<name>` by its associated `<function>`:

```
(<name> <argument>) == (<function> <argument>)
```

The replacement of a bound variable with an argument in a function body is called β reduction:

```
(<function> <argument>) => <expression>
```

The above means that the `<expression>` results from the application of the `<function>` to the unevaluated `<argument>`.

##### Functions from functions

```
def identity = λx.x
def apply = λfunc.λarg.(func arg)
def identity2 = λx.((apply identity) x)

(identity2 identity) == (λx.((apply identity) x) identity) =>
((apply identity) identity) == ((λfunc.λarg.(func arg) identity) identity) =>
(λarg.(identity arg) identity) =>
(identity identity) => ... =>
identity
```

