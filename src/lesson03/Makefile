CHAINPREFIX ?= aarch64-linux-gnu-

COPS = -Wall -nostdlib -fno-stack-protector -nostartfiles -ffreestanding -Iinclude -mgeneral-regs-only -g -O0 -DUSE_QEMU
ASMOPS = -Iinclude -g -DUSE_QEMU

BUILD_DIR = build
SRC_DIR = src

all: kernel8.img

clean:
	rm -rf $(BUILD_DIR) *.img

$(BUILD_DIR)/%_c.o: $(SRC_DIR)/%.c
	mkdir -p $(@D)
	$(CHAINPREFIX)gcc $(COPS) -MMD -c $< -o $@

$(BUILD_DIR)/%_s.o: $(SRC_DIR)/%.S
	$(CHAINPREFIX)gcc $(ASMOPS) -MMD -c $< -o $@

C_FILES = $(wildcard $(SRC_DIR)/*.c)
ASM_FILES = $(wildcard $(SRC_DIR)/*.S)
OBJ_FILES = $(C_FILES:$(SRC_DIR)/%.c=$(BUILD_DIR)/%_c.o)
OBJ_FILES += $(ASM_FILES:$(SRC_DIR)/%.S=$(BUILD_DIR)/%_s.o)

DEP_FILES = $(OBJ_FILES:%.o=%.d)
-include $(DEP_FILES)

kernel8.img: $(SRC_DIR)/linker-qemu.ld $(OBJ_FILES)
	$(CHAINPREFIX)ld -T $(SRC_DIR)/linker-qemu.ld -o $(BUILD_DIR)/kernel8.elf  $(OBJ_FILES)
	$(CHAINPREFIX)objcopy $(BUILD_DIR)/kernel8.elf -O binary kernel8.img
