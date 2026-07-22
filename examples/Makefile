CFLAGS_M0=-mcpu=cortex-m0 -Os -g -Trp2040.ld --oslib=semihost

CLANG_FLAGS_M0=--target=thumbv6m-none-eabi -mfloat-abi=soft -Os -g -Trp2040.ld -lcrt0-semihost -lsemihost

CFLAGS_M4=-mcpu=cortex-m4 -Os -g -Tmetro-m4.ld --oslib=semihost

CLANG_FLAGS_M4=--target=thumbv7em-none-eabi -mfloat-abi=hard -Os -g -Tmetro-m4-clang.ld -lcrt0-semihost -lsemihost

ALL_M0=\
	hello-world-rp2040.elf \
	hello-world-rp2040-zephyr.elf \
	hello-world-rp2040-arm-etk.elf \
	hello-world-rp2040-ctng.elf \
	hello-world-rp2040-clang.elf

ALL_M4=\
	hello-world-metro-m4.elf \
	hello-world-metro-m4-zephyr.elf \
	hello-world-metro-m4-arm-etk.elf

ALL=$(ALL_M0) $(ALL_M4)

all: $(ALL)

hello-world-rp2040.elf: hello-world.c rp2040.ld
	arm-none-eabi-gcc -o $@ $(CFLAGS_M0) hello-world.c

hello-world-rp2040-zephyr.elf: hello-world.c rp2040.ld
	zephyr-1 arm-zephyr-eabi-gcc -o $@ $(CFLAGS_M0) hello-world.c

hello-world-rp2040-arm-etk.elf: hello-world.c rp2040.ld
	arm-etk arm-none-eabi-gcc --specs=picolibc.specs -o $@ $(CFLAGS_M0) hello-world.c

hello-world-rp2040-ctng.elf:  hello-world.c rp2040.ld
	~/x-tools/arm-picolibc-eabi/bin/arm-picolibc-eabi-gcc -o $@ $(CFLAGS_M0) hello-world.c

hello-world-rp2040-clang.elf:  hello-world.c rp2040.ld
	atfe clang $(CLANG_FLAGS_M0) -o $@ hello-world.c

hello-world-metro-m4.elf: hello-world.c rp2040.ld
	arm-none-eabi-gcc -o $@ $(CFLAGS_M4) hello-world.c

hello-world-metro-m4-zephyr.elf: hello-world.c rp2040.ld
	zephyr-1 arm-zephyr-eabi-gcc -o $@ $(CFLAGS_M4) hello-world.c

hello-world-metro-m4-arm-etk.elf: hello-world.c rp2040.ld
	arm-etk arm-none-eabi-gcc --specs=picolibc.specs -o $@ $(CFLAGS_M4) hello-world.c

hello-world-metro-m4-clang.elf:  hello-world.c rp2040.ld
	atfe clang $(CLANG_FLAGS_M4) -o $@ hello-world.c

clean:
	rm -f $(ALL)

run: hello-world-metro-m4.elf
	qemu-system-arm -chardev stdio,mux=on,id=stdio0 -semihosting-config enable=on,chardev=stdio0 \
		-monitor none -serial none -machine mps2-an386,accel=tcg -cpu cortex-m4 \
		-device loader,file=./hello-world-metro-m4.elf,cpu-num=0 -nographic
