# @author   clemedon (Clément Vidon)
####################################### BEG_4 ####


#------------------------------------------------#
#   INGREDIENTS                                  #
#------------------------------------------------#
# SRC_DIR   source directory
# SRCS      source files
#
# BUILD_DIR object directory
# OBJS      object files
# DEPS      dependency files
#
# CC        compiler
# CFLAGS    compiler flags
# CPPFLAGS  preprocessor flags

SRC_DIR     := src
SRCS        :=  \
	arom/coco.c \
	base/milk.c \
	base/water.c
SRCS        := $(SRCS:%=$(SRC_DIR)/%)

BUILD_DIR         := .build
x86_OBJS          := $(SRCS:$(SRC_DIR)/%.c=x86/%.o)
arm64_OBJS        := $(SRCS:$(SRC_DIR)/%.c=arm64/%.o)
DEPS              := $(OBJS:.o=.d)

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

all: x86 arm64

x86: $(x86_OBJS) 
	$(AR) $(ARFLAGS) x86_lib.a $(x86_OBJS) 

arm64: $(arm64_OBJS) 
	$(AR) $(ARFLAGS) arm_lib.a $(arm64_OBJS) 

x86/%.o: $(SRC_DIR)/%.c
	$(DIR_DUP)
	$(CC) $(CFLAGS) $(CPPFLAGS) -arch x86_64 -c -o $@ $<

arm64/%.o: $(SRC_DIR)/%.c
	$(DIR_DUP)
	$(CC) $(CFLAGS) $(CPPFLAGS) -arch arm64 -c -o $@ $<


clean:
	$(RM) x86 arm64 $(DEPS) *.a

re:
	$(MAKE) clean
	$(MAKE) all

#------------------------------------------------#
#   SPEC                                         #
#------------------------------------------------#

.PHONY: clean fclean re

####################################### END_4 ####
