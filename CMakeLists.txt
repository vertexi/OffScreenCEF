cmake_minimum_required(VERSION 3.24)

set(CEF_ROOT ${CMAKE_CURRENT_LIST_DIR}/thirdparty/cef)

add_definitions(-DOS_WIN)

set(CMAKE_CONFIGURATION_TYPES Debug Release)

project(cefsimple VERSION 0.0)
set_property(GLOBAL PROPERTY OS_FOLDERS ON)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake")


find_package(CEF REQUIRED)
if(MSVC)
	add_definitions(-DUNICODE -D_UNICODE)
endif()


set(
    CMAKE_RUNTIME_OUTPUT_DIRECTORY
    ${CMAKE_HOME_DIRECTORY}/bin
    )
	
set(
    CMAKE_LIBRARY_OUTPUT_DIRECTORY
    ${CMAKE_HOME_DIRECTORY}/lib
    )
	
set(
    TARGET_OUTPUT_DIRECTORY
    ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/$<CONFIGURATION>
    )

add_subdirectory(${CEF_LIBCEF_DLL_WRAPPER_PATH} libcef_dll_wrapper)

add_subdirectory(cefsimple_opengl)

# Display configuration settings.
PRINT_CEF_CONFIG()

