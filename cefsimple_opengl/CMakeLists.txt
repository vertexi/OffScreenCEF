cmake_minimum_required(VERSION 3.24)

# Logical target used to link the libcef library.
ADD_LOGICAL_TARGET("libcef_lib" "${CEF_LIB_DEBUG}" "${CEF_LIB_RELEASE}")

# Determine the target output directory.
SET_CEF_TARGET_OUT_DIR()

#include the subdirectory containing our libs
include_directories(${_CEF_ROOT})

file(GLOB sources *.hpp *.cpp)
add_executable(cefsimple_opengl ${sources})

find_package(GLEW REQUIRED)
target_link_libraries(cefsimple_opengl GLEW::GLEW)

find_package(glfw3 CONFIG REQUIRED)
target_link_libraries(cefsimple_opengl glfw)

find_package(glm CONFIG REQUIRED)
target_link_libraries(cefsimple_opengl glm::glm)


SET_EXECUTABLE_TARGET_PROPERTIES(cefsimple_opengl)

source_group("src" FILES ${sources})
add_dependencies(cefsimple_opengl libcef_dll_wrapper)

# Indicate which libraries to include during the link process.
target_link_libraries (cefsimple_opengl d3d11.lib Shlwapi.lib libcef_lib libcef_dll_wrapper)

target_include_directories(cefsimple_opengl PUBLIC ${CMAKE_CURRENT_LIST_DIR} ${CMAKE_CURRENT_LIST_DIR}/../thirdparty/cef/include)

COPY_FILES(cefsimple_opengl "${CEF_BINARY_FILES}" "${CEF_BINARY_DIR}" "${TARGET_OUTPUT_DIRECTORY}")
COPY_FILES(cefsimple_opengl "${CEF_RESOURCE_FILES}" "${CEF_RESOURCE_DIR}" "${TARGET_OUTPUT_DIRECTORY}")


if(MSVC)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
    string(REGEX REPLACE "/MDd" "/MTd" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
endif(MSVC)
