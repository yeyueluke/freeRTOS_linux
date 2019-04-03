######## Build options ########
#gcc -I ../Project/ -I include/ -I portable/GCC/POSIX/ -c -m32 *.c portable/GCC/POSIX/port.c portable/MemMang/*.c
#ar -rcs libfreertos.a *.o

verbose = 1 

######## Build setup ########

# SRCROOT should always be the current directory
SRCROOT         = $(CURDIR)

# .o directory
ODIR            = obj

# Source VPATHS
VPATH           = $(SRCROOT)
VPATH	        += $(SRCROOT)/portable/MemMang
VPATH	        += $(SRCROOT)/portable/GCC/POSIX

# FreeRTOS Objects
C_FILES			+= croutine.c
C_FILES			+= event_groups.c
C_FILES			+= list.c
C_FILES			+= queue.c
C_FILES			+= tasks.c
C_FILES			+= timers.c

# portable Objects
C_FILES			+= heap_3.c
C_FILES			+= port.c

# Include Paths
INCLUDES        += -I$(SRCROOT)/include
INCLUDES        += -I$(SRCROOT)/portable/GCC/POSIX/
INCLUDES        += -I$(SRCROOT)/../Project

# Generate OBJS names
OBJS = $(patsubst %.c, %.o, $(C_FILES))

######## C Flags ########

# Warnings
CWARNS += -W
CWARNS += -Wall
CWARNS += -Werror
CWARNS += -Wextra
CWARNS += -Wformat
CWARNS += -Wmissing-braces
CWARNS += -Wno-cast-align
CWARNS += -Wparentheses
CWARNS += -Wshadow
CWARNS += -Wno-sign-compare
CWARNS += -Wswitch
CWARNS += -Wuninitialized
CWARNS += -Wunknown-pragmas
CWARNS += -Wunused-function
CWARNS += -Wunused-label
CWARNS += -Wunused-parameter
CWARNS += -Wunused-value
CWARNS += -Wunused-variable
CWARNS += -Wmissing-prototypes

#CWARNS += -Wno-unused-function
CC		= gcc
AR		= ar

CFLAGS += -m32  #work on 32-bit linux,comment it for 64-bit
CFLAGS += -DDEBUG=1
#CFLAGS += -g -DUSE_STDIO=1 -D__GCC_POSIX__=1
CFLAGS += -g -UUSE_STDIO -D__GCC_POSIX__=1
ifneq ($(shell uname), Darwin)
CFLAGS += -pthread
endif

AFLAGS	= -rcs

# MAX_NUMBER_OF_TASKS = max pthreads used in the POSIX port. 
# Default value is 64 (_POSIX_THREAD_THREADS_MAX), the minimum number required by POSIX.
CFLAGS += -DMAX_NUMBER_OF_TASKS=300

CFLAGS += $(INCLUDES) $(CWARNS) -O2

######## Makefile targets ########

# Rules
.PHONY : all
all: setup libfreertos.a 

.PHONY : setup
setup:
# Make obj directory
	@mkdir -p $(ODIR)

# Fix to place .o files in ODIR
_OBJS = $(patsubst %, $(ODIR)/%, $(OBJS))

$(ODIR)/%.o: %.c
# If verbose, print gcc execution, else hide
ifeq ($(verbose),1)
	@echo ">> Compiling $<"
	$(CC) $(CFLAGS) -c -o $@ $<
else
	@echo ">> Compiling $(notdir $<)"
	@$(CC) $(CFLAGS) -c -o $@ $<
endif

#LINKFLAGS = -LSource
#LIBS	= -lfreertos
libfreertos.a: $(_OBJS)
	@echo ">> Generating $@..."
ifeq ($(verbose),1)
	$(AR) $(AFLAGS) $@ $^ 
else
	@$(AR) $(AFLAGS) $@ $^ 
endif

	@echo "-------------------------"
	@echo "BUILD COMPLETE: $@"
	@echo "-------------------------"

.PHONY : clean
clean:
	@-rm -rf $(ODIR) libfreertos.a
	@echo "--------------"
	@echo "CLEAN COMPLETE"
	@echo "--------------"
