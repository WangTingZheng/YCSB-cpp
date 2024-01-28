# YCSB-cpp

## Build

build ycsb-cpp with cmake

```shell
git clone https://github.com/ls4154/YCSB-cpp.git
cd YCSB-cpp
git submodule update --init
```

To bind LevelDB:

```shell
make BIND_LEVELDB=1 EXTRA_CXXFLAGS=-I/example/leveldb/include \ EXTRA_LDFLAGS="-I/example/leveldb/include -L/example/leveldb/build -lleveldb"
```

## Use Snappy

download and build snappy

```
git clone https://github.com/google/snappy.git
cd snappy
git submodule update --init
git checkout 1.1.9
mkdir build && cd build
cmake -DCMAKE_BUILD_TYPE=Release ../
make
```

add snappy path to cmakefile
```
link_directories("/home/someone/project/snappy/build")
include_directories("/home/someone/project/snappy" "/home/someone/project/snappy/build")
check_library_exists(snappy snappy_compress "" HAVE_SNAPPY)
set(HAVE_SNAPPY ON)
```

make
```shell
make BIND_LEVELDB=1 EXTRA_CXXFLAGS=-I/example/leveldb/include \ EXTRA_LDFLAGS="-I/example/leveldb/include -L/example/snappy/build -L/example/leveldb/build -lsnappy -lleveldb"
```

## Rate limit

set rate in ``limit.file``, just like this:

```shell
10 1000
20 1000
30 1000
40 1000
50 1000
60 1000
70 1000
80 1000
90 1000
100 1000
```

20 stand for run 20 second, 288 stands for run 288 get in one second

use ``limit.ops`` to set initial rate, all command just like this:

```shell
./ycsb -run -db basic -P workloads/workloada  -p threadcount=4 -p basic.silent=true -p limit.ops=100 -p limit.file=rate_trace.txt -s
```

run result just like this:

```shell
2024-01-28 19:34:32 0 sec: 0 operations;
2024-01-28 19:34:42 10 sec: 1000 operations; [READ: Count=528 Max=26.86 Min=0.46 Avg=5.41 90=9.92 99=25.81 99.9=26.82 99.99=26.86] [UPDATE: Count=472 Max=30.32 Min=0.49 Avg=6.16 90=12.48 99=27.54 99.9=30.32 99.99=30.32]
2024-01-28 19:34:52 20 sec: 11049 operations; [READ: Count=5592 Max=55.42 Min=0.04 Avg=6.03 90=12.68 99=23.01 99.9=27.74 99.99=41.63] [UPDATE: Count=5457 Max=30.32 Min=0.08 Avg=6.39 90=12.80 99=23.33 99.9=28.18 99.99=29.95]
2024-01-28 19:35:02 30 sec: 21149 operations; [READ: Count=10609 Max=55.42 Min=0.04 Avg=5.73 90=12.54 99=22.62 99.9=27.74 99.99=41.63] [UPDATE: Count=10540 Max=30.32 Min=0.08 Avg=6.18 90=12.71 99=23.21 99.9=27.54 99.99=30.27]
2024-01-28 19:35:12 40 sec: 31249 operations; [READ: Count=15593 Max=184.32 Min=0.04 Avg=6.40 90=16.70 99=30.73 99.9=51.39 99.99=66.43] [UPDATE: Count=15656 Max=174.59 Min=0.08 Avg=7.03 90=17.20 99=31.63 99.9=61.09 99.99=132.61]
2024-01-28 19:35:22 50 sec: 41349 operations; [READ: Count=20618 Max=184.32 Min=0.04 Avg=5.57 90=12.58 99=30.37 99.9=49.05 99.99=66.43] [UPDATE: Count=20731 Max=174.59 Min=0.08 Avg=6.17 90=12.80 99=31.09 99.9=55.55 99.99=132.61]
2024-01-28 19:35:32 60 sec: 51449 operations; [READ: Count=25622 Max=184.32 Min=0.04 Avg=5.19 90=12.35 99=30.00 99.9=46.05 99.99=63.87] [UPDATE: Count=25827 Max=174.59 Min=0.08 Avg=5.82 90=12.45 99=30.59 99.9=55.39 99.99=124.67]
2024-01-28 19:35:42 70 sec: 61574 operations; [READ: Count=30607 Max=184.32 Min=0.04 Avg=5.18 90=12.35 99=29.31 99.9=43.68 99.99=63.87] [UPDATE: Count=30967 Max=174.59 Min=0.08 Avg=5.75 90=12.40 99=30.06 99.9=54.81 99.99=124.67]
2024-01-28 19:35:52 80 sec: 71674 operations; [READ: Count=35604 Max=184.32 Min=0.04 Avg=5.21 90=12.37 99=26.98 99.9=41.89 99.99=62.69] [UPDATE: Count=36070 Max=174.59 Min=0.08 Avg=5.78 90=12.42 99=27.87 99.9=54.62 99.99=124.09]
2024-01-28 19:36:02 90 sec: 81774 operations; [READ: Count=40717 Max=184.32 Min=0.04 Avg=5.24 90=12.38 99=25.92 99.9=40.83 99.99=62.69] [UPDATE: Count=41057 Max=174.59 Min=0.08 Avg=5.81 90=12.44 99=26.93 99.9=53.28 99.99=124.09]
2024-01-28 19:36:12 100 sec: 91924 operations; [READ: Count=45772 Max=184.32 Min=0.04 Avg=5.15 90=12.38 99=29.17 99.9=40.29 99.99=62.69] [UPDATE: Count=46152 Max=174.59 Min=0.08 Avg=5.67 90=12.39 99=29.60 99.9=51.45 99.99=123.26]
2024-01-28 19:36:20 108 sec: 100000 operations; [READ: Count=49762 Max=184.32 Min=0.04 Avg=4.96 90=12.28 99=28.25 99.9=39.74 99.99=62.69] [UPDATE: Count=50238 Max=174.59 Min=0.08 Avg=5.47 90=12.25 99=28.00 99.9=48.54 99.99=123.26]
Run runtime(sec): 108.089
Run operations(ops): 100000
Run throughput(ops/sec): 925.164
```