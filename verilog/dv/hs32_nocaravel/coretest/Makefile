RTL_PATH = ../../../rtl
VERILOG_PATH = ../../../

.SUFFIXES:

PATTERN = core

all: ${PATTERN:=.vcd}

%.vvp: %_tb.v
	iverilog -DSIM -DFUNCTIONAL -D$(TEST) -I $(RTL_PATH) -I $(RTL_PATH)/hs32cpu \
	$< -o $@

%.vcd: %.vvp
	vvp $< -fst-speed $(OPT)

clean:
	rm -f *.elf *.hex *.bin *.vvp *.vcd *.log *.idx *.fst

.PHONY: clean all
