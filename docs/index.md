# Welcome to micro**BioRust**
 
A blazing-fast, sustainable bioinformatics toolkit written in [Rust](https://www.rust-lang.org/) — for microbial genomics rresearch, and optimised for functions used in data exploration.  

---

## Features

- 🦀 Built in Rust programming language for speed and safety
- 🔄 Python bindings _via_ pyo3 for InterOp - Rust meets Python
- 📦 Open source and community-driven

---

## Get Started!!
See Installation for details on how to install Rust for Linux, MacOSX and Windows
Interested in microbiorust-py?  Check out the microbiorust-py section for quick-start & more!  

Install microbiorust-py from pip

```
pip install microbiorust
```

For Rust:

Using microBioRust in a Rust Project:  

To start a new project:

```
cargo new microbiorust_test  

cd microbiorust_test
```

To add the microBioRust format parsing crate to your Cargo.toml
```cargo add microBioRust```

to add other members of the workspace including sequence metrics, python bindings and data viz (heatmap demonstration)

```cargo add seqmetrics```  
```cargo add heatmap```    
```cargo add microbiorust-py```

or clone the repo  
```git clone https://github.com/LCrossman/microBioRust.git```

Build the project  
```cargo build```

Run the tests  
```cargo test```

