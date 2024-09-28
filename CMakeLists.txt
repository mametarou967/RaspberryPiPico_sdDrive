cmake_minimum_required(VERSION 3.12)

include(pico_sdk_import.cmake)

include(pico_extras_import_optional.cmake)

project(RaspberryPiPico_sdDrive C CXX ASM)
set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

if (PICO_SDK_VERSION_STRING VERSION_LESS "1.3.0")
    message(FATAL_ERROR "Raspberry Pi Pico SDK version 1.3.0 (or later) required. Your version is ${PICO_SDK_VERSION_STRING}")
endif()

pico_sdk_init()

if (TARGET tinyusb_device)
	add_executable(RaspberryPiPico_sdDrive
	        sdDrive.c
			lib/no-OS-FatFS-SD-SPI-RPi-Pico/FatFs_SPI/sd_driver/hw_config.c
	        )
	add_subdirectory(lib/no-OS-FatFS-SD-SPI-RPi-Pico/FatFs_SPI build)
	
	target_link_libraries(RaspberryPiPico_sdDrive pico_stdlib FatFs_SPI)
	
	# enable usb output, disable uart output
	pico_enable_stdio_usb(RaspberryPiPico_sdDrive 1)
	pico_enable_stdio_uart(RaspberryPiPico_sdDrive 0)
	
	pico_add_extra_outputs(RaspberryPiPico_sdDrive)
elseif(PICO_ON_DEVICE)
    message(WARNING "not building hello_usb because TinyUSB submodule is not initialized in the SDK")
endif()
