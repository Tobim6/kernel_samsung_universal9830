# SPDX-License-Identifier: GPL-2.0
# scripts/dtc makefile

hostprogs-y	:= dtc
always		:= $(hostprogs-y)

dtc-objs	:= dtc.o flattree.o fstree.o data.o livetree.o treesource.o \
		   srcpos.o checks.o util.o
dtc-objs	+= dtc-lexer.lex.o dtc-parser.tab.o

# Source files need to get at the userspace version of libfdt_env.h to compile
HOST_EXTRACFLAGS := -I$(src)/libfdt

# This kernel host-tool build runs inside Soong's sandboxed genrule, which
# only exposes explicitly declared inputs -- not arbitrary host paths like
# /usr/include. The $(wildcard /usr/include/yaml.h) check below therefore
# can't see the host's libyaml even when it's installed, and unconditionally
# forcing yamltree.o/-lyaml on would just fail to compile inside the sandbox.
# -Oyaml is an optional dtc CLI output format, not needed to produce our
# .dtb/.dtbo, so always disable it rather than depend on a host lib the
# sandboxed build can't see.
HOST_EXTRACFLAGS += -DNO_YAML

# Generated files need one more search path to include headers in source tree
HOSTCFLAGS_dtc-lexer.lex.o := -I$(src)
HOSTCFLAGS_dtc-parser.tab.o := -I$(src)

# dependencies on generated files need to be listed explicitly
$(obj)/dtc-lexer.lex.o: $(obj)/dtc-parser.tab.h
