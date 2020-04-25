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
LINKER_FILE = ../msp432p401r.lds
CPU = cortex-m4
ARCH = thumb
ARCHARM = armv7e-m
SPECS = nosys.specs
FLOAT= hard
FPU= fpv4-sp-d16
# Compiler Flags and Defines
CC = arm-none-eabi-gcc
LD = arm-none-eabi-ld
LDFLAGS = -Wl,-Map=$(TARGET).map -T $(LINKER_FILE)
CFLAGS = -mcpu=$(CPU) -m$(ARCH) -Wall -std=c99 -g -O0 -Werror -march=$(ARCHARM) -mfloat-abi=$(FLOAT) -mfpu=$(FPU) --specs=$(SPECS) 
CPPFLAGS = -MD

else ifeq ($(PLATFORM),HOST) 

CC= gcc
LDFLAGS= -Wl,-Map=$(TARGET).map
CFLAGS= -Wall -Werror -g -O0 -std=c99
CPPFLAGS= -MD
endif

TARGET= c1m1

OBJS=$(SOURCES:.c=.o)
PREPROCS=$(SOURCES:.c=.i)
DEPENDS=$(SOURCES:.c=.d)
ASSEMB=$(SOURCES:.c=.asm)

#Without linking
.PHONY: %.i
%.i: %.c
	$(CC) -E $< -o $@ $(CFLAGS) -D$(PLATFORM)

.PHONY: %.asm
%.asm: %.c
	$(CC) -S $< -o $@ $(CFLAGS) -D$(PLATFORM)

%.o: %.c
	$(CC) -c $< -o $@ $(CFLAGS) $(CPPFLAGS) -D$(PLATFORM)


.PHONY: compile-all
compile-all:
	$(CC) -c $(SOURCES) -D$(PLATFORM) $(INCLUDES) $(CFLAGS) $(CPPFLAGS)




#with linking

.PHONY: build
build: all

.PHONY: all
all: $(TARGET).out

$(TARGET).out: $(OBJS)
	$(CC) $(OBJS) $(CFLAGS) $(INCLUDES) $(CPPFLAGS) $(LDFLAGS) -D$(PLATFORM) -o $@

.PHONY: clean
clean:
	rm -f $(OBJS) $(TARGET).out $(TARGET).map $(PREPROCS) $(DEPENDS) $(ASSEMB)

 
