all: 8b_yolo

# ==========
#  Clean
# ==========

clean:
	cd ../fsim/build && make clean && make && cd ../../workspace

# ==========
#  Copying Files (switching 8b vs 16b)
# ==========

copy8: # Default 8-bit weights etc
	cd tinyyolo/tinyyolo && make copy8

copy16: # Generated 16-bit weights etc
	cd tinyyolo/tinyyolo && make copy16

# ==========
#  Running Examples
# ==========

test:
	pytest -v ../test/utest.py -k test_tiny_yolo

8b_yolo: # Fully TinyYOLO network
	make copy8                                                          # copy 8b weights etc
	mv ../fsim/defines.h ../fsim/defines.h.0                            # temporarily move defines.h
	echo "#define OPU8" > ../fsim/defines.h                             # write defines.h
	make test                                                           # run test
	mv ../fsim/defines.h.0 ../fsim/defines.h                            # restore defines.h

8b_simple: # Simple sequence of load->compute->store (8b)
	make copy8                                                          # copy 8b weights etc
	mv ../fsim/defines.h ../fsim/defines.h.0                            # temporarily move defines.h
	echo "#define OPU8\n#define MANUAL_INSTRS" > ../fsim/defines.h      # write defines.h
	(make test || true)                                                 # run test
	mv ../fsim/defines.h.0 ../fsim/defines.h                            # restore defines.h
	echo "\nChecking results:\n\nDiff:\n----" && \
	diff tinyyolo/out.txt tinyyolo/8b_outputs/out.txt && \
	echo "----"

16b_simple: # Simple sequence of load->compute->store (16b)
	make copy16                                                          # Copy 16b weights etc
	mv ../fsim/defines.h ../fsim/defines.h.0                             # temporarily move defines.h
	echo "#define OPU16\n#define MANUAL_INSTRS" > ../fsim/defines.h      # write defines (configuration)
	(make test || true)                                                  # run test
	mv ../fsim/defines.h.0 ../fsim/defines.h                             # restore defines.h
	echo "\nChecking results:\n\nDiff:\n----" && \
	diff tinyyolo/out.txt tinyyolo/16b_outputs/out.txt && \
	echo "----"
