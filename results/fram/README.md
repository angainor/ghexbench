# Results on Fram (Norway)

|Node   |Memory |#C/T    |CPU VERSION             | #ACCEL  |ACCEL TYPE|
|:------|------:|-------:|:-----------------------|:--------|:---------|
|fram   |128 GB |32/64   |E5-2683 v4 @ 2.10GHz    |NO       |          |

## hwloc info

```
depth 0:           1 Machine (type #0)
 depth 1:          2 Package (type #1)
  depth 2:         2 L3Cache (type #6)
   depth 3:        32 L2Cache (type #5)
    depth 4:       32 L1dCache (type #4)
     depth 5:      32 L1iCache (type #9)
      depth 6:     32 Core (type #2)
       depth 7:    64 PU (type #3)
Special depth -3:  2 NUMANode (type #13)
Special depth -4:  9 Bridge (type #14)
Special depth -5:  8 PCIDev (type #15)
Special depth -6:  10 OSDev (type #16)
```

## Compiler info
Benchmarks were compiled using the following compilers and MPI implementations

* OpenMPI (4.0.5) + GCC (10.2.0)  
  `CXXFLAGS="-Wall -Wextra -Wpedantic -Wno-unknown-pragmas -march=broadwell -funroll-all-loops"`
* Intel MPI + Intel C++ compiler (TODO)  
  `CXXFLAGS="-march=core-avx2 -fma -ftz -fomit-frame-pointer"`

Below, those configurations are refered to as `ompi` and `impi`, respectively. If for some results there is no explicit tag,
`ompi` was the configuration used.

## JUBE setup
The JUBE XML script is configured through the following tags

* `ompi`, `impi`: toolchain used
* `single_node`, `27_nodes` : number of nodes used
* `mpicart`, `hwcart`: Cartesian communicator implementation

The benchmark started 32 ranks per compute node, 1 rank per each physical core.

Within each compute node the ranks were organized into a 3D grid. The actual decomposition is mentioned
for each benchmark. Additionally, for `27_nodes` configurations the nodes were arranged into a 3D grid
with dimension `[3 3 3]`.

When using the `mpicart` configuration the rank neighborhood was determined by the `MPI_Cart_*` communicator.
In that case the rank to compute node assignment was done automatically by MPI. Running the benchmark
only required specifying the global rank grid dimensions, e.g., ``--MPICart 12 12 6``. 
With the `hwcart` configuration the ranks were remapped to a memory domain specific rank grid.
For example, `--socket 1 1 2 --core 4 4 1`.

## Benchmark results
The computational grid was periodic in all dimensions. Single precision, 32-bit floats were used.

### Setup 1

Results for a 64^3 domain with different halos (1,2,4,5) and different number of fields
(1 2 4 8).

In-node rank grid was `[4 4 2]`, with `hwcart` decomposition  
`--socket 1 1 2 --core 4 4 1`.

For 27-node runs the node grid dimensions were `[3 3 3]`.

#### Single-node tests

[`jube run bench.xml --tag single_node ompi hwcart`](single_node/jube_0.md)  
[`jube run bench.xml --tag single_node impi hwcart`](single_node/jube_1.md)  

The following tables present a human-readable comparison of the different benchmark results.

[`hwcart halfcores`](single_node/halo_fields_halfcores.md)  
[`hwcart allcores`](single_node/halo_fields_allcores.md)  

#### 27-node tests

[`jube run bench.xml --tag 27_nodes ompi mpicart`](27_nodes/jube_0.md)  
[`jube run bench.xml --tag 27_nodes impi mpicart`](27_nodes/jube_1.md)  
[`jube run bench.xml --tag 27_nodes ompi hwcart`](27_nodes/jube_2.md)  
[`jube run bench.xml --tag 27_nodes impi hwcart`](27_nodes/jube_3.md)  

The following tables present a human-readable comparison of the different benchmark results.

[`hwcart`](27_nodes/halo_fields_halfcores.md)  
[`mpicart` vs `hwcart`](27_nodes/mpicart_vs_hwcart.md)  
