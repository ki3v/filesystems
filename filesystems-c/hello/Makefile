TARGETS = hello hello_ll

# Root for OSXFUSE includes and libraries
OSXFUSE_ROOT = /usr/local
#OSXFUSE_ROOT = /opt/local

INCLUDE_DIR = $(OSXFUSE_ROOT)/include/fuse
LIBRARY_DIR = $(OSXFUSE_ROOT)/lib

CC ?= gcc

CFLAGS_OSXFUSE = -I$(INCLUDE_DIR) -L$(LIBRARY_DIR)
CFLAGS_OSXFUSE += -DFUSE_USE_VERSION=26
CFLAGS_OSXFUSE += -D_FILE_OFFSET_BITS=64
CFLAGS_OSXFUSE += -D_DARWIN_USE_64_BIT_INODE

CFLAGS_EXTRA = -Wall -g $(CFLAGS)

LIBS = -lfuse

.c:
	$(CC) $(CFLAGS_OSXFUSE) $(CFLAGS_EXTRA) -o $@ $< $(LIBS)

all: $(TARGETS)

hello: hello.c

hello_ll: hello_ll.c

clean:
	rm -f $(TARGETS) *.o
	rm -rf *.dSYM
