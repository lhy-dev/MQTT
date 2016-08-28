ifndef CROSS_COMPILE
CROSS_COMPILE	:= arm-linux-gnueabihf-
endif

ifndef ARCH_FLAG
ARCH_FLAG		:=
endif

ifndef CPU_FLAG
CPU_FLAG		:= -mcpu=cortex-a8
endif

ifndef VERSION_NUMBER
VERSION_NUMBER	:= 
endif

ifndef OUT_DIR
OUT_DIR			:= $(shell pwd)/output
endif

ifndef OBJ_DIR
OBJ_DIR			:= $(OUT_DIR)/obj
endif

ifndef MKDIR
ifdef SystemRoot 						# for Windows sytem
MKDIR			:= mkdir.exe
else
MKDIR			:= mkdir
endif
endif

export CROSS_COMPILE
export ARCH_FLAG
export CPU_FLAG
export VERSION_NUMBER
export OUT_DIR
export OBJ_DIR
export DEBUG_FLAG

.DEFAULT_GOAL	:= all

all:
	@$(MKDIR) -p $(OUT_DIR)
	@$(MKDIR) -p $(OBJ_DIR)
	@cd $(shell pwd)/src && make

clean:
	@cd $(shell pwd)/src && make clean

.PHONY: all clean
