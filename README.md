# repro

This is minimal repro issue while installing scroll zkevm circuits in a cargo project.

```
git clone <this repo>

$ cargo check
```

<details>

<summary>Click to see the error</summary>

```
error[E0277]: the trait bound `<C as CurveAffine>::ScalarExt: ff::PrimeField` is not satisfied
  --> /Users/sohamzemse/.cargo/git/checkouts/halo2-189ea3cf7f17e3ad/1c21e5b/halo2_proofs/src/transcript/poseidon.rs:18:12
   |
18 |     state: Poseidon<C::ScalarExt, POSEIDON_T, POSEIDON_RATE>,
   |            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ the trait `ff::PrimeField` is not implemented for `<C as CurveAffine>::ScalarExt`
   |
note: required by a bound in `Poseidon`
  --> /Users/sohamzemse/.cargo/git/checkouts/poseidon-9a32cea385ed9159/0d2fb81/src/poseidon.rs:9:24
   |
9  | pub struct Poseidon<F: PrimeField, const T: usize, const RATE: usize> {
   |                        ^^^^^^^^^^ required by this bound in `Poseidon`
help: consider introducing a `where` clause, but there might be an alternative better way to express this requirement
   |
17 | pub struct PoseidonRead<R: Read, C: CurveAffine, E: EncodedChallenge<C>> where <C as CurveAffine>::ScalarExt: ff::PrimeField {
   |                                                                          +++++++++++++++++++++++++++++++++++++++++++++++++++

error[E0277]: the trait bound `<C as CurveAffine>::ScalarExt: ff::PrimeField` is not satisfied
   --> /Users/sohamzemse/.cargo/git/checkouts/halo2-189ea3cf7f17e3ad/1c21e5b/halo2_proofs/src/transcript/poseidon.rs:104:12
    |
104 |     state: Poseidon<C::ScalarExt, POSEIDON_T, POSEIDON_RATE>,
    |            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ the trait `ff::PrimeField` is not implemented for `<C as CurveAffine>::ScalarExt`
    |
note: required by a bound in `Poseidon`
   --> /Users/sohamzemse/.cargo/git/checkouts/poseidon-9a32cea385ed9159/0d2fb81/src/poseidon.rs:9:24
    |
9   | pub struct Poseidon<F: PrimeField, const T: usize, const RATE: usize> {
    |                        ^^^^^^^^^^ required by this bound in `Poseidon`
help: consider introducing a `where` clause, but there might be an alternative better way to express this requirement
    |
103 | pub struct PoseidonWrite<W: Write, C: CurveAffine, E: EncodedChallenge<C>> where <C as CurveAffine>::ScalarExt: ff::PrimeField {
    |                                                                            +++++++++++++++++++++++++++++++++++++++++++++++++++

For more information about this error, try `rustc --explain E0277`.
error: could not compile `halo2_proofs` due to 2 previous errors
warning: build failed, waiting for other jobs to finish...
```

</details>

### How this reproduction was made?

1. Basic `cargo init` project created
2. Rust toolchain added
3. Added dependency in Cargo.toml file along with a patch which is defined in the scroll's root Cargo.toml file

```
[dependencies]
zkevm-circuits = { git = "https://github.com/scroll-tech/zkevm-circuits", branch = "goerli-0215", features = [ "test" ] }

[patch."https://github.com/privacy-scaling-explorations/halo2.git"]
halo2_proofs = { git = "https://github.com/scroll-tech/halo2.git", branch = "scroll-dev-0220" }

```