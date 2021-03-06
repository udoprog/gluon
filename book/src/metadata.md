# Metadata

Sometimes we need a way to associate some extra information to a specific named binding and have it be visible whenever we refer to that name. The most common reason for this is documentation comments. When we write some documentation for a binding we would like this documentation to be visible whenever someone uses that binding.

For this reason gluon runs a "metadata" pass on all code in which "metadata" (such as documentation comments) gets statically propagated throughout the code.

```f#
/// Adds one to the argument `x`
let add1 x = x + 1

/// Adds two to the argument `x`
let add2 x = x + 2

add1 // Looking up the metadata of this variable yields the documentation of `add1`

// It can't be statically determined which branch the `if` takes (since constant folding do not
// take place). Thus `addN` do not get any metadta from either `add1` or `add2`
let addN = if True then add1 else add2
addN
```


## Attributes

In addtion to documentation comments gluon also has a special notion of attributes that get propagated in the same manner. These are specified using the following syntax.

```f#
/// @<NAME> <VALUE?>
```

### @infix

```f#
/// @infix (left|right) <NON-NEGATIVE INTEGER>
```

The `@infix` attribute is used to specified the fixity and precedence of infix operators. This lets us specify that multiplication binds tighter that addition.

```f#
/// @infix left 6
let (+) ?num : [num a] -> a -> a -> a = num.(+)
/// @infix left 7
let (*) ?num : [num a] -> a -> a -> a = num.(*)
```


### @implicit

```f#
/// @implicit
```

The `@implicit` attribute is used to mark value bindings or type bindings as usable for implicit resolution. If specified on a value binding then only that specific binding can be used on implicit resolution. If specified on a type binding then all bindings that has that type can be used in implicit resolution.

```
// Can be used as an implicit argument
/// @implicit
let binding : MyType = ..

/// @implicit
type Eq a = { (==) : a -> a -> Bool }

// Can be used as an implicit argument
let eq_Int : Eq Int = ..
```
