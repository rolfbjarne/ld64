
XCODE=/Applications/Xcode5.app
LLVM=/mono/main/svn/llvm/src
DYLD=/Users/rolf/Downloads/dyld-239.3

OBJECTS = \
	.libs/parsers/archive_file.o \
	.libs/parsers/lto_file.o \
	.libs/parsers/macho_dylib_file.o \
	.libs/parsers/macho_relocatable_file.o \
	.libs/parsers/opaque_section_file.o \
	.libs/passes/stubs/stubs.o \
	.libs/passes/branch_island.o \
	.libs/passes/compact_unwind.o \
	.libs/passes/dylibs.o \
	.libs/passes/huge.o \
	.libs/passes/order.o \
	.libs/passes/branch_shim.o \
	.libs/passes/dtrace_dof.o \
	.libs/passes/got.o \
	.libs/passes/objc.o \
	.libs/passes/tlvp.o \
 	.libs/InputFiles.o	\
 	.libs/Options.o	\
 	.libs/OutputFile.o	\
 	.libs/Resolver.o	\
 	.libs/Snapshot.o	\
 	.libs/SymbolTable.o		\
 	.libs/ld_vers.o \
 	.libs/debugline.o \
 	.libs/ld.o


INC_EXTRA=-idirafter $(XCODE)/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS7.0.sdk/usr/include -I$(LLVM)/include -I$(DYLD)/include
SYSROOT= -isysroot $(XCODE)/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.8.sdk
CXX=$(XCODE)/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/clang++
CC=$(XCODE)/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/clang

CC_FLAGS=-g -arch x86_64 -fmessage-length=0 -pipe -Wno-trigraphs -fpascal-strings -O0 \
    -mdynamic-no-pic -Werror -Wmissing-prototypes -Wreturn-type -Woverloaded-virtual -Wmissing-braces -Wparentheses -Wswitch -Wunused-function -Wunused-label \
    -Wunused-variable -Wunused-value -Wunknown-pragmas -Wfour-char-constants -Wsign-compare -Wnewline-eof -DDEBUG \
   	$(SYSROOT) \
    -fasm-blocks -Wno-invalid-offsetof -gdwarf-2 -fvisibility=hidden -fvisibility-inlines-hidden \
    -I$(XCODE)/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/local/include \
    -I$(XCODE)/Contents/Developer/usr/local/include \
    -I$(XCODE)/Contents/Developer/usr/include \
     -Wall \
     -I../abstraction -I../ld -Iparsers \
     $(INC_EXTRA) \
     -I$(XCODE)/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.8.sdk/usr/include

Q_CC=  $(if $(V),,@echo "CC       $(@F)";)
Q_LINK=$(if $(V),,@echo "LINK     $(@F)";)

all: bin/custom-ld

lib/libLTO.dylib: $(XCODE)/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/libLTO.dylib
	@mkdir -p lib
	@cp $^ $@

bin/custom-ld: $(OBJECTS) lib/libLTO.dylib
	@mkdir -p $(shell dirname $@)
	$(Q_LINK) $(CXX) -g -o $@ $(OBJECTS) lib/libLTO.dylib

.libs/%.o: %.cpp
	@mkdir -p $(shell dirname $@)
	$(Q_CC) $(CXX) $(CC_FLAGS) -c $^ -o $@

.libs/%.o: %.c
	@mkdir -p $(shell dirname $@)
	$(Q_CC) $(CC) $(CC_FLAGS) -c $^ -o $@


clean:
	rm -Rf .libs lib bin

.SUFFIXES: