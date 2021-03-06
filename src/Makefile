KERNEL_OBJECTS=kernelcore.o main.o console.o memory.o keyboard.o mouse.o clock.o interrupt.o kmalloc.o pic.o ata.o cdromfs.o string.o bitmap.o graphics.o font.o syscall_handler.o process.o mutex.o list.o pagetable.o rtc.o kshell.o fs.o kevinfs/kevinfs.o hashtable.o kevinfs/kevinfs_test.o kevinfs/kevinfs_ata.o serial.o elf.o

USER_OBJECTS=user-start.o test.o syscall.o syscalls.o string.o user-io.o malloc.o

KERNEL_CCFLAGS=-Wall -c -ffreestanding -fno-pie
CC=i686-elf-gcc
LD=i686-elf-ld
ISOGEN=genisoimage
OBJCOPY=i686-elf-objcopy

all: basekernel.iso

basekernel.iso: basekernel.img test.exe saver.exe ball.exe long.exe write.exe copy.exe
	${ISOGEN} -J -R -o $@ -b $^ $<

basekernel.img: bootblock kernel
	cat bootblock kernel /dev/zero | head -c 1474560 > basekernel.img

bootblock.elf: bootblock.o
	${LD} ${KERNEL_LDFLAGS} -Ttext 0 $< -o $@

kernel: kernel.elf
	${OBJCOPY} -O binary $< $@

bootblock: bootblock.elf
	${OBJCOPY} -O binary $< $@

%.exe: %.elf
	${OBJCOPY} -O binary $< $@

kernel.elf: ${KERNEL_OBJECTS}
	${LD} ${KERNEL_LDFLAGS} -Ttext 0x10000 ${KERNEL_OBJECTS} -o $@

%.o: %.c
	${CC} ${KERNEL_CCFLAGS} $< -o $@

%.o: %.S
	${CC} ${KERNEL_CCFLAGS} $< -o $@

test.exe: test.o $(USER_OBJECTS)
	${LD} ${KERNEL_LDFLAGS} -Ttext 0x80000000 user-start.o $< syscall.o syscalls.o string.o user-io.o malloc.o -o $@

long.exe: long.o $(USER_OBJECTS)
	${LD} ${KERNEL_LDFLAGS} -Ttext 0x80000000 user-start.o $< syscall.o syscalls.o string.o user-io.o -o $@

saver.elf: saver.o $(USER_OBJECTS)
	${LD} ${KERNEL_LDFLAGS} -Ttext 0x80000000 user-start.o $< syscall.o syscalls.o string.o user-io.o -o $@

saver.exe: saver.o $(USER_OBJECTS)
	${LD} ${KERNEL_LDFLAGS} -Ttext 0x80000000 user-start.o $< syscall.o syscalls.o string.o user-io.o -o $@

ball.exe: ball.o $(USER_OBJECTS)
	${LD} ${KERNEL_LDFLAGS} -Ttext 0x80000000 user-start.o $< syscall.o syscalls.o user-io.o string.o -o $@

copy.elf: copy.o $(USER_OBJECTS)
	${LD} ${KERNEL_LDFLAGS} -Ttext 0x80000000 user-start.o $< syscall.o syscalls.o string.o user-io.o -o $@

clean:
	rm -rf basekernel.img bootblock kernel *.o kevinfs/*.o test.exe saver.exe ball.exe long.exe write.exe copy.exe
