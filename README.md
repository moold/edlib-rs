# edlib_rs

This crate provides a Rust interface to the Edlib C++ library by Martin Šošić. See [Martinsos-edlib](https://github.com/Martinsos/edlib)

The reference paper is :

Martin Šošić, Mile Šikić; Edlib: a C/C ++ library for fast, exact sequence alignment using edit distance. Bioinformatics 2017 [btw753. doi] <https://doi.org/10.1093/bioinformatics/btw753>

The crate offers 2 interfaces to edlib.  
The first, accessed via module bindings, is direcly the interface generated by the bindgen crate.  
The second, accessed via module edlibrs, provides a more idiomatic Rust interface. It comes at the cost of cloning information stored in pointers startLocations and endLocations in C **struct EdlibAlignResult** to get a Rust **struct EdlibAlignResultRs** with **Option<Vec\<u8\>>** fields instead of pointers. The cigar string representation is also cloned when computed.  
As a consequence memory management is fully transferred to Rust.  
Structures and functions have the same name as in edlib with just "Rs" appended to original names.

## Installation

The crate relies on the C++ edlib library being installed and compiled as described in edlib documentation.  
Before running cargo build (or cargo install) the environment variable EDLIB_DIR must be set to where the original C++ edlib directory was cloned. This is necessary for the build.rs step of Cargo to access the edlib library includes.
Also libstdc++ must be in your path.  
The crate enables a logger to monitor the call to the C-interface which is by default set in Cargo.toml to *info* for release mode and *trace* for debug mode, but can changed by setting the variable RUST_LOG (see env_logger doc).

## Tests and examples

Some tests in module edlib.rs can serve as basic examples.
In directory examples there is also a small version of the edlib edaligner module (see apps/aligner in edlib installation dir) which runs on
Fasta file containing only one sequence as contained in the **edlib** directory *test_data*. Contrary to the edlib version the module given a query and a target sequence runs the 3 modes (normal/NW, prefix/SHW and infix/HW)

We get the following timing in release mode for Enterobacteria_phage_1.fasta as target sequence  and  mutated_90_perc.fasta as query sequence.

| mode    | edlibrs time(s) | edlib time(s) |  distance |
|  :---:  |     :---:       |  :------:     |  :----:   |
|  NW     |     0.106       |  0.106        |  9506     |
|  SHW    |     0.184       |  0.191        |  9502     |
|  HW     |     0.682       |  0.695        |  9502     |

We get the following timing in release mode for Enterobacteria_phage_1.fasta as target sequence  and  mutated_60_perc.fasta as query sequence.

| mode    | edlibrs time(s) | edlib time(s) |  distance |
|  :---:  |     :---:       |  :------:     |  :----:   |
|  NW     |     0.398       |  0.398        |  39829    |
|  SHW    |     0.670       |  0.684        |  39828    |
|  HW     |     1.182       |  1.206        |  39828    |

Except for infinitesimal variations of cpu time measurement we wee we have the same computation times.

## License

Licensed under either of

* Apache License, Version 2.0, [LICENSE-APACHE](LICENSE-APACHE) or <http://www.apache.org/licenses/LICENSE-2.0>
* MIT license [LICENSE-MIT](LICENSE-MIT) or <http://opensource.org/licenses/MIT>

at your option.

This software was written on my own while working at [CEA](http://www.cea.fr/), [CEA-LIST](http://www-list.cea.fr/en/)
