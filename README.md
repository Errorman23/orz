Orz
===
this is a general purpose data compressor written in rust.

orz is mainly base on an optimized ROLZ (reduced offset Lempel-Ziv) dictionary compressor. symbols and matches are then encoded by an order-0 static huffman encoder. for better compression, there is a simplified order-1 MTF model before huffman coding.

with the great ROLZ algorithm, orz is more powerful than traditional LZ77 compressors like old gzip, zstandard from Facebook, lzfse from Apple, and brotli from Google. in our benchmark with large text (enwik8, test data of Hutter Prize), we can see that orz is faster and compressing better than other LZ77 ones, while decompression is still fast enough.

orz is completely implemented in rust. thanks to the wonderful rust compiler, we implemented orz in less than 1,000 lines of code, and the running speed is still as fast as C/C++.

installation
============
you can install orz with cargo:

    cargo install --git https://github.com/richox/orz --tag v1.3.0

usage
=====

for compression:

    orz encode <source-file-input> <compressed-file-output>

for decompression:

    orz decode <compressed-file-input> <source-file-output>

for more details, see `orz --help`

benchmarks
==========
benchmark for large text ([enwik8](http://mattmahoney.net/dc/text)):

| name        | compressed size | encode time | decode time |
|-------------|-----------------|-------------|-------------|
| **orz -l3** | 28,067,562      | 4.65s       | 0.73s       |
| **orz -l2** | 28,175,983      | 4.09s       | 0.73s       |
| **orz -l1** | 28,516,109      | 3.34s       | 0.74s       |
| bzip2       | 29,008,758      | 6.83s       | 3.91s       |
| **orz -l0** | 29,267,897      | 2.54s       | 0.80s       |
| brotli -9   | 29,685,672      | 36.47s      | 0.28s       |
| zstd -15    | 29,882,879      | 20.93s      | 0.18s       |
| zstd -12    | 31,106,827      | 11.07s      | 0.17s       |
| xz -3       | 31,233,128      | 30.59s      | 1.53s       |
| zstd -9     | 31,834,628      | 4.63s       | 0.17s       |
| xz -2       | 31,989,048      | 14.22s      | 1.60s       |
| brotli -6   | 32,446,572      | 5.28s       | 0.26s       |
| xz -1       | 33,276,380      | 7.17s       | 1.71s       |
| lzfse       | 36,157,828      | 1.75s       | 0.19s       |
| gzip        | 36,548,933      | 4.14s       | 0.33s       |
| brotli -3   | 36,685,022      | 1.16s       | 0.31s       |

reference:
1. zstandard: https://github.com/facebook/zstd
2. brotli: https://github.com/google/brotli
3. lzfse: https://github.com/lzfse/lzfse
