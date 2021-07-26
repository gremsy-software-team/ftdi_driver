cmake_minimum_required(VERSION 3.5)

project(kernel_modules)

set(KERNEL_MODULE_LIST
	# hello_world
	ftdi_module
)

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
    PLATFORM STREQUAL s6lm)
	set(KERNEL_ARCH arm64)
else()
	message(FATAL_ERROR "Invalid kernel architecture for modules")
endif()

foreach(KERNEL_MODULE ${KERNEL_MODULE_LIST})
	add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/${KERNEL_MODULE})
endforeach()
