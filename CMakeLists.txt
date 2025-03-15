
cmake_minimum_required(VERSION 3.13)

set(PICO_BOARD pico_w CACHE STRING "Board type")

if(DEFINED PICO_SDK_PATH)
    include("${PICO_SDK_PATH}/external/pico_sdk_import.cmake")
elseif(DEFINED ENV{PICO_SDK_PATH})
    include("$ENV{PICO_SDK_PATH}/external/pico_sdk_import.cmake")
else()
    message(FATAL_ERROR "PICO_SDK_PATH is not set. Provide it as an environment variable or a CMake variable.")
endif()


# if(DEFINED PICO_EXTRAS_PATH)
#     include("pico_extras_import.cmake")
# else
if(DEFINED ENV{PICO_EXTRAS_PATH})
    include("$ENV{PICO_EXTRAS_PATH}/external/pico_extras_import.cmake")
else()
    message(FATAL_ERROR "PICO_EXTRAS_PATH is not set. Provide it as an environment variable or a CMake variable.")
endif()

# Include the Pico SDK
include(${PICO_SDK_PATH}/pico_sdk_init.cmake)


# Specify the target platform explicitly
set(PICO_PLATFORM "rp2040")

if (PICO_SDK_VERSION_STRING VERSION_LESS "1.4.0")
  message(FATAL_ERROR "Raspberry Pi Pico SDK version 1.4.0 (or later) required. Your version is ${PICO_SDK_VERSION_STRING}")
endif()

# project name
project(demo C CXX ASM)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# Initialise the Raspberry Pi Pico SDK
pico_sdk_init()

#need to add additional .c file if any
add_executable( demo

main.c

)

#pico_enable_stdio_uart(demo 0)
pico_enable_stdio_usb(demo 1) #usb to printf logs

# Add the standard include files to the build
target_include_directories(demo PRIVATE
  ${CMAKE_CURRENT_LIST_DIR}
  #littlefs-lib INTERFACE 
  ${CMAKE_CURRENT_LIST_DIR}
 # ${CMAKE_CURRENT_LIST_DIR}/.. # for our common lwipopts or any other standard includes, if required
)

#keep adding dependencies as per the project needs
target_link_libraries ( demo
    pico_stdlib
    pico_stdio
)

pico_add_extra_outputs(demo)
