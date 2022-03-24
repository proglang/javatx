# Global Type Inference for Featherweight Generic Java - Prototype implementation

* Title of the submitted paper: Global Type Inference for Featherweight Generic Java

* Artifact download: https://github.com/AnonymousArtifact/FGJ-GT/releases/download/ecoop/fgj-gt.zip
    * The code is also made accessible on [github](https://github.com/AnonymousArtifact/FGJ-GT) -> It is possible to pull the code via git as well. The git account used for this repository was freshly created under a pseudonym to comply with the double-blind review process.

* We want to claim an Available Badge

## Implementation of the type inference algorithm

We implemented a prototype of the type inference algorithm described in the paper "Global Type Inference for Featherweight Generic Java".

* Code is written in Scala
* The project compiles to a web-application


## For authors claiming an available badge

The web application will be published on [github](https://github.com).
Github allows to publish static web sites, which our web application is.
The source code will also be published on github under a [MIT](https://opensource.org/licenses/MIT) license.

## Artifact Requirements

* for running:
    * A regular web browser will do. For example the current version of [firefox](https://www.mozilla.org/de/firefox/new/) or [google chrome](https://www.google.com/intl/de_de/chrome/)

* for compiling:
    * scala version >2.13
    * the scala built tool [sbt](https://www.scala-sbt.org/)


## Getting Started

The web application can be started by opening `index.html` in a web browser.
It is also possible to access the web application under the following link:

https://anonymousartifact.github.io/FGJ-GT/index.html

### Howto Use

Just enter a proper FGJ-TI program on the left side and see the type infered result on the right side.
The implementation is currently in a prototype state. It only works if you supply a valid input.
The input has to be according to the syntax given in figure 6 of our paper and satisfy the type rules given in figure 7-9.
If a incorrect input is given the right side will either show no result at all or a short error message.
It also is possible that the algorithm fails with an uncaught exception. This exception is only displayed in the error console of your browser and will not be shown in the web application.

Hints:
* Every class has to have an `extends` clause
    * `class Example extends Object ...` for example
* Every generic variable has to have an `extends` clause as well
    * `class Example<A extends Object> extends Object ...`
* The classes are inferred in the order given in the input
    * the first class can only access its own methods
    * the subsequent classes can also access the methods of their predecessors
* see Example inputs below

## Connection to our paper

The idea of this prototype is to showcase our type inference algorithm for featherweight generic java.
It can help to understand how our algorithm is functioning.
* Take the chapter 7 (complexity) for example.
    * The example in Figure 17 can be copy-pasted into the input field of our web application (or copied here from the examples below).
    By changing the `return` expression of the `sat` method it is possible to see the NP complexity of our algorithm take effect
        * Change it to `o1.nand(v1, v2)` and the solution should appear nearly in an instand
        * Change it to `o1.nand(o1.nand(v1, v2), o2.nand(v2, v3))` and it should take longer to compute a solution or even fail to javascript limitations (depending on your machine and web-browser/javascript-engine)


It also showcases a possible use case scenario for a user who wants to write code in FGJ-GT.
The code would then be equipped with type annotations by our type inference algorithm and converted to regular FGJ code.
This enables a user to write code without type annotations and get the correct types automatically assigned by our type inference algorithm.
The type inference algorithm could be extended to other programming languages similiar to Featherweight Java, like Scala or Java.
A possible tool for programmers could then be similiar to our prototype.

### Differences to the algorithm described in our paper

The algorithm described in our paper is nondeterministic.
The function described in chapter 4 selects only one of the solutions from the Unify function.
Our prototype implementation internally computes every possible solution.
It refines the solutions by filtering out unnecessary and duplicate solutions and then will show every solution by overloading the input methods.

For example the input:

    class Example<A extends Object> extends Object{
    A f;
    m(){ return new Example(this);}
    }

will generate two solutions for the method `m`:

    Example<Object> m() {
        return new Example(this);
    }
     Example<Example<A>> m() {
        return new Example(this);
    }


## Compile it yourself

The project is a standard scala sbt project. If you are familiar with scala you can compile the project to javascript by running `fullLinkJS` in the sbt console.
See the documentation on scala.js: https://www.scala-js.org/doc/tutorial/basic/index.html

# Example inputs

```
class Function<B extends Object, A extends Object> extends Object{
A a;

A apply(B p){
    return this.a;
}
}

class Box <S extends Object> extends Object {
S val;
map(f) {
    return new Box( f.apply( this.val ) ) ;
}
}
```


```
class True extends Object{
}
class False extends Object{
}

class Nand1 extends Object{
False nand(True a, True b){ return new False(); }
}
class Nand2 extends Object{
True nand(False a, True b){ return new True(); }
}
class Nand3 extends Object{
True nand(True a, False b){ return new True(); }
}
class Nand4 extends Object{
True nand(False a, False b){ return new True(); }
}

class SATExample extends Object{
True f;

sat(v1, v2, v3, o1, o2){
    return o1.nand(v1, o2.nand(v2, v3));
}

forceSATtoTrue(v1, v2, v3, o1, o2){
    return new SATExample(this.sat(v1, v2, v3, o1, o2));
}
}
```


```
class Identity extends Object{
  id(a){
    return a;
  }
}

```


```
class List<A extends Object> extends Object{
  A head;
  List<A> tail;
  add( a){
    return new List(a, this);
  }
  get(){
    return this.head;
  }
}

class PrincipleType extends Object {
  function(a){
    return a.add(this).get();
  }
}
```
