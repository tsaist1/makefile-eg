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
CPPFLAGS    := -I include
AR          := ar
ARFLAGS     := -r -c -s

#------------------------------------------------#
#   UTENSILS                                     #
#------------------------------------------------#
# RM        force remove
# MAKEFLAGS make flags
# DIR_DUP   duplicate directory tree

RM          := rm -rf
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

all: x86/lx86.a arm/larm64.a

x86/lx86.a:: $(X86_OBJS)
	$(AR) $(ARFLAGS) x86/lx86.a $(X86_OBJS)

arm/larm64.a:: $(ARM_OBJS)
	$(AR) $(ARFLAGS) arm/larm64.a $(ARM_OBJS)

x86/%.o:: $(SRC_DIR)/%.c
	$(DIR_DUP)
	$(CC) $(CFLAGS) $(CPPFLAGS) -arch x86_64 -c $< -o $@
	$(info x86 $@)

arm/%.o:: $(SRC_DIR)/%.c
	$(DIR_DUP)
	$(CC) $(CFLAGS) $(CPPFLAGS) -arch arm64 -c $< -o $@
	$(info arm64 $@)


clean:
	$(RM) $(X86_OBJS) $(ARM_OBJS) $(DEPS) *.a *.d
re:
	$(MAKE) clean
	$(MAKE) all

#------------------------------------------------#
#   SPEC                                         #
#------------------------------------------------#

.PHONY: all clean fclean re

####################################### END_4 ####
