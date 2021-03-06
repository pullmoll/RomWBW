#
# this makefile does double duty.  it serves as the top level make
# and as the invoked make for the different ways that the cpm3 is built
#
# it does this by overriding OBJECTS in an invoked sub-make
#
OBJECTS = cpmldr.com cpmldr.sys cpm3res cpm3bnk zpmbios3 cpm3.sys gencpm.dat
OTHERS = cpmldr.rel biosldr.rel cpm3res.sys cpm3bnk.sys zpmbios3.spr loader.bin cpmldr.bin
OTHERS += biosldrc.rel biosldrd.rel

TOOLS = ../../Tools
include $(TOOLS)/Makefile.inc

export ZXINCDIR = ../

BIOSOBJS = bioskrnl.rel scb.rel boot.rel chario.rel 
BIOSOBJS += move.rel drvtbl.rel diskio.rel util.rel
COMMA := ,
NULL :=
SPACE := $(NULL) $(NULL)
BIOSNAMES := $(subst $(SPACE),$(COMMA),$(basename $(BIOSOBJS)))

DEFCPM3 = bnk
#DEFCPM3 = res

clean:: biosclean
	@rm -f bios3.spr bnkbios3.spr zpmbios3.spr cpmldr.com gencpm.dat options.lib ldropts.lib

biosclean:
	@rm -f $(BIOSOBJS)

cpm3res:
	make biosclean
	cp optres.lib options.lib
	cp genres.dat gencpm.dat
	make OBJECTS=bios3.spr
	$(ZXCC) gencpm -auto -display
	mv cpm3.sys cpm3res.sys
	rm gencpm.dat

cpm3bnk:
	make biosclean
	cp optbnk.lib options.lib
	cp genbnk.dat gencpm.dat
	make OBJECTS=bnkbios3.spr
	$(ZXCC) gencpm -auto -display
	mv cpm3.sys cpm3bnk.sys
	rm gencpm.dat

zpmbios3:
	make biosclean
	cp optzpm.lib options.lib
	cp genbnk.dat gencpm.dat
	make OBJECTS=zpmbios3.spr
	rm gencpm.dat

cpmldr.bin:	biosldrd.rel cpmldr.rel
	$(ZXCC) $(TOOLS)/cpm/bin/LINK -CPMLDRD[L100]=CPMLDR,BIOSLDRD
	mv cpmldrd.com cpmldr.bin

cpmldr.com: biosldrc.rel cpmldr.rel
	$(ZXCC) $(TOOLS)/cpm/bin/LINK -CPMLDRC[L100]=CPMLDR,BIOSLDRC
	mv cpmldrc.com cpmldr.com

biosldrc.rel: biosldr.z80 optcmd.lib
	cp optcmd.lib ldropts.lib	
	$(ZXCC) $(TOOLS)/cpm/bin/Z80ASM -BIOSLDR/MF
	mv biosldr.rel biosldrc.rel

biosldrd.rel: biosldr.z80 optdsk.lib
	cp optdsk.lib ldropts.lib	
	$(ZXCC) $(TOOLS)/cpm/bin/Z80ASM -BIOSLDR/MF
	mv biosldr.rel biosldrd.rel
 
bios3.spr: $(BIOSOBJS)
	$(ZXCC) $(CPM)/LINK -bios3[OS]=$(BIOSNAMES)

bnkbios3.spr: $(BIOSOBJS)
	$(ZXCC) $(CPM)/LINK -bnkbios3[B]=$(BIOSNAMES)

zpmbios3.spr: $(BIOSOBJS)
	$(ZXCC) $(TOOLS)/cpm/bin/LINK -zpmbios3[B]=$(BIOSNAMES)

cpm3.sys: cpm3$(DEFCPM3).sys
	cp cpm3$(DEFCPM3).sys cpm3.sys

gencpm.dat: gen$(DEFCPM3).dat
	cp gen$(DEFCPM3).dat gencpm.dat

cpmldr.sys: loader.bin cpmldr.bin
	cat loader.bin cpmldr.bin > $@
