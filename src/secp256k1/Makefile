
CC = gcc
CXX = @CXX@
AS = gcc
LD = gcc
CPP = gcc
CFLAGS = -g -O2 -I/usr/local/include -DARCH_LITTLE_ENDIAN=1 -DPREFER_FLOCK -Wall -Wno-stringop-truncation -Wno-format-overflow -Wno-format-truncation -fno-omit-frame-pointer -Wno-deprecated-declarations -Wformat-extra-args -Wunused-but-set-variable -Wdate-time -D_POSIX_SOURCE -D_GNU_SOURCE -D_XOPEN_SOURCE=600  -Wno-unused-function
ASFLAGS =  -DARCH_LITTLE_ENDIAN=1 -c 
LDFLAGS =  -L/usr/local/lib -L/usr/lib64 -L/lib64  -lssl -lcrypto
YASM = 
AR = /usr/bin/ar
FIND = /usr/bin/find
RM = /bin/rm -f

IN = secp256k1.o

all: secp256k1.a
default: secp256k1.a

secp256k1.o: secp256k1.c
	$(CC) -DHAVE_CONFIG_H $(CFLAGS) -c secp256k1.c -o secp256k1.o

.PHONY: subdirs $(SUBDIRS)

subdirs: $(SUBDIRS)

$(SUBDIRS):
	$(MAKE) -C $@ all

secp256k1.a: $(SUBDIRS) secp256k1.o
	$(AR) -rs $@ $(IN)

clean:
	$(FIND) . -name \*.a -exec $(RM) {} \;
	$(FIND) . -name \*.o -exec $(RM) {} \;

distclean: clean
	$(RM) Makefile
