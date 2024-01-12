# The gem5 Simulator

This is the repository for the gem5 simulator. It contains the full source code
for the simulator and all tests and regressions.

The gem5 simulator is a modular platform for computer-system architecture
research, encompassing system-level architecture as well as processor
microarchitecture. It is primarily used to evaluate new hardware designs,
system software changes, and compile-time and run-time system optimizations.

The main website can be found at <http://www.gem5.org>.

## Getting started

A good starting point is <http://www.gem5.org/about>, and for
more information about building the simulator and getting started
please see <http://www.gem5.org/documentation> and
<http://www.gem5.org/documentation/learning_gem5/introduction>.

## Building gem5

To build gem5, you will need the following software: g++ or clang,
Python (gem5 links in the Python interpreter), SCons, zlib, m4, and lastly
protobuf if you want trace capture and playback support. Please see
<http://www.gem5.org/documentation/general_docs/building> for more details
concerning the minimum versions of these tools.

Once you have all dependencies resolved, execute
`scons build/ALL/gem5.opt` to build an optimized version of the gem5 binary
(`gem5.opt`) containing all gem5 ISAs. If you only wish to compile gem5 to
include a single ISA, you can replace `ALL` with the name of the ISA. Valid
options include `ARM`, `NULL`, `MIPS`, `POWER`, `SPARC`, and `X86` The complete
list of options can be found in the build_opts directory.

See https://www.gem5.org/documentation/general_docs/building for more
information on building gem5.

## The Source Tree

The main source tree includes these subdirectories:

* build_opts: pre-made default configurations for gem5
* build_tools: tools used internally by gem5's build process.
* configs: example simulation configuration scripts
* ext: less-common external packages needed to build gem5
* include: include files for use in other programs
* site_scons: modular components of the build system
* src: source code of the gem5 simulator. The C++ source, Python wrappers, and Python standard library are found in this directory.
* system: source for some optional system software for simulated systems
* tests: regression tests
* util: useful utility programs and files

## gem5 Resources

To run full-system simulations, you may need compiled system firmware, kernel
binaries and one or more disk images, depending on gem5's configuration and
what type of workload you're trying to run. Many of these resources can be
obtained from <https://resources.gem5.org>.

More information on gem5 Resources can be found at
<https://www.gem5.org/documentation/general_docs/gem5_resources/>.

## Regarding CSCE_614 Term project

CSCE 614 Term project to implement address translation and replay-loads conscious cache replacement policy in L2/L3 Cache, implemented in Gem5 simulator

Recent advancements in cache replacement policies have significantly improved memory access efficiency in computing systems. However, existing solutions like RRIP, SRRIP, Hawkeye, and SHiP, while proficient in many scenarios, exhibit a critical limitation: they do not adequately differentiate between address translation loads and replay loads. This oversight leads to suboptimal performance, particularly in contexts involving the Second-Level Translation Lookaside Buffer (STLB) and Reorder Buffer (ROB) stalls, contributing to a notable performance overhead.

The research paper we have selected is "Address Translation Conscious Caching and Prefetching for High-Performance Cache Hierarchy", which introduces innovative signature-based policies, namely T-DRRIP and T-SHiP, alongside the ATP (Address Translation Prefetcher), to address this gap. These policies are specifically designed to distinguish between different types of loads, thereby optimizing the cache replacement process. By employing a combination of these methods, notable improvements in benchmark performances are observed, reducing ROB stalls due to STLB misses significantly.

This term project aims to implement these signature-based policies within the gem5 simulator, a versatile tool for computer-system architecture research. By integrating T-DRRIP and T-SHiP into gem5, the project seeks to evaluate and validate the performance enhancements claimed by the original research in a simulated environment. The successful implementation and analysis will not only reinforce the findings of the paper but also contribute to the broader understanding of effective cache management techniques in modern computing systems.

Directory structure

The configs directory in the gem5 repository contains a variety of configuration scripts and related files that are used to set up and simulate different system architectures and environments. common directory includes helper scripts and functions for creating simulated systems. It contains files like Caches.py, Options.py, CacheConfig.py. These files provide various options and functions for setting cache parameters, memory systems, and full-system simulations, as well as managing simulation execution and checkpoints.

Our example scripts are provided in learning_gem5/part1 directory.

Outputs

This folder contains reference run and signature based run output files for within each subdirectory named with the benchmarks.

src

This directory contains all the source code changes for our project implementation. arch/x86 contain tlb.cc file where we included flags for tracking type of load requests. base/types.hh file includes the scope for all the variables used within gem5. Flags are declared as extern variables within this file. cpu/BaseCPU.py files contain modifications to include memory bus definition used to connect newly created L3 cache. mem/cache/replacement_policies directory contains our replacement policies and SConscript to create simulation objects.

Below are the steps to run:

Step1 Building Gem5:

Checkout Gem5 repository from https://github.com/gem5/gem5. Gem5 requires python3.6, GCC>=7 and SCons>=3. The project was built in Olympus server (olympus.ece.tamu.edu) as linux.cse.tamu.edu server neither has these dependencies nor has developer tools package support. Below command will enable developer environment to set gcc and python

%scl enable devtoolset-9 bash

Below command will install scons4.5 in your ~/.local area which needs to be set in $PATH environment variable.

%python3 -m pip install scons --user

%export PATH=$PATH:<your_location>/.local/lib

%export PATH=$PATH:<your_location>/.local/bin

After setting up the environment for building gem5, download the given files here in your local gem5 repository and replace them in respective directories. Once the setup is completed we can build gem5 using below command:

%scons build/X86/gem5.opt -j<numCores>

Step2 Running configuration scripts

Once the build is updated successfully, use below command with any of the configuration script to observe outputs:

%build/X86/gem5.opt --outdir <specify output directory name> configs/learning_gem5/part1/<config_file>.py

Step3 Results analysis:

Results are available in output directory specified above. Stats.txt file contains information related to CPI and simulated instructions, overall Misses in L2 and L3 cache. Config.json contains information regarding cache size and replacement policies at each level along with additional details. Config.ini is similar to .json file but in different format.
