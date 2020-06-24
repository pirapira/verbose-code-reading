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

### 2020-05-19

* hash_intf.ml the constraints look like better encoded in linear type.
* read hash_intf.ml
* what is [@@noalloc]? fast C call, when no bad side-effects
* what is indurect function call in ocaml? other C calls.
* read hash.ml
* read Hash.Builtin
* read Ppx_compare_lib.Builtin
* read Int_replace_polymorphic_compare

### 2020-05-20

* read import.ml
* mandatory: is the label optional or mandatory in val map  : 'a t -> f:('a -> 'b)   -> 'b t (seen in https://github.com/janestreet/ppx_let)?
* what is t Ctypes.typ? https://github.com/ocamllabs/ocaml-ctypes/blob/master/src/ctypes/ctypes_static.mli#L29
* read bindings_base.ml
* read Ctypes.FOREIGN https://github.com/ocamllabs/ocaml-ctypes/blob/master/src/ctypes/ctypes.mli#L485 vaguely remembered what it was.
* "beyond was is leaked by the output z" should be "beyond what is..." in https://www.usenix.org/system/files/conference/usenixsecurity14/sec14-paper-ben-sasson.pdf
* "cross-over points?"
* it might be interesting to formalize vnTinyRAM
* they say subring of degree <= d. But that's not closed for multiplication. https://www.usenix.org/system/files/conference/usenixsecurity14/sec14-paper-ben-sasson.pdf
* bilinear doen't look like bilinear. https://www.usenix.org/system/files/conference/usenixsecurity14/sec14-paper-ben-sasson.pdf
* ceil and floor confused. must be ceiled all the way.
* it's impossible to distinguish long string with zero's and short strings
* read backend_types.mli
* undestand R1CS constraint system; itself is not so hard.
* why is func_name a function from string to string in backend_types.mli? (adding prefix)
* understand Camlsnark_c.Backend_types.Var.t: just field_unconstrained. the meaning depends on usage.
* read backend_types.mli
* read backend_tpes.ml
* Binable? from Bin_prot
* libsnark.ml: I vaguely remember calling these profiling configs
* find group_coefficients in libsnark. could not find interesting things.
* understand Camlsnark_c.Bindings.Group_coefficients (Fq); almost. not seeing why there are two: a and b.
* window seems like multiexponentiation gadget
* what are G and V in the window table?  G is perhaps the generator.  V is perhaps the window table itself?  Vector.
* understand Camlsnark_c.Bindings.Window_table
* libsnark.ml looks lots of boilerplate, but understandable given this is a wrapper
* deduplicate? what to do?
* read libsnark.ml; only scanned.
* understand the definition of schedule_delete'ed
* why are things schedule_delete'ed? OCaml GC cannot invoke C++ delete
* read Snarky.mli; doesn't exist
* read Snarky.ml; doesn't exist
* what does [@@deriving enum] do? association with int

### 2020-05-21
* what is .. in constraint.ml?  object type!!! row variable!!!  no. that was https://caml.inria.fr/pub/docs/manual-ocaml/extensiblevariants.html
* read about roh variable in ocaml object... found a paper
* row variables provide parametric polymorphism for method invocation. ok.
* read http://caml.inria.fr/pub/papers/remy_vouillon-objective_ml-tapos98.pdf half way
* read Field_intf.ml
* why does the `map` is covariant on constraints in constraint.ml?
* module as a value. --> first class modules hah.
* read https://dev.realworldocaml.org/first-class-modules.html
* sent a PR removing Conv, which seems never used
* read Constraint
* read https://caml.inria.fr/pub/docs/manual-ocaml/privatetypes.html
* what is private type?
* read boolean.mli
* read Boolean
* why are there Cvar.t and Cvar.cvar?
* to_constant_and_terms might return the same variable multiple times
* read Comparable.S
* Parametrised Notions of Computation looks like fun.

### 2020-05-22

* why is can_mutate_state mutable? weird.
* sent a PR about it
* read about Map.singleton
* read about Map.merge_skewed
* read cvar.ml from L132-
* does variable start with zero or one? must not be zero (for the json representation)
* how are Free_monad.Functor.S2 and S3 used?  Like ('a, e') result
* read List0
* read monad_intf.mli
* what is type nonrec?, ah the name is not bound in the body
* read monad.ml
* why is the Let_syntax module type declared again in monad_let.ml?

### 2020-05-23

* monad_let.ml can be probably simplified (like monad.ml) -> filed a PR
  * try to implement Make2 and Make using Make3, in monad_let.ml
* read Monad_let
* try functorize in constraint.ml; this is not so hard
* modifiable field element and immutable field element needs to have different type sigs -> this grows into a very big change, and requires the previous change

### 2020-06-02

* Free_monad.Make1 and Make2 are special cases of Free_monad.Make3 --> that was false
* read Free_monad.Make2
* annotatted types in Store
* read Store
* annotated types in Read
* read Read
* read Alloc
* read Typ_monads
* Had a look at Types. Some VM like thing is going on. Maybe time to read one of the SNARK papers.

### 2020-06-03
* k stands for continuation in Monad_typ
* what are return and result in bindings_base.ml? Maybe a way to deal with the partial application in ML
* Foreign_intf looks reasonable
* read bindings_base.ml
* vector.mli
  * does 'emplace_back' automatically allocate?
  * what does 'get' do in case of out of index access?
* vector.ml
  * "camlsnark_bool_vector" prefix, how is this used?
    * see ./libsnark-caml/libsnark/caml/common.cpp
* read Camlsnark_c.Vector
* what is Bindings (Vector_ffi_bindings) (vector.ml l.3)
  * where does this symbol "Vector_ffi_bindings" come from?
* bin_prot is a good target for formal verification, right? Well, this involves OCaml-C interface.
* what are "extensionally defined OCaml types" (README.md of Bin_prot)
* skimmed through README of Bin_prot
* what is the difference between exception Read_error Empty_type pos and exception Error_type? (perhaps the second doesn't contain a position pos)
* what is c_layout https://caml.inria.fr/pub/docs/manual-ocaml/libref/Bigarray.html
* what does `let ( + ) = ( + )` do in `common.ml`?
* skimmed through Bin_prot.Common
* skimmed through size.mli
* skimmed through size.ml
* read Write
* why do reader and writer return the final positions in different ways?
* opened Shape
* opened Type_class
* read Bin_prot.binable
* had a look at binable.mli in core_kernel
* had a look at binable.ml in core_kernel
* why are schedule_delete functions in the binable functor arguments; ah, to sync OCaml gc and C++ delete
* why not on 64-bit arch, max_array_length is just halved?
* read vector.mli
* read vector.ml
* read type_equal.mli
* skimmed through tppe_equal.ml
* what does Constraint_system_intf.finalize do?
* read Backend_intf.Constraint_system_intf
* read Constraint_system; a dynamic module and its instance.
* request.ml looks a bit too mumble
* what is done in type response += T of a Response.t? Dynamically allocated new response clause?
* but why can handler return this new clause in the inductive definition? Well, the handler is given the respond function, so it's fine...
* read Request
* reading Run_state, I realized that the underlying math needed

### 2020-06-24
* done reading E.  Ben-Sasson,  A.  Chiesa,  E.  Tromer,  and  M.  Virza,  “Succinct  non-interactive zero knowledge for a von neumann architecture.  from 2.4.
* read libsnark's readme
* identify how snarky calls libsnark
    * Backend.make_groth16_proof
    * looks like using R1CS directly

## Stack of TODOs
* read Run_state
* read Types.Typ
* read Typ
* read snrark_intf
* read src/enumerable.mli
* read src/enumerable.ml
* what is Enumerable?
* read Election.ml
* figure out what from Core is used in Election.ml
* read Base
* check out core_kernel v0.12.3
* read Core_kernel
* read Core
* figure out how S2 in applicative.mli is used
* something about linking... what is the module Ignored doing in libsnark.ml?
* why the same module type is defined in bingings_base.ml and somewhere else (libsnrk.ml?)
* read Gannaro et al. [38]
* why are there two bounds a and b? in Camlsnark_c.Bindings.Group_coefficients? I'll see when it's used.
* Field.one should not be modifiable

## Advertisement

[Technical Debt Huting](https://convenience-logician.de/tech-debt.html).
