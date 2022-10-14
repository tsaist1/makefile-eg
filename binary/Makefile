# @author   clemedon (Clément Vidon)
####################################### BEG_5 ####

x86_LIBS   := arom_x86 base_x86 m
arm64_LIBS := arom_arm64 base_arm64 m

x86_LIBS_TARGET :=            \
	lib/libarom/libarom_x86.a \
	lib/libbase/libbase_x86.a \

arm64_LIBS_TARGET :=            \
	lib/libarom/libarom_arm64.a \
	lib/libbase/libbase_arm64.a \

INCS := include    \
	lib/libarom/include   \
	lib/libbase/include

SRC_DIR           := src
SRCS              := main.c
SRCS              := $(SRCS:%=$(SRC_DIR)/%)

x86_OBJS          := $(SRCS:$(SRC_DIR)/%.c=x86/%.o)
arm64_OBJS        := $(SRCS:$(SRC_DIR)/%.c=arm64/%.o)


CC                := clang
CFLAGS            := -Wall -Wextra -Werror
CPPFLAGS          := $(addprefix -I,$(INCS)) -MMD
x86_LDFLAGS       := $(addprefix -L,$(dir $(x86_LIBS_TARGET)))
arm64_LDFLAGS     := $(addprefix -L,$(dir $(arm64_LIBS_TARGET)))
x86_LDLIBS        := $(addprefix -l,$(x86_LIBS))
arm64_LDLIBS      := $(addprefix -l,$(arm64_LIBS))

#------------------------------------------------#
#   UTENSILS                                     #
#------------------------------------------------#
# RM        force remove
# MAKEFLAGS make flags
# DIR_DUP   duplicate directory tree

RM          := rm -rf
# MAKEFLAGS   += --silent --no-print-directory
DIR_DUP     = mkdir -p $(@D)

#------------------------------------------------#
#   RECIPES                                      #
#------------------------------------------------#
# all       default goal
# $(NAME)   link .o -> archive
# $(LIBS)   build libraries
# %.o       compilation .c -> .o
# clean     remove .o
# fclean    remove .o + binary
# re        remake default goal
# run       run the program
# info      print the default goal recipe
all: universal x86b arm64b

x86b: $(x86_OBJS) $(x86_LIBS_TARGET)
	$(CC) $(x86_LDFLAGS) $(x86_OBJS) $(x86_LDLIBS) -o x86b -target x86_64-apple-macos12

arm64b: $(arm64_OBJS) $(arm64_LIBS_TARGET)
	$(CC) $(arm64_LDFLAGS) $(arm64_OBJS) $(arm64_LDLIBS) -o arm64b -target arm64-apple-macos12

universal: x86b arm64b
	lipo -create -output universal arm64b x86b

$(x86_LIBS_TARGET):
	$(MAKE) -C $(@D)

$(arm64_LIBS_TARGET):
	$(MAKE) -C $(@D)

x86/%.o: $(SRC_DIR)/%.c
	$(DIR_DUP)
	$(CC) $(CFLAGS) $(CPPFLAGS) -arch x86_64 -c -o $@ $<
	$(info CREATED $@)

arm64/%.o: $(SRC_DIR)/%.c
	$(DIR_DUP)
	$(CC) $(CFLAGS) $(CPPFLAGS) -arch arm64 -c -o $@ $<
	$(info CREATED $@)

clean:
	for f in $(dir $(x86_LIBS_TARGET)); do $(MAKE) -C $$f clean; done
	for f in $(dir $(arm64_LIBS_TARGET)); do $(MAKE) -C $$f clean; done
	$(RM) $(x86_OBJS) $(arm64_OBJS) x86 arm64 x86b arm64b universal *.a

fclean: clean
	for f in $(dir $(LIBS_TARGET)); do $(MAKE) -C $$f fclean; done
	$(RM) $(NAME)

re:
	$(MAKE) fclean
	$(MAKE) all

info-%:
	$(MAKE) --dry-run --always-make $* | grep -v "info"

.PHONY: clean fclean re

####################################### END_5 ####
