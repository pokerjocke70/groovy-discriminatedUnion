Discriminated Unions
====================
Discriminated unions provide support for values that can be one of a number of named cases, possibly each with different values and types. UsesDiscriminated unions are useful for heterogeneous data: 

* Data that can have special cases, including valid and error cases
* Data that varies in type from one instance to another
* As an alternative for small object hierarchies. 

See: [MSDN][DU-msdn],[Wikipedia][DU-wikipedia]. 


##A Groovy Discriminated Union
### `Option` Type
Let's say we want a variable `option` to hold some value (`Some`) or no value (`None`). Let's start with `Some`:

```groovy
def option = Option.Some( 100 )
```
We can test for `Some` vs. `None` like this:

```groovy
option.match {
  when Some then { println "We have 'some' value" }
  when Some then { println "It's None!" }
}
```

We can get the value of an `Option` like this:

```groovy
def value = option.match {
  when Some then { it }
  otherwise null
}
assert 100 == value
```

where `otherwise` is a *catch-all* matcher. When an Option is matched with Some, the value held by Some is injected into the `then` closure (if a closure is provided).

It follows that:

```groovy
assert Option.None().match{
  when Some then false
  when None then true
  otherwise false
}
```

##About the `match` method
* A `match` method is defined for `DiscriminatedUnion` and is available to its descendants (e.g. Option). 
* We also inject a `match` method into `List` for list based pattern matching.
* `match` matches one and only one `when` pattern or `othewise` if no matching `when` was found.
* `match` returns the result of the matched pattern, i.e. the argument to `then`. If a closure is supplied, arguments are injected into the closure depending on the matched pattern, the closure is evaluated and its result returned. In the case of a non-closure argument, the argument is returned.  *Note* that any non-closure expression in the entire `match` block is evaluated even if it is part of a pattern that has not matched. When in doubt, enclose `then` arguments in a closure, or expect interesting side-effects.
* 

###Option Type

### Some Option
The `Some` case of the Option type has a value whose type is represented by the type of the value held by the option.


### None Option
The `None` case has no associated value. Note that the result of the matched statement is returned by `match{}`

    def option = Option.None()
    def value = option.match {
        when Some then 100
        when None then null
        otherwise 100
    }
    assert null == value

###Return Value


##Trees
    
    assert new Tree(100).match {
        when Tip then false
        when Tree then true
        otherwise false
    }

    assert new Tip().match {
        when Tip then true
        when Tree then false
        otherwise false
    }

##Credits
The core matcher functionality comes from [graphology][graphology].

[graphology]: https://github.com/will-lp/graphology-case-match
[DU-msdn]:http://msdn.microsoft.com/en-us/library/dd233226.aspx
[DU-wikipedia]:https://en.wikipedia.org/wiki/Tagged_union


