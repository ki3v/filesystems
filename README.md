# Notes for this fork

These are hacks against the macOS-modified versions of these FUSE filesystems.

This is not a general-purpose fork.  This has ugly experimental hacks to allow loading an odd Plexus-specific SVR2 fs flavor (and to build on modern macOS).  These hacks will break the ability to work with non-Plexus filesystems.

This is based primarily on this filesystem information from the <sys/filsys.h> header on the Plexus filesystem:

```
/* SID @(#)filsys.h.1.1 */
/* @(#)filsys.h.6.1 */

/*
 * Structure of the super-block
 */

struct filsys {		// XXX: KI3V -	//offset+size	/* original comment */
        ushort	s_isize;		//0+2		/* size in blocks of i-list */
        daddr_t	s_fsize;		//2+4		/* size in blocks of entire volume */
        short	s_nfree;		//6+2		/* number of addresses in s_free */
        daddr_t	s_free[NICFREE];	//8+4*50	/* free block list */
        short	s_ninode;		//208+2		/* number of i-nodes in s_inode */
        ino_t	s_inode[NICINOD];	//210+2*100	/* free i-node list */
        char	s_flock;		//410+1		/* lock during free list manipulation */
        char	s_ilock;		//411+1		/* lock during i-list manipulation */
        char	s_fmod;			//412+1		/* super block modified flag */
        char	s_ronly;		//413+1		/* mounted read-only flag*/
        time_t	s_time;			//414+4		/* last super block update */
        short	s_dinfo[4];		//418+2*4	/* device information */
        daddr_t	s_tfree;		//426+4		/* total free blocks*/
        ino_t	s_tinode;		//430+2		/* total free inodes */
        char	s_fname[6];		//432+6		/* file system name */
        char	s_fpack[6];		//438+6		/* file system pack name */
        long	s_fill[13];		//444+4*13	/* ADJUST to make sizeof filsys be 512 */
        long	s_magic;		//496+4		/* magic number to indicate new file system */ 
                                                        // XXX: KI3V - WRONG! this should be at offset 504!
                                                        // XXX: looks like Plexus compiled with wrong struct packing
                                                        // XXX: or something?
        long	s_type;			//500+4		/* type of new file system */
};

#define	FsMAGI 0xfd187e20	/* s_magic number */
#define	Fs1b	1		/* 512 byte block */
#define Fs2b	2		/* 1024 byte block */
```
