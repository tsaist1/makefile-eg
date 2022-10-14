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
	coco.c \
	cherry.c

SRCS              := $(SRCS:%=$(SRC_DIR)/%)
x86_OBJS          := $(SRCS:$(SRC_DIR)/%.c=x86/%.o)
arm64_OBJS        := $(SRCS:$(SRC_DIR)/%.c=arm64/%.o)

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
	$(AR) $(ARFLAGS) libarom_x86.a $(x86_OBJS)
	$(info CREATED $@)

arm64: $(arm64_OBJS)
	$(AR) $(ARFLAGS) libarom_arm64.a $(arm64_OBJS)
	$(info CREATED $@)

x86/%.o: $(SRC_DIR)/%.c
	$(DIR_DUP)
	$(CC) $(CFLAGS) $(CPPFLAGS) -arch x86_64 -c $< -o $@
	$(info CREATED $@)

arm64/%.o: $(SRC_DIR)/%.c
	$(DIR_DUP)
	$(CC) $(CFLAGS) $(CPPFLAGS) -arch arm64 -c $< -o $@
	$(info CREATED $@)

clean:
	$(RM) $(x86_OBJS) $(arm64_OBJS) x86 arm64 *.a

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
