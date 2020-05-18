reading https://github.com/o1-labs/snarky

## Log

### 2020-05-16

 * created this file.
 * [snarky](https://github.com/o1-labs/snarky) compilation passes with Ocaml 4.07.1
 * [examples/election/election.ml](https://github.com/o1-labs/snarky/blob/master/examples/election/election.ml) imports Core
 * Why is it using Core v0.12 not newer? Because newer Core requires newer OCaml.
 * Why upgrading Core breaks the compilation with (<>)? There is a bigger problem.
 * understand why (<>) became not visible (after importing async). No need to. This only becomes a question with 4.08.0.
* opam pin . shows this:
```
[WARNING] Failed checks on sponge package definition from source at git+file:///home/privat/snarky/snarky#master:
```
  filed a PR against this. [#478](https://github.com/o1-labs/snarky/pull/478)
* checked out [core v0.12.4](https://github.com/janestreet/core/tree/v0.12.4)
* [core/src/core.ml](https://github.com/janestreet/core/blob/v0.12.4/src/core.ml) says it is an extention of Core_kernel
* [core_kernel/README.md](https://github.com/janestreet/core_kernel/blob/master/README.md) says it is an extention of Base
* checked out [base v0.12.2](https://github.com/janestreet/base/tree/v0.12.2)
* [base/README.org](https://github.com/janestreet/base/blob/v0.12.2/README.org)
* base/src/applicative.mli says Applicative_intf.
* opened [base/src/applicative_intf.ml](https://github.com/janestreet/base/blob/v0.12.2/src/applicative_intf.ml)
* finished browsing applicative_inf.ml
* read https://www.cs.cornell.edu/courses/cs3110/2019sp/textbook/data/polymorphic_variants.html
* opened http://staff.city.ac.uk/~ross/papers/Applicative.pdf
* read http://staff.city.ac.uk/~ross/papers/Applicative.pdf
* two monads, a--> tmb --> tmtmc but how to get tmc?
* saw applicative.ml
* saw https://blog.janestreet.com/let-syntax-and-why-you-should-use-it/, what `Let_syntax` modules are
* open! supresses warning against shadowing: https://stackoverflow.com/questions/22607755/what-does-open-mean-in-ocaml

### 2020-05-17

* what happens with module type A = sig
      ...
      module A : sig
         ...
      end
  end
  a module of that type needs to contain a module of that signature.
* read https://github.com/janestreet/ppx_let/README
* understand how to prepare Let_syntax module, just list those functions in a module
* what is happening in Make_let_syntax in applicative.ml? Just duplicating things because the same thing is useful for the let syntax as well as the programming around it.
* opened https://github.com/janestreet/base/blob/v0.12.2/src/import.ml
* opened https://github.com/janestreet/base/blob/v0.12.2/src/import0.ml
* why only lnot looks different? https://github.com/janestreet/base/blob/v0.12.2/src/import0.ml#L362 ah, not a binary operator.
* run tests on base; couldn't figure out how to run tests.
* do an experiment removing include Int_replace ... from Import0; then the compilation failed.
* but removing the include from import.ml seems not to break anything.

### 2020-05-18

* why not (||) https://github.com/janestreet/base/blob/master/src/ppx_compare_lib.mli#L12 because "or" is not used in record comparison
* Int_replace_polymorphic_compare has already been included in Import0, so maybe this is a duplicate https://github.com/janestreet/base/blob/v0.12.2/src/import.ml#L6 --> posted a PR
* are *_replace_polymorphic_compare included somewhere later? yes.
* how does OCaml find symbols, especially, is it like lastly defined? just overwritten in the symbol table? or, adhoc polymorphism allowed? no, just overwritten in the symbol table.
* read https://caml.inria.fr/pub/docs/manual-ocaml/intfc.html 20.1.1
* what does external symb : type = string do? done.
* read Poly0, read
* what does [@ocaml.warning "-3"] do? disable a warning.
* read import0.ml

## Stack of TODOs
* read hash_intf.ml
* read Hash.Builtin
* read Ppx_compare_lib.Builtin
* read Int_replace_polymorphic_compare
* read import.ml
* is the label optional or mandatory in val map  : 'a t -> f:('a -> 'b)   -> 'b t (seen in https://github.com/janestreet/ppx_let)?
* figure out how S2 in applicative.mli is used
* read Base
* check out core_kernel v0.12.3
* read Core_kernel
* read Core
* read Election.ml

## Advertisement

[Technical Debt Huting](https://convenience-logician.de/tech-debt.html).
