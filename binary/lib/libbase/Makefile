# @author   clemedon (Clément Vidon)
####################################### BEG_4 ####

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

SRC_DIR           := src
SRCS              := \
	milk.c \
	water.c

SRCS              := $(SRCS:%=$(SRC_DIR)/%)
x86_OBJS          := $(SRCS:$(SRC_DIR)/%.c=x86/%.o)
arm64_OBJS        := $(SRCS:$(SRC_DIR)/%.c=arm64/%.o)
# DEPS              := $(OBJS:.o=.d)

CC                := clang
CFLAGS            := -Wall -Wextra -Werror
CPPFLAGS          := -MMD -MP -I include
AR                := ar
ARFLAGS           := -r -c -s

#------------------------------------------------#
#   UTENSILS                                     #
#------------------------------------------------#
# RM        force remove
# MAKEFLAGS make flags
# DIR_DUP   duplicate directory tree

RM          := rm -rf
# MAKEFLAGS   += --no-print-directory
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
	$(AR) $(ARFLAGS) libbase_x86.a $(x86_OBJS)

arm64: $(arm64_OBJS)
	$(AR) $(ARFLAGS) libbase_arm64.a $(arm64_OBJS)
	
x86/%.o: $(SRC_DIR)/%.c
	$(DIR_DUP)
	$(CC) $(CFLAGS) $(CPPFLAGS) -arch x86_64 -c $< -o $@

arm64/%.o: $(SRC_DIR)/%.c
	$(DIR_DUP)
	$(CC) $(CFLAGS) $(CPPFLAGS) -arch arm64 -c $< -o $@

clean:
	$(RM) $(arm64_OBJS) $(x86_OBJS) x86 arm64 *.a

fclean: clean
	$(RM) x86 arm64

re:
	$(MAKE) fclean
	$(MAKE) all

#------------------------------------------------#
#   SPEC                                         #
#------------------------------------------------#

.PHONY: clean fclean re
# .SILENT:

####################################### END_4 ####
