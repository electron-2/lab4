NAME = blink
LIBRARY_MODULES = gpio.o timer.o

CS107E=../../../cs107e.github.io/cs107e
CFLAGS = -I$(CS107E)/include -g -Wall -Og -std=c99 -ffreestanding -mapcs-frame -fno-omit-frame-pointer
LDFLAGS = -nostdlib -T memmap -L$(CS107E)/lib -L.
LDLIBS = -lmypi


all : libmypi.a $(NAME).bin

libmypi.a: $(LIBRARY_MODULES)
	arm-none-eabi-ar crf $@ $^

%.bin: %.elf
	arm-none-eabi-objcopy $< -O binary $@

%.elf: %.o start.o cstart.o
	arm-none-eabi-gcc $(LDFLAGS) $^ $(LDLIBS) -o $@

%.o: %.c
	arm-none-eabi-gcc $(CFLAGS) -c $< -o $@

%.o: %.s
	arm-none-eabi-as $(ASFLAGS) $< -o $@

%.list: %.o
	arm-none-eabi-objdump --no-show-raw-insn -d $< > $@

%.elf.list: %.elf
	arm-none-eabi-objdump --no-show-raw-insn -d $< > $@

install: $(NAME).bin
	rpi-install.py -p $<

clean:
	rm -f *.o *.bin *.elf *.list *~

.PHONY: all clean install

.PRECIOUS: %.elf %.o

%:%.c
%:%.o
