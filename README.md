This repository is a WIP for devs to reverse engineer the new format SUPERPACK_OB used by Meta to store all the libs in a libs.spo archive.  
I'm trying to create a python script to unpack and build again like ![this one](https://github.com/itsMoji/Instagram_SSL_Pinning/blob/f68e836445d6da24ee16f8f3fb8f3a416f9dd3d5/tools/libs_builder.py).  
In the function read_native of libsuperpack-jni there is the whole process, I uploaded the decompiled C code with IDA pro.    

Using IDA Pro of Ghidra, I noticed that there is a check for the new format "spo", they read the first 8 bytes of "libs.spo" and check for the magic bytes 0x5ABAF0150C00100.  
The first time that has been implemented a SUPERPACK_OB arhive was with Istagram v190.0.0.0.26-arm64