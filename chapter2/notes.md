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

##### Selecting the first of two arguments

Consider:

```
def select_first = λfirst.λsecond.first
```

The bound variable is `first` and the body is `λsecond.first`. Through replacement and reduction when applying identity and apply:

```
((select_first identity) apply) == ((λfirst.λsecond.first identity) apply) =>
(λsecond.identity apply) =>
apply
```

This function always returns the first argument.

##### Selecting the second of two arguments

Consider:

```
def select_second = λfirst.λsecond.second
```

The bound variable is `first` and the body is `λsecond.second`. Through replacement and reduction when applying identity and apply:

```
((select_second identity) apply) == ((λfirst.λsecond.second identity) apply) =>
(λsecond.second apply) =>
apply
```

This function always returns the second argument. In fact, `select_second` applied to anything returns a version of identity.

```
(select_second <arg>) == (λfirst.λsecond.second identity) =>
λsecond.second

... replacing second with x ...

λx.x
```

##### Making pairs from two arguments

Consider:

```
def make_pair = λfirst.λsecond.λfunc.((func first) second)
```

The bound variable is `first` and the body is `λsecond.λfunc.((func first) second)`. Example:

```
((make_pair identity) apply) == ((λfirst.λsecond.λfunc.((func first) second) identity) apply) =>
(λsecond.λfunc.((func identity) second) apply) =>
λfunc.((func identity) apply)
```

If the above result is applied to `select_first`:

```
(λfunc.((func identity) apply) select_first) == ((select_first identity) apply) ==
((λfirst.λsecond.first identity) apply) =>
(λsecond.identity apply) =>
identity
```

In the case when the result is applied to `select_second`:

```
(λfunc.((func identity) apply) select_second) == ((select_second identity) apply) ==
((λfirst.λsecond.second identity) apply) =>
(λsecond.second apply) =>
apply
```

#### Free and bound variables

If the bound variables in functions are distinct from one another, then the substitution is straightforward. Consider this:

```
(λf.(f λx.x) λs.(s s))
```

There are 3 functions. The first one has bound variable `f`, the second has bound variable `x` and the third has bound variable `s`. So:

```
(λf.(f λx.x) λs.(s s)) =>
λs.(s s) λx.x =>
λx.x λx.x =>
λx.x
```

If the bound variables in different functions were the same, the results would still be the same. So:

```
(λf.(f λf.f) λs.(s s)) =>
λs.(s s) λf.f =>
λf.f λf.f =>
λf.f
```

For an arbitrary function:

```
λ<name>.<body>
```

the bound variable `<name>` may correspond to occurrences of `<name>` in `<body>` and nowhere else. A variable is said to be bound in the body of a function for which it is a bound variable, provided no other functions within the body introduce the same bound variable. Otherwise, it is said to be free.

In `λx.x`, `x` is bound but in `x`, `x` is free. In `λf.(f λx.x)` `f` is bound, but in the expression `(f λx.x)`, `f` is free.

Consider:

```
λf.(f λf.f)
```

The body of the function is `(f λf.f)`. The first `f` is free while the second `f` is bound and so is distinct from the first `f`.

So, a variable is bound in an expression if:

(1) The expression is an application:

```
(<func> <arg>)
```

and the variable is bound in `<func>` or `<arg>`. Example: the variable `convict` is bound in`(λconvict.convict fugitive)`  and in `(λprison.prison λconvict.convict)`.

(2) The expression is a function:

```
λ<name>.<body>
```

Either the variable name is `<name>` or it is bound in `<body>`. Example: `prisoner` is bound in `λprisoner.(number6 prisoner)` and in `λprison.λprisoner.(prison prisoner)`.

A free variable is:

(1) the expression is a single name:

```
<name>
```

and the variable's name is `<name>`.

(2) The expression is an application:

```
(<function> <argument>)
```

and the variable is free in `<function>` or in `<argument>`. Example: `escaper` is free in `(λprisoner.prisoner escaper)` and in `(escaper λjailor.jailor)`

(3) The expression is a function:

```
λ<name>.<body>
```

and the variable's name is not `<name>` . Example: `fugitive` is free in `λprison.(prison fugitive)` and in `λshort.λsharp.λshock.fugitive`.

With the above, it is possible to re-define β reduction: for the normal order of β reduction of an application:

```
λ<name>.<body> <arg>
```

we replace all free occurrences of `<name>` in `<body>` with `<arg>`. Example:

```
(λf.(f λf.f) λs.(s s)) =>
(λs.(s s) λf.f) =>
(λf.f λf.f) =>
λf.f
```

