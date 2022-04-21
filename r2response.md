
# Overview

# Change list
- Adding an example showcasing the advantages of generalized methods and the necessary to generate type annotations for each class individually. (see Response to Review A)

- Fixing typos and mistakes pointed out by the reviewers

- Adding examples in section 4 as proposed by Review C

- Clarifying the transformation of cyclic class dependencies in chapter 3.1.

# Response
## Review A

- "My feeling is that a better approach would be to typecheck a class in isolation, generating constraints on the omitted types for parameters and results, and then solving constraints in all possible ways only when putting together classes to form a program"

This approach only works when the whole program is given during the type inference step.

We basically do this by overloading all methods.
No type solution is left out.

By applying type inference to each class individually we generate generalised methods.
If we solved the constraints of all classes in one final step we would have to change the 'GT-CLASS' rule in figure 8.
Our approach allows the typisation of the following program, which would not be possible with the algorithm given in the papers "Polymorphic bytecode: compositional compilation for Java-like languages" and "Type Inference by Coinductive Logic Programming":

    class Identity extends Object{
        id(a){return a;}
    }
    class Example extends Object{
        m1(){return id(new Object());}
        m2(){return id(this);}
    }


- "could you provide an example of how class hierarchy should be modified? For instance, how would you rewrite the example in Figure 11 left side?"

class C1C2Merge {
    m1(){ return new C1C2Merge().m2(); }
    m2(){ return new C1C2Merge().m1(); }
}

- "n the same line: I do not understand whether the function mtype returns a set or a single type"

It returns a single type. This is a typo and will be changed.

# Review B
- "Lack of cyclic class dependencies"

See transformation of cyclic class dependencies in chapter 3.1 and the response to review A above.

# Review C
- "The contributions mention a prototype which is then never discussed again in the paper. Did you base it on an existing codebase for FGJ? What was your experience implementing it? Is the addition of global type inference largely orthogonal?"

We submitted a prototype implementation of the algorithm.
