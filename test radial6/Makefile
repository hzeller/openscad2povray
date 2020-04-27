# Where the povray.scad file is located
POVRAY_SCAD=../povray.scad

# Desired picture size
POVRAY_WIDTH=2048
POVRAY_HEIGHT=2048

# Get good translation and rotation matrix from the OpenSCAD status line.
OPENSCAD_VPT=[-35, 29, 42]
OPENSCAD_VPR=[59.2, 0, 45.3]
OPENSCAD_DISTANCE=1000

all: radial6.png

%.csg : %.scad
	openscad $< -o $@

%.pov.csg : %.csg
	sed 's/^\(\s*\)\([a-z]\)/\1_\2/' < $< > $@

%.pov.scad: %.pov.csg
	echo "use <$(POVRAY_SCAD)>\nopenscad2povray_init();\ninclude <$<>" > $@

%.echo : %.scad $(POVRAY_SCAD)
	openscad -Dopenscad_vpw=$(POVRAY_WIDTH) \
                 -Dopenscad_vph=$(POVRAY_HEIGHT) \
                 -D'$$vpd=$(OPENSCAD_DISTANCE)' \
                 -D'$$vpt=$(OPENSCAD_VPT)' -D'$$vpr=$(OPENSCAD_VPR)' \
                 $< -o $@

%.pov : %.pov.echo
	sed -e 's/^ECHO: "POVRAY:\(.*\)"/\1/p;d' < $< | sed -e 's/\s*$$//' > $@

%.png : %.pov
	povray +W$(POVRAY_WIDTH) +H$(POVRAY_HEIGHT) -D +FN12 +UA +A +AM2 $<
