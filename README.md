This repository is a WIP for devs to reverse engineer the new format SUPERPACK_OB used by Meta to store all the libs in a libs.spo archive.  
I'm trying to create a python script to unpack and build again like ![this one](https://github.com/itsMoji/Instagram_SSL_Pinning/blob/f68e836445d6da24ee16f8f3fb8f3a416f9dd3d5/tools/libs_builder.py).  
In the function read_native of libsuperpack-jni there is the whole process, I uploaded the decompiled C code with IDA pro.    

Using IDA Pro of Ghidra, I noticed that there is a check for the new format "spo", they read the first 8 bytes of "libs.spo" and check for the magic bytes 0x5ABAF0150C00100.  
The first time that has been implemented a SUPERPACK_OB arhive was with Istagram v190.0.0.0.26-arm64



**UPDATE 22/06/2022: **  
The "libs.spo" file is just a container of many xz archives.
The first one is the file "libs.xz", you can see the magic bytes "FD 37 7A 58 5A 00" and
before the magic bytes of each XZ archives, there are 22 bytes like this:
?? ?? 00 00 00 00 00 00 00 00 00 00 4bytes size 4bytes compressed_size ?? ??

Therefore you can calculate the end of the archive copying the 4bytes of compressed_size and adding to the offset of "FD 37...", or you can search for the footer "59 5A" before a new "FD 37 7A 58..."
You will obtain the "libs.xz" archive as first entry, and you can extract the file "libs" that contains all the compressed libs, but they are obfuscated, in some case like arm-v7a you can be lucky finding the bytes that you want to patch (like @itsMoji did in Instagram armv7a version SSL unpinning, patching libliger.so), therefore you can recompress it, update the new compressed size, and replacing inside libs.spo.
But the obfuscation for armv64 looks heavier or different and you won't be able to find the bytes to patch, **we need to find a way to deobfuscate them**.
