# Make sure that you are building openblas with OPENBLAS_DYNAMIC_ARCH=1
# You may have to wipe your openblas build to ensure that it is built
# with support for all architectures, or else performance may suffer.

JULIAHOME = $(abspath ../../..)
include ../../../Make.inc

JULIA_PKGDIR=$(shell echo $$JULIA_PKGDIR)
ifeq ($(JULIA_PKGDIR),)
	JULIA_PKGDIR=$(shell echo ~)/.julia
endif

all: clean dmg

dmg:
	make -C ../../../deps install-git
	make -C ../../.. dist
	tar zxf ../../../julia-*.tar.gz
	mv julia-* julia
	-mkdir -p ./julia/libexec ./julia/share
	-cp -a $(BUILD)/libexec/git* ./julia/libexec
	-cp -a $(BUILD)/share/git* ./julia/share
	rm -f julia/lib/*.{a,la}
	-mkdir dmg
	platypus -a Julia -p /bin/bash -V $(JULIA_VERSION) -u "The Julia Project" -i julia.icns -Q julia.icns -o "None" -I org.julialang -x -f julia script ./dmg/Julia-$(JULIA_VERSION)-pre-$(JULIA_COMMIT).app
	-cp -f Julia.icns dmg/.VolumeIcon.icns
	-ln -fs /Applications ./dmg/Applications
	hdiutil create Julia-$(JULIA_VERSION)-pre-$(JULIA_COMMIT).dmg -ov -volname "Julia" -imagekey zlib-level=9 -srcfolder dmg


clean:
	rm -fr julia dmg *.dmg
