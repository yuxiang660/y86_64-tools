# Modify this line to indicate the default version

VERSION=std

# Comment this out if you don't have Tcl/Tk on your system

# GUIMODE=-DHAS_GUI

# Modify the following line so that gcc can find the libtcl.so and
# libtk.so libraries on your system. You may need to use the -L option
# to tell gcc which directory to look in. Comment this out if you
# don't have Tcl/Tk.

# Please use ubuntu18 to install tcl8.5, ubuntu20 install tcl8.6 by default which doesn't work for the code
# Install command:
#	sudo apt install tcl8.5-dev tk8.5-dev

# TKLIBS=-L/usr/lib/x86_64-linux-gnu -ltk -ltcl

# Modify the following line so that gcc can find the tcl.h and tk.h
# header files on your system. Comment this out if you don't have
# Tcl/Tk.

TKINC=-isystem /usr/include/tcl

# Modify these two lines to choose your compiler and compile time
# flags.

CC=gcc
# -DUSE_INTERP_RESULT is used to workaround tcl8.6 compile error: ‘Tcl_Interp’ {aka ‘struct Tcl_Interp’} has no member named ‘result’
CFLAGS=-Wall -Og -g -DUSE_INTERP_RESULT

##################################################
# You shouldn't need to modify anything below here
##################################################

MISCDIR=../misc
HCL2C=$(MISCDIR)/hcl2c
INC=$(TKINC) -I$(MISCDIR) $(GUIMODE)
LIBS=$(TKLIBS) -lm
YAS=../misc/yas

all: ssim

# This rule builds the SEQ simulator (ssim)
ssim: seq-$(VERSION).hcl ssim.c  sim.h $(MISCDIR)/isa.c $(MISCDIR)/isa.h
	# Building the seq-$(VERSION).hcl version of SEQ
	$(HCL2C) -n seq-$(VERSION).hcl <seq-$(VERSION).hcl >seq-$(VERSION).c
	$(CC) $(CFLAGS) $(INC) -o ssim \
		seq-$(VERSION).c ssim.c $(MISCDIR)/isa.c $(LIBS)

# This rule builds the SEQ+ simulator (ssim+)
ssim+: seq+-std.hcl ssim.c sim.h $(MISCDIR)/isa.c $(MISCDIR)/isa.h 
	# Building the seq+-std.hcl version of SEQ+
	$(HCL2C) -n seq+-std.hcl <seq+-std.hcl >seq+-std.c
	$(CC) $(CFLAGS) $(INC) -o ssim+ \
		seq+-std.c ssim.c $(MISCDIR)/isa.c $(LIBS)

# These are implicit rules for assembling .yo files from .ys files.
.SUFFIXES: .ys .yo
.ys.yo:
	$(YAS) $*.ys


clean:
	rm -f ssim ssim+ seq*-*.c *.o *~ *.exe *.yo *.ys

run:
	./ssim ../y86-code/asum.yo

run_gui:
	./ssim -g ../y86-code/asum.yo

debug_gui:
	gdb -q ./ssim \
	-ex "start -g ../y86-code/asum.yo"

