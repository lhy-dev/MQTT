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

ifndef DEBUG_FLAG
DEBUG_FLAG		:= NDEBUG
endif

PROJECT_NAME	:= libpaho-mqtt3as
VERSION_INFO	:= $(VERSION_NUMBER) rev $(shell svnversion -n .)
PREDEFINE		:= _GNU_SOURCE $(DEBUG_FLAG) 
CFLAGS			:= -O -g -Wall -fPIC $(PREDEFINE:%=-D %) 
LIBS			:= 
USER_LIBS		:= 
LDFLAGS			:= -pthread -lssl -lcrypto -shared $(LIBS:%=-l%) $(USER_LIBS:%=-l%)
TARGET			:= $(PROJECT_NAME).so

ifndef OUT_DIR
OUT_DIR			:= $(shell pwd)/output
endif

ifndef OBJ_DIR
OBJ_DIR			:= $(OUT_DIR)/obj
MY_OBJ_DIR		:= $(OBJ_DIR)
else
MY_OBJ_DIR		:= $(OBJ_DIR:%=%/$(PROJECT_NAME))
endif

ifndef INC_DIR
INC_DIR			:= -I"$(shell pwd)/../" 
endif

ifndef LIB_DIR
LIB_DIR			:= -L$(OUT_DIR)
endif

ifeq ($(CPU_FLAG),-mcpu=cortex-a8)
	LIB_DIR	+= -L"lib/armv7-linux-gcc"
else
	LIB_DIR	+= -L"lib/x86-linux-gcc"
endif


SRC_DIR			:= 
SRC_DIR			:= 
SRCS_C			:= Clients.c
SRCS_C			+= Heap.c
SRCS_C			+= LinkedList.c
SRCS_C			+= Log.c
SRCS_C			+= Messages.c
SRCS_C			+= MQTTAsync.c
SRCS_C			+= MQTTPacket.c
SRCS_C			+= MQTTPacketOut.c
SRCS_C			+= MQTTPersistence.c
SRCS_C			+= MQTTPersistenceDefault.c
SRCS_C			+= MQTTProtocolClient.c
SRCS_C			+= MQTTProtocolOut.c
SRCS_C			+= Socket.c
SRCS_C			+= SocketBuffer.c
SRCS_C			+= SSLSocket.c
SRCS_C			+= StackTrace.c
SRCS_C			+= Thread.c
SRCS_C			+= Tree.c
SRCS_C			+= utf-8.c 

OBJS      		:= $(SRCS_C:%.c=$(MY_OBJ_DIR)/%.o)
DEPS			:= $(OBJS:%.o=%.d)

ifneq ($(MAKECMDGOALS),clean)
ifneq ($(strip $(DEPS)),)
-include $(DEPS)
endif
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

.DEFAULT_GOAL	:= $(TARGET)

all:$(TARGET)

$(TARGET):$(OUT_DIR)/$(TARGET)

$(OUT_DIR)/$(TARGET):$(OBJS) $(USER_LIBS:%=$(OUT_DIR)/lib%.so)
	@echo Building target: $@
	@$(MKDIR) -p $(OUT_DIR)
	@$(CROSS_COMPILE)gcc $(ARCH_FLAG) $(CPU_FLAG) $(CFLAGS) $(LDFLAGS) $(INC_DIR) $(LIB_DIR) -Wl,-soname=$(TARGET) -Wl,-Map=$(OUT_DIR)/$(TARGET).map -o $(OUT_DIR)/$(TARGET) $(OBJS)
	@echo $(VERSION_INFO) > version_info.tmp
	@$(CROSS_COMPILE)objcopy --add-section version_number=version_info.tmp $(OUT_DIR)/$(TARGET)
	@$(RM) version_info.tmp
	@echo Building finished.

$(MY_OBJ_DIR)/%.o:%.c
	@echo Compiling: $<
	@$(MKDIR) -p $(MY_OBJ_DIR)/$(SRC_DIR)	
	@$(CROSS_COMPILE)gcc -c $(ARCH_FLAG) $(CPU_FLAG) $(CFLAGS) $(INC_DIR)  -MMD -MP -MF"$(@:%.o=%.d)" -MT"$@" -c -o "$@" "$<"

clean:
	@$(RM) -r $(OBJ_DIR) $(OUT_DIR)/*.so*

.PHONY: all clean $(TARGET)
