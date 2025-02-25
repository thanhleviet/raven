# Raven

[![Latest GitHub release](https://img.shields.io/github/release/lbcb-sci/raven.svg)](https://github.com/lbcb-sci/raven/releases/latest)
[![Build status for c++/clang++](https://travis-ci.org/lbcb-sci/raven.svg?branch=master)](https://travis-ci.org/lbcb-sci/raven)

Tool for de novo genome assembly of long uncorrected reads.

## Description
Raven is an assembler for raw reads generated by third generation sequencing. It first finds overlaps between reads by chaining minimizer hits (submodule [Ram](https://github.com/lbcb-sci/ram) which is [minimap](https://github.com/lh3/minimap1) turned into a library), creates an assembly graph and simplifies it (code from [Rala](https://github.com/rvaser/rala)), and polishes the obtained contigs with partial order alignment (submodule [Racon](https://github.com/lbcb-sci/racon)).

Raven takes as input a single file containing raw reads in FASTA/FASTQ format (can be compressed with gzip) and outputs a set of contigs with high accuracy in FASTA format to stdout.

## Dependencies
1. gcc 4.8+ or clang 3.4+
2. cmake 3.2+

### CUDA Support
1. gcc 5.0+
2. cmake 3.10+
4. CUDA 9.0+

## Installation
To install Raven run the following commands:

```bash
git clone --recursive https://github.com/lbcb-sci/raven.git raven
cd raven
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release ..
make
```

After successful installation, an executable named `raven` will appear in `build/bin`.

Optionally, you can run `sudo make install` to install raven executable to your machine.

***Note***: if you omitted `--recursive` from `git clone`, run `git submodule update --init --recursive` before proceeding with compilation.

### CUDA Support
Submodule `racon` makes use of [NVIDIA's ClaraGenomicsAnalysis SDK](https://github.com/clara-genomics/ClaraGenomicsAnalysis) for CUDA accelerated polishing and alignment.

To build with CUDA support, add `-Dracon_enable_cuda=ON` while running `cmake`. If CUDA support is unavailable, the `cmake` step will report an error.
Note that the CUDA support flag does not produce a new binary target. Instead it augments the existing `raven` binary itself.

```bash
cd build
cmake -DCMAKE_BUILD_TYPE=Release -Dracon_enable_cuda=ON ..
make
```

### Other options
Install [Linuxbrew](https://docs.brew.sh/Homebrew-on-Linux) and run the following command:

```bash
brew install brewsci/bio/raven-assember
```

## Usage
Usage of `raven` is as following:

    raven [options ...] <sequences>

        <sequences>
            input file in FASTA/FASTQ format (can be compressed with gzip)
            containing sequences

        options:
            -p, --polishing-rounds <int>
                default: 2
                number of times racon is invoked
            -t, --threads <int>
                default: 1
                number of threads
            --version\
                prints the version number
            -h, --help
                prints the usage
        (only available when built with CUDA)
            -c, --cudapoa-batches
                default: 1
                number of batches for CUDA accelerated polishing
            -b, --cuda-banded-alignment
                use banding approximation for polishing on GPU
                (only applicable when -c is used)
            -a, --cudaaligner-batches
                number of batches for CUDA accelerated alignment

## Contact information

For additional information, help and bug reports please send an email to one of the following: robert.vaser@fer.hr, mile.sikic@fer.hr.

## Acknowledgment

This work has been supported in part by the Croatian Science Foundation under the projects Algorithms for genome sequence analysis (UIP-11-2013-7353) and Single genome and metagenome assembly (IP-2018-01-5886), and in part by the European Regional Development Fund under the grant KK.01.1.1.01.0009 (DATACROSS).
