Download googletest

    $ curl -O https://googletest.googlecode.com/files/gtest-1.7.0.zip
    $ unzip gtest-1.7.0.zip

Download and build google benchmark

    $ git clone https://github.com/google/benchmark
    $ cd benchmark
    $ cmake . -DCMAKE_BUILD_TYPE=Release
    $ make
    $ sudo make install

Build s2-geometry-library and run tests

    $ cd geometry
    $ GTEST_DIR=/path/to/gtest-1.7.0 make
    $ GTEST_DIR=/path/to/gtest-1.7.0 make test
    $ GTEST_DIR=/path/to/gtest-1.7.0 make bench