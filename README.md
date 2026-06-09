# NEWCAS
NEWCAS tutorial

## Installations

# MacOS: 
brew install yosys icarus-verilog verilator cocotb
pip install pytest

# Ubuntu / Windows Ubuntu
apt install yosys icarus-verilog verilator cocotb
pip install pytest

## Synthesis
Download gscl45nm.lib
Download problem1_tb.v

# Hardware Design and Optimization: 

Write a combinational Verilog module named signed_isqrt to compute the integer square root of x, where x is an input signed 16-bit integer and the output y is an unsigned 8-bit integer. 

x - Signed 16-bit input representing the value for integer square root computation (operational range: -32,768 to +32,767, with negative values handled as special cases) 

y - Signed 8-bit output containing the computed integer square root value (operational range: 0 to 181 for valid positive inputs, 0 for negative inputs) 

0. Save the LLM verilog file into <design_fname>.v
1. Make sure your verilog design produces SUCCESS on the testbench

verilator -Wno-LATCH -Wno-WIDTH --binary   --top-module signed_isqrt_tb problem1_tb.v <design_fname>.v
./obj_dir/Vsigned_isqrt_tb

If there are any failures, iterate with the LLM to fix the bugs.

2. Synthesis the design and report total area
read_verilog <file_name>.v
hierarchy -check -top <top_module>
proc; opt; opt; techmap; opt
dfflibmap -liberty gscl45nm.lib
abc -liberty gscl45nm.lib
stat -liberty gscl45nm.lib
write_verilog <file_name>_syn.v

run commands individually or save in a script, and run yosys -s <script_filename>

3. Optimize your design to be area efficient using the LLM. Report the final design area

#  Design Verification: 

1. report coverage of the given testbench
verilator -Wno-LATCH -Wno-WIDTH  --binary -j 0 --coverage --coverage-line --coverage-toggle --top-module signed_isqrt_tb problem1_tb.v problem1a.v
./obj_dir/Vsigned_isqrt_tb
verilator_coverage --annotate report coverage.dat

2. Use LLM to increase coverage of the testbench, re-test and report the new coverage


1. Create a Python testbench

Write a python test bench using cocotb for a a combinational Verilog module named signed_isqrt to compute the integer square root of x, where x is an input signed 16-bit integer and the output y is an unsigned 8-bit integer. Create also the Makefile

Save the python file and the Makefile. Adjust any filenames in the Makefile
run make SIM=icarus

2. Ensure the design passes the testbench
3. 
