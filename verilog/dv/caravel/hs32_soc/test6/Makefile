FIRMWARE_PATH = ../..
RTL_PATH = ../../../../rtl
IP_PATH = ../../../../ip
BEHAVIOURAL_MODELS = ../../ 

GCC_PATH?=/opt/riscv32i/bin
GCC_PREFIX?=riscv32-unknown-elf
PDK_PATH?=$(PDK_ROOT)/sky130A

.SUFFIXES:

PATTERN = test6

all:  ${PATTERN:=.vcd}

hex:  ${PATTERN:=.hex}

%.vvp: %_tb.v %.hex
	iverilog -DSIM -DFUNCTIONAL -I $(BEHAVIOURAL_MODELS) \
	-I $(PDK_PATH) -I $(IP_PATH) -I $(RTL_PATH) -I $(RTL_PATH)/hs32cpu \
	-o $@ $<

%.vcd: %.vvp
	vvp $<

%.elf: %.c $(FIRMWARE_PATH)/sections.lds $(FIRMWARE_PATH)/start.s
	${GCC_PATH}/${GCC_PREFIX}-gcc -march=rv32imc -mabi=ilp32 -Wl,-Bstatic,-T,$(FIRMWARE_PATH)/sections.lds,--strip-debug -ffreestanding -nostdlib -o $@ $(FIRMWARE_PATH)/start.s $<

%.hex: %.elf
	${GCC_PATH}/${GCC_PREFIX}-objcopy -O verilog $< $@ 
	# to fix flash base address
	sed -i 's/@10000000/@00000000/g' $@

%.bin: %.elf
	${GCC_PATH}/${GCC_PREFIX}-objcopy -O binary $< /dev/stdout | tail -c +1048577 > $@

# ---- Clean ----

clean:
	rm -f *.elf *.hex *.bin *.vvp *.vcd *.log *.idx

.PHONY: clean hex all
