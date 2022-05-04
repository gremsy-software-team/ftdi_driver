# EXTRA_CFLAGS		+=
# EXTRA_AFLAGS		+=
# EXTRA_LDFLAGS		+=
# EXTRA_ARFLAGS		+=

obj-m += hello_world.o

KDIR := $(KERNEL_HEADERS_DIR)
PWD := $(shell pwd)

all:
	make -C $(KERNEL_HEADERS_DIR) M=$(PWD) modules

clean:
	make -C $(KERNEL_HEADERS_DIR) M=$(PWD) clean

