# ZkInterface - Gadget Standard

<!-- a short paragraph that includes:

* One liner description of zk-interface
* Explain the ZkProof relation
* Link to wasm repo & the live demo -->

zkInterface is a standard tool for zero-knowledge interoperability between diffrent prove system.
The idea behind the zkInterface project was born by zkproof cumminity (attach link) to improve zkproof utilzation and usiblity.
We got live demo(live demo) that demonstrate the integration(Link to wasm) and basic use of zkInterface.

## Introduction

<!-- Eran's intro and we can add the overview diagram from the paper. (fig. 1) -->
![alt text](https://qedit.s3.eu-central-1.amazonaws.com/pictures/zkinterface.png)

*zkInterface* is specification and associated tools for enabling interoperability between implementations of general-purpose zero-knowledge proof systems. It aims to facilitate interoperability between zero knowledge proof implementations, at the level of the low-constraint systems that represent the statements to be proven. Such constraint systems are generated by _frontends_ (e.g., by compilation from higher-level specifications), and are consumed by cryptographic _backends_ which generate and verify the proofs. The goal is to enable decoupling of frontends from backends, allowing application writers to choose the frontend most convenient for their functional and development needs and combine it with the backend that best matches their performance and security needs.

The standard specifies the protocol for communicating constraint systems, for communicating variable assignments (for production of proofs), and for constructing constraint systems out of smaller building blocks (_gadgets_). These are specified using language-agnostic calling conventions and formats, to enable interoperability between different authors, frameworks and languages.

A simple special case is monolithic representation of a whole constraint system and its variable assignments. However, there are a need for more richer and more nuanced forms of interoperability:

* Precisely-specified statement semantics, variable representation and variable mapping
* Witness reduction, from high-level witnesses to variable assignments
* Gadgets interoperability, allowing components of constraint systems to be packaged in reusable and interoperable form
* Procedural interoperability, allowing execution of complex code to facilitate the above

## Current Status

<!-- What we have done, what we supports, and add the table that we have under Implementations -->

### Implementations

|                                                           | Proving System | Export Circuits | Import Circuits |
| --------------------------------------------------------- | -------------- | --------------- | --------------- |
| [Bellman](https://github.com/QED-it/zkinterface-bellman) (Groth16) | Yes            | No              | Yes             |
| [Dalek](https://github.com/QED-it/bulletproofs/blob/zkinterface/src/r1cs/zkinterface_backend.rs) (Bulletproofs) | Yes | No | No |
| [ZoKrates](https://github.com/QED-it/ZoKrates/blob/zkinterface/zokrates_core/src/proof_system/zkinterface.rs) | - | Yes | No |
| [Libsnark](https://github.com/QED-it/zkinterface/tree/master/cpp) | PGHR | Yes | No |

See also the [WebAssembly modules](https://github.com/QED-it/zkinterface-wasm/) and the [live demo](https://qed-it.github.io/zkinterface-wasm-demo/).

## How to use it

*TODO: Discuss how different stakeholders will use this (frontend authors, backend authors, gadget authors, integrators) and what they should do.*

### Repository content

|                           |                             |
| ------------------------- | --------------------------- |
| `zkInterface.pdf`         | The interface specification |
| `zkinterface.fbs`         | The gadget interface definition using FlatBuffers |
| `rust/src/zkinterface_generated.rs` | Generated Rust code         |
| `rust/src/gadget_call.rs`           | Example gadget call in Rust |
| `cpp/zkinterface_generated.h`       | Generated C++ code          |
| `cpp/gadget_example.cpp`            | Example gadget in C++       |
| `build.rs`                | Generate Rust and C++ code from zkinterface.fbs, and compile the C++ example |
| `cpp/libsnark_integration.hpp` | Libsnark support            |
| `cpp/libsnark_example.cpp`     | Libsnark gadget example     |

### Testing

*TODO: Clarify what this tests*

In the `rust` directory:

`cargo test`

This will generate and compile Rust and C++ code, and run a test of both sides communicating
through the standard interface.

### Generated code

Generated C++ and Rust code is included.

For other languages, install the FlatBuffers code generator (`flatc`).
One way is to compile it with the following:

```
git clone https://github.com/google/flatbuffers.git
cd flatbuffers
cmake -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Release
make
```

Then run:
`flatc --LANGUAGE zkinterface.fbs`

## How to contribute

<!-- define broad goals and some more specific goals -->

- In a frontend, implement a feature to export the circuits or gadgets to ZkInterface format.
- In a proving system, support loading circuits from ZkInterface buffers or files.

See the implementation guide section in the spec above for more details, and check out the existing implementations below.


## Specification

The [zkInterface specification document](zkInterface.pdf) providers further context on the above, and defines the communication protocol and calling convention between frontends and backends:

* The logical content of messages being exchange.
* The serialization format of the messages (which is based on [FlatBuffers](FlatBuffers) and may be used in-memory, saved or streamed).
* A protocol for building a constraint system from gadget composition.
* Technical recommendations for implementation.

## Limitations

This first revision focuses on non-interactive proof systems (NIZKs) for general statements (i.e., NP relations) represented in the R1CS/QAP-style constraint system representation. Extension to other forms of constraint systems is planned.

The associated code is experimental.

See the [specification document](zkInterface.pdf) for more information about limitations and scope.


<!-- 
zkInterface is a standard tool for zero-knowledge interoperability.

[The spec can be found here](zkInterface.pdf).

## How to contribute 
- In a frontend, implement a feature to export the circuits or gadgets to ZkInterface format.
- In a proving system, support loading circuits from ZkInterface buffers or files.

See the implementation guide section in the spec above for more details, and check out the existing implementations below.

## Implementations

|                                                           | Proving System | Export Circuits | Import Circuits |
| --------------------------------------------------------- | -------------- | --------------- | --------------- |
| [Bellman](https://github.com/QED-it/zkinterface-bellman) (Groth16) | Yes            | No              | Yes             |
| [Dalek](https://github.com/QED-it/bulletproofs/blob/zkinterface/src/r1cs/zkinterface_backend.rs) (Bulletproofs) | Yes | No | No |
| [ZoKrates](https://github.com/QED-it/ZoKrates/blob/zkinterface/zokrates_core/src/proof_system/zkinterface.rs) | - | Yes | No |
| [Libsnark](https://github.com/QED-it/zkinterface/tree/master/cpp) | PGHR | Yes | No |

See also the [WebAssembly modules](https://github.com/QED-it/zkinterface-wasm/) and the [live demo](https://qed-it.github.io/zkinterface-wasm-demo/).

## Structure of this repo

|                           |                             |
| ------------------------- | --------------------------- |
| `zkInterface.pdf`         | The interface specification |
| `zkinterface.fbs`         | The gadget interface definition using FlatBuffers |
| `rust/src/zkinterface_generated.rs` | Generated Rust code         |
| `rust/src/reading.rs`               | Rust helpers to read messages |
| `rust/src/writing.rs`               | Rust helpers to write messages |
| `rust/src/cpp_gadget.rs`            | Rust helpers to interact with C++ |
| `rust/src/examples.rs`              | Example messages for a simple test circuit |
| `cpp/zkinterface_generated.h`       | Generated C++ code          |
| `cpp/gadget_example.cpp`            | Example gadget in C++       |
| `build.rs`                | Generate Rust and C++ code from zkinterface.fbs, and compile the C++ example |
| `cpp/libsnark_integration.hpp` | Libsnark support            |
| `cpp/libsnark_example.cpp`     | Libsnark gadget example     |

## Test

In the `rust` directory:

`cargo test`

This will generate and compile Rust and C++ code, and run a test of both sides communicating
through the standard interface.

```
cargo run --bin example > example.zkif
cargo run --bin print   < example.zkif
```

This will generate and print a file containing the messages Circuit, R1CSConstraints, and Witness for a toy circuit.

## Generated code

Generated C++ and Rust code is included.

For other languages, install the FlatBuffers code generator (`flatc`).
One way is to compile it with the following:

```
git clone https://github.com/google/flatbuffers.git
cd flatbuffers
cmake -G "Unix Makefiles" -DCMAKE_BUILD_TYPE=Release
make
```

Then run:
`flatc --LANGUAGE zkinterface.fbs` -->
