reading https://github.com/o1-labs/snarky

[LOG]
2020-05-16
 * created this file.
 * compilation passes with Ocaml 4.07.1
 * examples/election/election.ml imports Core
 * Why is it using Core v0.12 not newer? Because newer Core requires newer OCaml.
 * Why upgrading Core breaks the compilation with (<>)? There is a bigger problem.
 * understand why (<>) became not visible (after importing async). No need to. This only becomes a question with 4.08.0.
* opam pin . shows this: [WARNING] Failed checks on sponge package definition from source at git+file:///home/privat/snarky/snarky#master:
     filed a PR against this. [#478](https://github.com/o1-labs/snarky/pull/478)
* checked out core v0.12.4
* core/src/core.ml says it is an extention of Core_kernel
* core_kernel/README.md says it is an extention of Base
* checked out base v0.12.2
* base/README.org
* base/src/applicative.mli says Applicative_intf.
* opened base/src/applicative_intf.ml
* finished browsing applicative_inf.ml
* read https://www.cs.cornell.edu/courses/cs3110/2019sp/textbook/data/polymorphic_variants.html
* opened http://staff.city.ac.uk/~ross/papers/Applicative.pdf
* read http://staff.city.ac.uk/~ross/papers/Applicative.pdf
* two monads, a--> tmb --> tmtmc but how to get tmc?
* saw applicative.ml
* saw https://blog.janestreet.com/let-syntax-and-why-you-should-use-it/, what `Let_syntax` modules are

[Stack of TODOs]
* what is happening in Make_let_syntax in applicative.ml?
* read Base
* check out core_kernel v0.12.3
* read Core_kernel
* read Core
* read Election.ml

## Advertisement

[Technical Debt Huting](https://convenience-logician.de/tech-debt.html).
