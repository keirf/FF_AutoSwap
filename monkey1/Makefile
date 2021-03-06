PREFIX = m68k-amigaos-
CC = $(PREFIX)gcc
AS = $(PREFIX)as
PYTHON = python3

CFLAGS += -MMD -MF .$(@F).d
DEPS = .*.d

CFLAGS += -nostdlib -fno-builtin -I..
CFLAGS += -Os -m68000 -Wall -Werror -std=gnu99 -msoft-float
#CFLAGS += -msmall-code
#CFLAGS += -DDEBUG
LDFLAGS += $(CFLAGS) -s
AFLAGS += $(CFLAGS) -D__ASSEMBLY__ -Wa,--register-prefix-optional
AFLAGS += -Wa,-l -Wa,--bitwise-or -Wa,--base-size-default-16
AFLAGS += -Wa,--disp-size-default-16 -Wa,--pcrel

TARGET = Monkey1-FFAS
VER = v1.5
NAME_PREFIX = Monkey1 - 

all: mi1_01.adf mi1_01_alt.adf
	rm -rf $(TARGET)*
	mkdir -p $(TARGET)
	xdftool "$(TARGET)/$(NAME_PREFIX)Save.adf" format "MonkeySave"
	cp mi1_01.adf "$(TARGET)/$(NAME_PREFIX)01.adf"
	cp mi1_01_alt.adf "$(NAME_PREFIX)01.adf"
	zip "$(TARGET)/monkey1_alt_boot.zip" "$(NAME_PREFIX)01.adf"
	rm -f "$(NAME_PREFIX)01.adf"
	cp Orig/mi_02.adf "$(TARGET)/$(NAME_PREFIX)02.adf"
	cp Orig/mi_03.adf "$(TARGET)/$(NAME_PREFIX)03.adf"
	cp Orig/mi_04.adf "$(TARGET)/$(NAME_PREFIX)04.adf"
	cp 00-README.txt $(TARGET)/.
	zip -r $(TARGET)-$(VER).zip $(TARGET)

mi1_01_alt.adf: mi1_01.adf monkey1_alt.zip
	unzip monkey1_alt.zip
	rm -rf Monkey1*
	xdftool mi1_01_alt_orig.adf unpack .
	cp mi1_autoswap Monkey1/c/.
	rm Monkey1/c/Copy
	cp startup-sequence.alt Monkey1/s/startup-sequence
	rm -rf $@
	xdftool $@ pack Monkey1
	rm -rf Monkey1*
	$(PYTHON) ../shared/bootblock_fix.py --ntsc-patch $@

mi1_01.adf: Orig/mi_01.adf ../atlantis/Orig/atlantis_01.adf mi1_autoswap
	rm -rf Monkey1* atlantis1*
	xdftool Orig/mi_01.adf unpack .
	xdftool ../atlantis/Orig/atlantis_01.adf unpack .
	cp atlantis1/c/echo Monkey1/c/.
	cp atlantis1/c/stack Monkey1/c/.
	cp atlantis1/c/run Monkey1/c/.
	cp mi1_autoswap Monkey1/c/.
	cp startup-sequence Monkey1/s/startup-sequence
	$(PYTHON) ./crack_mi1.py Monkey1/Rooms/disk01.lec
	$(PYTHON) ../pack/pack_exe.py Monkey1/Monkey_Island monkey.pack
	mv monkey.pack Monkey1/Monkey_Island
	rm -rf $@
	xdftool $@ pack Monkey1
	rm -rf Monkey1* atlantis1*
	$(PYTHON) ../shared/bootblock_fix.py --ntsc-patch $@

mi1_autoswap: mi1_autoswap_1 mi1_autoswap_2
	$(PYTHON) ../shared/hunk_cat.py -o $@ $^

mi1_autoswap_1: mi1_autoswap_entry.o
	$(CC) -o $@ $^ $(LDFLAGS)

mi1_autoswap_2: mi1_autoswap.o libnix.o crc.o
	$(CC) -o $@ $^ $(LDFLAGS)

libnix.c: ../shared/libnix.c
	ln -sf $< $@

crc.c: ../shared/crc.c
	ln -sf $< $@

mi1_autoswap_entry.S: ../shared/amigados_autoswap_entry.S
	ln -sf $< $@

%.o: %.S
	$(CC) -o $@ -c $< $(AFLAGS)

%.o: %.c
	$(CC) -o $@ -c $< $(CFLAGS)

clean:
	rm -rf *.o *.adf mi1_autoswap $(DEPS) $(TARGET)* mi1_autoswap_* crc.c libnix.c Monkey1* atlantis1*

DRIVE ?= /dev/sdb1
write: all
	sudo mount $(DRIVE) /mnt
	sudo rm -rf /mnt/$(TARGET)
	sudo cp -r $(TARGET) /mnt/.
	echo -n "$(TARGET)/$(NAME_PREFIX)01.adf" >IMAGE_A.CFG
	sudo cp IMAGE_A.CFG /mnt/.
	rm -f IMAGE_A.CFG
	sudo umount /mnt
	sudo fatsort $(DRIVE)

.PHONY: all clean write

-include $(DEPS)
