####################################### BEG_4 ####
# @author   clemedon (Clément Vidon)

#------------------------------------------------#
#   INGREDIENTS                                  #
#------------------------------------------------#
# SRC_DIR   source directory
# BUILD_DIR object directory
# SRCS      source files
#
# OBJS      object files
# DEPS      dependency files
#
# CC        compiler
# CFLAGS    compiler flags
# CPPFLAGS  preprocessor flags

SRC_DIR     := src
BUILD_DIR   := .build
SRCS        := \
	milk.c \
	water.c

SRCS        := $(SRCS:%=$(SRC_DIR)/%)
ARM_OBJS        := $(SRCS:$(SRC_DIR)/%.c=arm/%.o)
X86_OBJS        := $(SRCS:$(SRC_DIR)/%.c=x86/%.o)
DEPS        := $(OBJS:.o=.d)

CC          := clang
CFLAGS      := -Wall -Wextra -Werror
CPPFLAGS    := -MMD -MP -I include
AR          := ar
ARFLAGS     := -r -c -s

#------------------------------------------------#
#   UTENSILS                                     #
#------------------------------------------------#
# RM        force remove
# MAKEFLAGS make flags
# DIR_DUP   duplicate directory tree

RM          := rm -f
MAKEFLAGS   += --no-print-directory
DIR_DUP     = mkdir -p $(@D)

#------------------------------------------------#
#   RECIPES                                      #
#------------------------------------------------#
# all       default goal
# $(NAME)   link .o -> library
# %.o       compilation .c -> .o
# clean     remove .o
# fclean    remove .o + binary
# re        remake default goal

all: lx86.a larm64.a

lx86.a:: $(X86_OBJS)
	$(AR) $(ARFLAGS) lx86.a $(X86_OBJS)
	
larm64.a:: $(ARM_OBJS)
	$(AR) $(ARFLAGS) larm64.a $(ARM_OBJS)

x86/%.o:: $(SRC_DIR)/%.c
	$(DIR_DUP)
	$(CC) $(CFLAGS) $(CPPFLAGS) -arch x86_64 -c $< -o $@
	$(info x86 $@)

arm/%.o:: $(SRC_DIR)/%.c
	$(DIR_DUP)
	$(CC) $(CFLAGS) $(CPPFLAGS) -arch arm64 -c $< -o $@
	$(info arm64 $@)

-include $(DEPS)

clean:
	$(RM) $(X86_OBJS) $(ARM_OBJS) $(DEPS) *.a
re:
	$(MAKE) clean
	$(MAKE) all

#------------------------------------------------#
#   SPEC                                         #
#------------------------------------------------#

.PHONY: all clean fclean re
.SILENT:

####################################### END_4 ####
