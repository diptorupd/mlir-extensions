# Intel Extension for MLIR

The Intel MLIR Extensions (IMEX) project is a staging ground for MLIR
dialects and MLIR-based compiler tools.

# Dialects

## gpu_runtime : Dialect to launch compute kernels on Level-Zero GPUs
## PLIER: Typed Numba IR as an MLIR dialect.
# Tools

## numba_dpcomp: Prototype Numba compiler using the PLIER dialect.

# Getting started

## Build MLIR from upstream

IMEX is tied to a specific SHA of LLVM main. The SHA is stored in the
`llvm_sha.txt` file.

The easiest way to build LLVM+MLIR is to use the helper script provided in
the `scripts` directory.

```bash
python scripts/build_locally.py                                   \
  --working-dir <some place where LLVM will be built>             \
  --imex-tbb-dir <where tiy downloaded the latest oneTBB package>
```

The above script will build LLVM+MLIR and IMEX for you.

## Create a conda environment with needed packages

You can now build numba_dpcomp. But, before you do so install some needed Python
packages:

```bash
conda create -n dpcompenv python=3.7 numba=0.53 scipy pybind11 tbb tbb-devel cmake pytest scikit-learn
```

## Build numba_dpcomp
```bash
cd numba_dpcomp
export LLVM_PATH=<location of _mlir_instal dir inside the working dir specified to build_locally.py>
export TBB_PATH=<PATH to your oneTBB install>
python setup.py develop
```

## Run python tests

Use Pytest from root of repository to run tests.

```bash
source <path_to_miniconda>/bin/activate
source $(pwd)/../tbb/env/vars.sh
pytest
```

To run tests in parallel and to prevent segfaults from terminating your test
runner use you can use `pytest-xdist`
```bash
conda install pytest-xdist
pytest -n4
```

## Run lit/FileCheck tests

```bash
ctest -C Release
```
