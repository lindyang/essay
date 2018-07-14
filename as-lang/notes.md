.code32 才可以使用 pushl
cpuid2.s
- as --32 -o cpuid2.o cpuid2.s
- ld -m elf_i386 --dynamic-linker /lib/ld-linux.so.2 -lc -o cpuid2 cpuid2.o
- gcc -m32 -o cpuid2 cpuid2.s
