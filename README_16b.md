# File Structure

OPU_Simulator

	fsim/

		accelerator.h    - Main accelerator header

		accelerator.cc   - Main accelerator source file

		driver.cc        - Driver to run layer(s)

                defines.h        - Defines for 8 vs 16b and manual instructions vs single layer vs network

	test/

		utest.py         - Main test file

		defines.py       - Contains Debug macros to modify (set #defines)

	workspace/               - Run make from here!!!

		Makefile         - Rebuilds from scratch and runs simulator

		tinyyolo/

			out.txt       - Current log output

			8b_outputs/   - Final 8b outputs of each type

			16b_outputs/  - Final 16b outputs of each type

			tinyyolo/     - Has initial weights/bias/ifm

				test.py  - Has data rearrangement to get 16b ifm/weight/bias values

				Makefile - Allows quickly switching between 8 and 16bit binaries


# Prerequisites

- Copy the tinyyolo folder from Google Drive into /workspace (contains weights etc) - https://drive.google.com/file/d/16fpXPOCBABgVh_WHkt7P9xqLtLGwn71m/view?usp=sharing

# Running Simulator for 8 and 16-bit

(1) Generate modified data for 16-bit
- Run "python3 test.py" in workspace/tinyyolo/tinyyolo to generate 16b ifm/weights/bias

(2) Switch between 8b vs 16b
- In workspace/tinyyolo/tinyyolo, copy over the 8/16 bias, ifm, weights using the Makefile (make copy8/copy16)
- defines.h - #define OPU8/OPU16, and #define MANUAL_INSTRS/SINGLE_LAYER/neither (use MANUAL_INSTRS for simple test, or neither for full yolov3 network)

(3) Run
- pick which debug point to use in test/defines.py (by uncommenting the correct DEBUG_DEFINE)
- go to /workspace and run "make test"
- compare /workspace/tinyyolo/out.txt to the appropriate reference in /workspace/tinyyolo/16b_outputs (or 8b_outputs)

# Simpler way to run simulator (using predefined make targets)

(1) Run step 1 from the previous section to generate 16b ifm/weights/bias


(2) Go to /workspace and run one of three targets:

- "make 8b_yolo" (full yolov3 network, 8b)
- "make 8b_simple" (8b sequence of load->compute->store)
- "make 16b_simple" (16b sequence of load->compute->store)

The two simple tests will both compare outputs automatically by doing a "diff" with the expected outputs.

Note: the 8b_simple and 16b_simple tests will both show a pytest failure. This is expected. To make sure it runs correctly, make sure the "diff" section is correct.
