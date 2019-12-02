# Onions(Misc) - Writeup

## Question

````
Ogres are like files -- they have layers!
````

## Answer
When the specified file is confirmed, the 7z format file is added.

When decompressing as follows, a test file with flag is output.
1. Extract 7z file
1. Unzip 7z file → Output flag.tar.gz file
1. Unzip flag.tar.gz file → Output flag.cpio file
1. Decompress flag.cpio file → output flag.lzma file
1. Decompress flag.lzma file → Output bzip2 file named flag1.txt file
1. Decompress flag1.txt file → Output XZ file
1. Decompress XZ file → output flag text

###### TUCTF{F1L3S4R3L1K30N10NSTH3YH4V3L4Y3RS}
