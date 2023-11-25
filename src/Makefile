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
# This is the second assessment in the course, it's a makefile to automate the build process
#
# Use: make [TARGET] [PLATFORM-OVERRIDES]
#
# Build Targets:
#      <file>.i --> This rule generates the intermediate file resulting from the preprocessor applied to the corresponding C file. It also creates the associated dependency file (.d).
#      <file>.asm --> Generates the assembly file from the corresponding C file.
#      <file>.o --> Compiles all source files, producing the corresponding object file.
#      compile-all --> Compiles all source files without linking, generating the respective object files.
#      build --> Links the object files into a single executable named c1m2.out, generates the memory map, and provides concise information about the output code size on the command line interface (CLI).
#      clean --> Removes all generated files, including .i, .o, .d, .map, .s, and .out.
# Platform Overrides:
#      This makefile accommodates two platforms: the host Linux machine and the MSP432 microcontroller. 
#      For the MSP432, specific configurations such as its linker file (msp432p401r.lds), CPU, and architecture are defined, differing from the "host" platform.
#
#------------------------------------------------------------------------------
include sources.mk

# Platform Overrides
#PLATFORM =
BASENAME = c1m2
 
# Architectures Specific Flags
ifeq ($(PLATFORM), MSP432)
	LINKER_FILE = -T ../msp432p401r.lds 
	CPU = cortex-m4
	ARCH = armv7e-m
	SPECS = nosys.specs
else
	CPU = x86_64
	ARCH:=$(shell arch)
endif 

# Compiler Flags and Defines
ifeq ($(PLATFORM), MSP432)
	CC = arm-none-eabi-gcc
	LD = arm-none-eabi-ld
	LDFLAGS = -Wl,-Map=$(BASENAME).map $(LINKER_FILE) 
	### -Wl,option: Pass option as an option to the linker. If option contains commas, it is split into multiple options at the commas. You can use this syntax to pass an argument to the option. For 		example, -Wl,-Map,output.map passes -Map output.map to the linker. When using the GNU linker, you can also get the same effect with -Wl,-Map=output.map.
	CFLAGS = -mcpu=$(CPU) -march=$(ARCH) --specs=$(SPECS) -mfloat-abi=hard -mfpu=fpv4-sp-d16 -mthumb -Wall -Werror -O0 -g -std=c99
	### Some flags as described from GCC documentation:
	### -Wall: This enables all the warnings about constructions that some users consider questionable, and that are easy to avoid (or modify to prevent the warning), even in conjunction with macros.
	### -Werror: Make all warnings into errors. 
	### -O0: Reduce compilation time and make debugging produce the expected results. This is the default.    
	CPPFLAGs = -M -MF test.d
	DEFINEFLAG = -DMSP432
else
	CC = gcc
	#LD = 
	LDFLAGS = -Wl,-Map=$(BASENAME).map
	CFLAGS = -Wall -Werror -O0 -g -std=c99
	CPPFLAGs = -M -MF test2.d
	DEFINEFLAG = -DHOST
endif

# More Declared Variables
OBJS:= $(SOURCES:.c=.o)
ASMS:= $(SOURCES:.c=.s)
PPS:= $(SOURCES:.c=.i)
MAPS:= $(SOURCES:.c=.map)
OUTS:= $(SOURCES:.c=.out)

#---------------------------------------------------------------------------------------------------------------------------------------------------------------------- #
# Target: <file>.i
# Prerequisites: the .c file
# Output: the .d as well as .i files
#---------------------------------------------------------------------------------------------------------------------------------------------------------------------- #
%.i : %.c
	$(CC) $(CPPFLAGs) $(DEFINEFLAG) $<
	$(CC) $(DEFINEFLAG) -E -o $@ $<
#$(CC) $(CPPFLAGs) -E -o $@ $< 
### -E: Stop after the preprocessing stage; do not run the compiler proper. The output is in the form of preprocessed source code, which is sent to the standard output. 

#---------------------------------------------------------------------------------------------------------------------------------------------------------------------- #
# Target: <file>.asm
# Prerequisites: the .c file
# Output: the .asm file whether from the .c file or the object file
#---------------------------------------------------------------------------------------------------------------------------------------------------------------------- #



