
INCLUDEDIR = .

LzmaAlone=../../../../$(SQUASHFS_LZMALIB_DIR1)
LzmaC=../../../../$(SQUASHFS_LZMALIB_DIR2)

CFLAGS := -I$(INCLUDEDIR) -D_FILE_OFFSET_BITS=64 -D_LARGEFILE_SOURCE -O2
ifdef UseDebugFlags
DebugFlags = -g -Wall -Wno-unused-variable -O0 -UNDEBUG
endif
CFLAGS += -D_REENTRANT -DNDEBUG ${DebugFlags}
LDLIBS += -lz -L${LzmaAlone} -L${LzmaC}
Tgt = mksquashfs unsquashfs

all: ${Tgt}

read_fs.o: read_fs.c squashfs_fs.h read_fs.h global.h \
	sqlzma.h sqmagic.h

sort.o: sort.c squashfs_fs.h global.h sort.h

mksquashfs.o: mksquashfs.c squashfs_fs.h mksquashfs.h global.h sort.h \
	sqlzma.h sqmagic.h $(LzmaAlone)/liblzma_r.a $(LzmaC)/libunlzma_r.a

mksquashfs: LDLIBS += -lpthread -lunlzma_r -llzma_r -lstdc++
mksquashfs: mksquashfs.o read_fs.o sort.o

unsquashfs.o: CFLAGS += -U_REENTRANT
unsquashfs.o: unsquashfs.c squashfs_fs.h read_fs.h global.h \
	sqlzma.h sqmagic.h $(LzmaC)/libunlzma.a

unsquashfs: LDLIBS += -lunlzma
unsquashfs: unsquashfs.o

clean:
	${RM} *~ *.o ${Tgt}

-include test.mk

# Local variables: ;
# compile-command: (concat "make Sqlzma=../.. -f " (file-name-nondirectory (buffer-file-name)));
# End: ;
