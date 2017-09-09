# Linux Kernel

Your kernel needs to be built with the following options:
```
CONFIG_BPF=y
CONFIG_BPF_SYSCALL=y
CONFIG_BPF_JIT=y
CONFIG_HAVE_BPF_JIT=y
CONFIG_BPF_EVENTS=y
```

To use some BPFtrace features, minimum kernel versions are required:
- 4.1+ - kprobes
- 4.3+ - uprobes
- 4.6+ - stack traces, count and quantize builtins (use PERCPU maps for accuracy and efficiency)
- 4.7+ - tracepoints
- 4.9+ - timers/profiling


# Building BPFtrace

## Native build process

### Requirements

- A C++ compiler
- CMake
- Flex
- Bison
- LLVM 3.9 development packages
- LibElf

### Compilation
```
git clone https://github.com/ajor/bpftrace
mkdir -p bpftrace/build
cd bpftrace/build
cmake -DCMAKE_BUILD_TYPE=Debug ../
make
```

By default bpftrace will be built as a static binary to ease deployments. If a dynamically linked executable would be preferred, the CMake option `-DDYNAMIC_LINKING:BOOL=ON` can be used.

## Using Docker

Building BPFtrace inside a Docker container is the recommended method:

`./build.sh`

There are some more fine-grained options if you find yourself building BPFtrace a lot:
- `./build-docker.sh` - builds just the `bpftrace-builder` Docker image
- `./build-debug.sh` - builds BPFtrace with debugging information
- `./build-release.sh` - builds BPFtrace in a release configuration

`./build.sh` is equivalent to `./build-docker.sh && ./build-release.sh`

These build scripts pass on any command line arguments to `make` internally. This means specific targets can be built individually, e.g.:
- `./build.sh bpftrace` - build only the targets required for the bpftrace executable
- `./build.sh bcc-update` - update the copy of BCC used to build BPFtrace
- `./build.sh gtest-update` - update the copy of Google Test used to build the BPFtrace tests

The latest versions of BCC and Google Test will be downloaded on the first build. To update them later, the targets `bcc-update` and `gtest-update` can be built as shown above.