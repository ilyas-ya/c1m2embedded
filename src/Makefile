#******************************************************************************
# Copyright (C) 2017 by Alex Fosdick - University of Colorado
#
# Redistribution, modification or use of this software in source or binary
# forms is permitted as long as the files maintain this copyright. Users are 
# permitted to modify this and use it to learn about the field of embedded
# software. Alex Fosdick and the University of Colorado are not liable for any
# misuse of this material. 
#
#*****************************************************************************

#------------------------------------------------------------------------------
# <Put a Description Here>
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <Put a description of the supported targets here>
#
# Platform Overrides:
#      <Put a description of the supported Overrides here
#
#------------------------------------------------------------------------------
include sources.mk


ifeq ($(PLATFORM),MSP432)
# Architectures Specific Flags
LINKER_FILE = msp432p401r.lds
CPU = cortex-m4
ARCH = thumb
ARCHARM = armv7e-m
SPECS = nosys.specs
FLOAT= hard
FPU= fpv4-sp-d16
# Compiler Flags and Defines
CC = arm-none-eabi-gcc
LD = arm-none-eabi-ld
PTARGET= MSP432
TARGET = c1m2
LDFLAGS = -Wl,-Map=$(TARGET).map -T $(LINKER_FILE)
CFLAGS = -mcpu=$(CPU) -m$(ARCH) -Wall -std=c99 -g -O0 -Werror -march=$(ARCHARM) -mfloat-abi=$(FLOAT) -mfpu=$(FPU) --specs=$(SPECS) 
CPPFLAGS = -M -D$(PTARGET)

else 

CC= gcc
TARGET= c1m2
PTARGET= HOST
CPPFLAGS= -D$(PTARGET)
endif


OBJS=$(SOURCES:.c=.o)

%.o: %.c
	$(CC) -c $< $(CFLAGS) $(CPPFLAGS) -o $@

.PHONY: build
build: all

.PHONY: all
all: $(TARGET).out

$(TARGET).out: $(OBJS)
	$(CC) $(OBJS) $(CFLAGS) $(INCLUDES) $(CPPFLAGS) $(LDFLAGS) -o $@

.PHONY: clean
clean:
	rm -f $(OBJS) $(TARGET).out $(TARGET).map

.PHONY: %.i
%.i: $(OBJS)
	$(CC) -E $(OBJS) -o $@

.PHONY: %.asm
%.asm:
	$(CC) -S $(OBJS) -o $@

 
