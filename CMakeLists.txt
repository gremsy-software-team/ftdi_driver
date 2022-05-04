cmake_minimum_required(VERSION 3.5)

project(kernel_modules)

if (PLATFORM STREQUAL s5l)
    	set(KERNEL_MODULE_LIST ftdi_sio_v4)
else()
	set(KERNEL_MODULE_LIST ftdi_sio_v5)
endif()

# Get kernel version string
if(NOT DEFINED KERNEL_VERSION)
    file(READ ${KERNELHEADERS_DIR}/include/config/kernel.release KERNEL_VERSION)
    string(REGEX REPLACE "\n$" "" KERNEL_VERSION "${KERNEL_VERSION}")
endif()

if(NOT DEFINED FAKEROOT_DIR)
    set(FAKEROOT_DIR ${CMAKE_INSTALL_PREFIX})
endif()

# Set kernel architecture
if (PLATFORM STREQUAL cv22 OR
    PLATFORM STREQUAL s5l OR
    PLATFORM STREQUAL cv25 OR
    PLATFORM STREQUAL s6lm OR
    PLATFORM STREQUAL cv28)
    set(KERNEL_ARCH arm64)
elseif(PLATFORM_FAMILY STREQUAL s2l)
    set(KERNEL_ARCH arm)
else()
    message(FATAL_ERROR "Invalid kernel architecture for modules")
endif()

# Different commands to generate kernel headers on 4.9 vs 5.4 kernels
if (PLATFORM STREQUAL s5l)
   set(KERNEL_GENERATE_COMMANDS silentoldconfig scripts)
   #set(KERNEL_MODULE_LIST ftdi_sio_kernel_v4)
else()
   set(KERNEL_GENERATE_COMMANDS prepare)
   #set(KERNEL_MODULE_LIST ftdi_sio_kernel_v5)
endif()

include (ExternalProject)

# We need to generate kernel headers before we can build kernel modules
ExternalProject_Add(kernel_generated_headers
	SOURCE_DIR "${KERNELHEADERS_DIR}"
	CONFIGURE_COMMAND ""
	BUILD_COMMAND $(MAKE) -C ${KERNELHEADERS_DIR} ${KERNEL_GENERATE_COMMANDS} CROSS_COMPILE=${TOOLCHAIN_DIR}/usr/bin/${TOOLCHAIN_NAME}/${TOOLCHAIN_NAME}- ARCH=${KERNEL_ARCH}
        INSTALL_COMMAND ""
)

foreach(KERNEL_MODULE ${KERNEL_MODULE_LIST})
    add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/${KERNEL_MODULE})
endforeach()
