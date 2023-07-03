# ===================================================================================
# Project:  USB-RTC (demo) - makefile
# Author:   Stefan Wagner
# Year:     2021
# URL:      https://github.com/wagiminator     
# ===================================================================================
# Type "make help" in the command line.
# ===================================================================================

# Input and Output File Names
SKETCH  = USB_RTC_demo.ino
TARGET  = usbrtc_demo

# Microcontroller Settings
DEVICE ?= attiny814
CLOCK   = 5000000
FUSE0   = 0x00
FUSE1   = 0x00
FUSE2   = 0x02
FUSE4   = 0x00
FUSE5   = 0xC5
FUSE6   = 0x04
FUSE7   = 0x00
FUSE8   = 0x00

# Toolchain
CC       = avr-gcc
OBJCOPY  = avr-objcopy
OBJDUMP  = avr-objdump
AVRSIZE  = avr-size
PYPROG   = python3 -u ./tools/tinyupdi.py -d $(DEVICE)
CLEAN    = rm -f *.lst *.obj *.cof *.list *.map *.eep.hex *.o *.s *.d

# Compiler Flags
CFLAGS   = -w -flto -Os -mmcu=$(DEVICE) -DF_CPU=$(CLOCK) -x c++
CFLAGS  += -B include/dev/$(DEVICE) -I include

# Symbolic Targets
help:
	@echo "Use the following commands:"
	@echo "make all       compile and build $(TARGET).elf/.bin/.hex/.asm for $(DEVICE)"
	@echo "make upload    compile and upload to $(DEVICE)"
	@echo "make fuses     burn fuses of $(DEVICE)"
	@echo "make install   compile, upload, and burn fuses for $(DEVICE)"
	@echo "make clean     remove all build files"

all: build removetemp size

build: $(TARGET).elf $(TARGET).bin $(TARGET).hex $(TARGET).asm

$(TARGET).elf: $(SKETCH)
	@echo "Compiling $(SKETCH) for $(DEVICE) @ $(CLOCK)Hz ..."
	@$(CC) $(CFLAGS) $< -o $@

$(TARGET).bin: $(TARGET).elf
	@echo "Building $@ ..."
	@$(OBJCOPY) -O binary -R .eeprom $< $@

$(TARGET).hex: $(TARGET).elf
	@echo "Building $@ ..."
	@$(OBJCOPY) -j .text -j .data -O ihex $< $@

