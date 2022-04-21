
# Overview

Thank you for your response and valuable comments to our paper.
We considered each of your proposals and will make small adjustments to our paper accordingly. (See 'Change list' below)

Please see our answers to your reviews listed below in section 'Response'.

# Change list

Our paper needs no essential changes. We will clarify some parts and add examples as stated below.
These changes are minor and can be applied rather quickly.

The changes:

- Adding an example showcasing the advantages of generalized methods and the necessary to generate type annotations for each class individually. (see Response to Review A)

- Fixing typos and mistakes pointed out by the reviewers

- Adding examples in section 4 as proposed by Review C

- Clarifying the transformation of cyclic class dependencies in
  chapter 3.1.
  
- Add to related work: discussion of  "Polymorphic bytecode: compositional compilation for Java-like languages" and "Type Inference by Coinductive Logic Programming"

- Describing the advantages to previous type inference algorithms in greater detail (as proposed by Review C)

# Response
## Review A

### "not clear whether the restriction that classes cannot be mutually recursive is really unimportant"

It is indeed a restriction of the presented algorithm (and the
prototype implementation) that classes
cannot be mutually recursive. We made this choice to simplify the
presentation. For mutually recursive classes, we would have to
generate the constraints at the same time and generalize analogously
to the way it is done for mutually recursive methods.

###  "better ... typecheck a class in isolation, generating constraints on the omitted types for parameters and results, and then solving constraints in all possible ways only when putting together classes to form a program"

We don't see how this suggestion can be made to work in the context of
Featherweight *generic* Java. We apply a generalization step after
constraint generation for each class individually to obtain generic methods.
For instance, our approach typechecks the following program, which
would - to our understanding - not be possible with the algorithm
given in the papers "Polymorphic bytecode: compositional compilation
for Java-like languages" and "Type Inference by Coinductive Logic
Programming":

```
class Identity extends Object{
    id(a){return a;}
}
class Problem extends Object{
    Problem field;

    method1(){ new Identity().id(new Identity()); }
    method2(){ new Problem(new Identity().id(this)); }
}
```

### "Why should not the original typing be the most general one?"

Indeed, that should be included! We'll make the sentence more precise. 

### "subtyping constraint positively"

That follows from results of Simonet [24]. We'll add the citation.

### "field types"

Field types are determined by the class type, and there might be a
choice of different class types during inference. This is handled by
an or-constraint in the inference algorithm. Solving the or-constraint
exposes all alternatives. 

### "mtype and non determinism"

There is a typo in rule (Fig 8: GT-INVK): should be = instead of $\in$. 
It's still non-deterministic because of the $\in\Pi$ in rule (Fig 9: MT-CLASS).

### "provide an example of how class hierarchy should be modified? For instance, how would you rewrite the example in Figure 11 left side?"

- we'll provide an example for the transformation in lines 329-341.
- Figure 11 cannot be rewritten; it is not a valid input for type
  inference.


## Review B

The reviewer summarizes his criticism as follows:

* Contrast this with the very real disadvantages of the approach, which are at least three-fold:
  * Lack of polymorphic recursion. This one could be accepted in my mind.
  * Lack of cyclic class dependencies. For typical Java like code this is a serious blocker.
  * Multiple typings. This means that principal types are not smallest types, and that in fact inferred types could get very complex. This is in my mind the biggest downside.

### polymorphic recursion

This could be handled as in Haskell by giving type signatures for
those functions that are polymorphically recursive.

### cyclic class dependencies

See response to review A above.

### multiple typings

In full-blown Java, multiple typings cannot be avoided because of
overloading. 

### why do research into this

Compared to languages like Haskell or Scala, Java types are much more
verbose and Java lacks facilities for type abstraction (no type
classes, no implicits, no higher-order polymorphism, not even type
synonyms).

Moreover, we believe our algorithms can be adapted for use for code completion (e.g.,
generating type suggestions for partial code) in an IDE. 

# Review C

### "I find that the related work section and/or the exposition in the paper itself does not cover this aspect satisfactorily [...] the work by Plümicke (et al.) [19-22] seems the most relevant and [...] suggests that similar problems have already been recognized and addressed by earlier work"

Thank you for your remarks, we will look into this.

The advantage of specifying a type inference algorithm for Featherweight Java are the soundness and completeness proofs, which use the typing rules described in chapter 3.

In difference to the algorithm described in Plümicke (et al.)
our type inference algorithm is also able to infer method types with bounded generic types.

For example the input:

```
class Box<A extends Object> extends Object{
  A item;
}

class BoundedGenerics extends Object{
    BoundedGenerics f;
    m(a){return new BoundedGenerics(a.item); }
}
```

will spawn the typing ```<X extends BoundedGenerics> BoundedGenerics m(Box<X> a)```
for the method m.
This is a more general typing than ```BoundedGenerics m(Box<BoundedGenerics> a)```, which would be generated by the algorithm in Plümicke (et al.).

### "The contributions mention a prototype which is then never discussed again in the paper. Did you base it on an existing codebase for FGJ? What was your experience implementing it? Is the addition of global type inference largely orthogonal?"

We submitted a prototype implementation of the algorithm as an artifact.
It can be tested as a web application here:

https://anonymousartifact.github.io/FGJ-GT/index.html

### Specifically, what is the impact of back-tracking and the resulting NP-completeness?

The backtracking can be resolved by overloading methods during the type inference process.
By dealing with overloaded methods we internally keep every possible solution during the processing of multiple classes, therefore no backtracking is needed.
We show this in our prototype implementation (see mention above).

To see an example of overloaded methods try the following input:

```
class Example<A extends Object> extends Object{
    A f;
    m(){ return new Example(this);}
}
```

This will generate the typing:
    
```
class Example<A extends Object> extends Object{
A f;
  Example<Object> m() {
    return new Example(this);
}
  Example<Example<A>> m() {
    return new Example(this);
  }
}
```


For small examples the algorithm is rather quick despite its NP-Completeness.
