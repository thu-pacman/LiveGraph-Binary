# Reproduce LinkBench Results of LiveGraph

As [LinkBench](https://github.com/facebookarchive/linkbench) is implemented in Java and LiveGraph is in C++, we record traces collected from LinkBench Java driver and replay them in a driver of C++ version, so that LiveGraph can perform those queries.

These repositories are used to run LinkBench on LiveGraph:

* A modified LinkBench Java driver with a wrapper implementation which processes the queries correctly (by calling MySQL implementation inside) and also records the operations of each thread into files.
* A C++ driver which reads the queries from files, calls LiveGraph (or other implementations LMDB and RocksDB), and gives performance statistics.

Detailed steps of reproducing are listed as follows. For questions, you may want to open an issue or contact Jiping Yu (yjp19@mails.tsinghua.edu.cn). Also, we are currently working on implementing LinkBench directly in C++, to make reproducing the results easier.

## Recording

* Clone the [original LinkBench Java driver](https://github.com/facebookarchive/linkbench) and ensure the dependencies.
* Instead of building the project, place `FacebookLinkBench.jar` (downloaded from the [Release page](https://github.com/thu-pacman/LiveGraph-Binary/releases)) into the `target` directory.'
* Modify your config file (like `something.properties`), change `linkstore = com.facebook.LinkBench.GraphStoreWrapper` and `nodestore = com.facebook.LinkBench.GraphStoreWrapper`.
* Before running the driver, make sure there exists an empty directory named `record` in the current working directory.
* Run the driver in the same way as the original driver (by executing `./bin/linkbench [args]`) with the MySQL implementation. The single run must include the loading and requesting stages (specifying `-l -r` in the command line).
* After the run, the recorded traces are saved to `record` directory of the current working directory.

## Replaying

Download the C++ driver `linkbench_livegraph`, and run

```
./linkbench_livegraph [database path] [max #vertices] [trace directory path] [#loaders] [#requesters]
```

`[#loaders]` and `[#requesters]` should equal the numbers during recording. You may need to properly specify `LD_LIBRARY_PATH` to find LiveGraph library files.
