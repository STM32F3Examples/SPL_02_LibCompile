INCLUDE_DIRS:= device/include cmsis/include ../include ../STM32F30x_StdPeriph_Driver/inc
SOURCE_DIRS:=  ../STM32F30x_StdPeriph_Driver/src

#Target Library Name
TLIB_NAME:=libSPL_STM32F3.a
#All the dtandard peripherial library files
SPL_FILES:= $(notdir $(wildcard ../STM32F30x_StdPeriph_Driver/src/*.c ))
#the C files to compile
CFILES:= $(SPL_FILES)
#the assembly files assemble
ASM_FILES:=
#the target microcontroller
TCHIP=STM32F302x8
#The compiler to use
CC=arm-none-eabi-gcc
#LINKER OPTIONS
#The linker script for the microcontroller 
LD_SCRIPTS:= 
#the directories for linker files 
LIB_DIRS:= 
#the libraries to link 
LIB_NAMES:=
#linker flags 
LFLAGS:= $(patsubst %,-L%,$(LIB_DIRS)) $(patsubst %,-l%,$(LIB_NAMES)) -T$(LD_SCRIPTS) 
#PREPROCESSOR OPTIONS
#Define macro for all files
DEFINES:= USE_STDPERIPH_DRIVER
DEFINE_FLAGS:=$(patsubst %,-D%,$(DEFINES))
INCLUDE_FLAGS:=$(patsubst %,-I%,$(INCLUDE_DIRS)) 
CFLAGS= -mthumb -mcpu=cortex-m4 -mfloat-abi=hard -mfpu=fpv4-sp-d16 -std=c99 -Wall -g -ffunction-sections -fdata-sections 
CPPFLAGS= $(INCLUDE_FLAGS) $(DEFINE_FLAGS) -D$(TCHIP) 
vpath %.h $(INCLUDE_DIRS)
vpath %.c $(SOURCE_DIRS)
vpath %.S device/source
vpath %.o objects

COBJECTS:= $(patsubst %.c,%.o,$(CFILES)) 
ASMOBJECTS:= $(patsubst %.S,%.o,$(ASM_FILES))

all: $(TLIB_NAME)
objects:
	mkdir objects
$(TLIB_NAME): objects $(COBJECTS) $(ASMOBJECTS)
	arm-none-eabi-ar rcs $@ objects/*.o
$(COBJECTS): %.o: %.c
	$(CC) $(CFLAGS) $(CPPFLAGS) -c $< -o objects/$@
$(ASMOBJECTS): %.o: %.S
	$(CC) $(CFLAGS) $(CPPFLAGS) -c $< -o objects/$@
.PHONY:clean rebuild burn
clean:
	rm -f  *.a
	rm -fr objects
rebuild: clean all
