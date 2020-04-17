cmake_minimum_required(VERSION 3.0.0)
project(imgui VERSION 1.0.0)

set (CMAKE_C_STANDARD 11)
set (CMAKE_C_STANDARD_REQUIRED ON)
set (CMAKE_CXX_STANDARD 11)
set (CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(OpenGL REQUIRED)

if ( ${OPERATING_SYSTEM} STREQUAL "LINUX" )
    find_package(PkgConfig REQUIRED)
    pkg_search_module(GLFW REQUIRED glfw3)

    find_package(GLEW REQUIRED)
endif()

set (CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${OUTPUT_LIB_DIR})

if ( ${OPERATING_SYSTEM} STREQUAL "WINDOWS" )
	set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /MDd")
	set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /MD")
endif()

if ( ${OPERATING_SYSTEM} STREQUAL "WINDOWS" )
	set (GLFW_INCLUDE_DIRS ../glfw/include)
	set (GLFW_LIBRARIES    ${OUTPUT_LIB_DIR}/Debug/glfw3.lib)
	set (GLEW_LIBRARIES    ${OUTPUT_LIB_DIR}/../glew32d.lib)
elseif ( ${OPERATING_SYSTEM} STREQUAL "LINUX" )
elseif ( ${OPERATING_SYSTEM} STREQUAL "UNIX" )
else()
	message(FATAL_ERROR "Operating System not defined")
endif()

include_directories(
	../include
	${GLFW_INCLUDE_DIRS}
	${GLEW_INCLUDE_DIRS}
	/usr/include
	/usr/local/include
)

link_directories(
	${OUTPUT_LIB_DIR}
	/lib
	/usr/lib
	/usr/local/lib/
	/usr/lib/x86_64-linux-gnu/
)

add_library(imgui STATIC
	../imgui.cpp
	../imgui_draw.cpp
	../imgui_widgets.cpp
	../imgui_impl_glfw.cpp
	../imgui_impl_opengl3.cpp
)

target_compile_definitions(imgui 
	PUBLIC GLEW_STATIC
	PUBLIC IMGUI_IMPL_OPENGL_LOADER_GLEW
)

target_link_libraries(imgui
	${OPENGL_LIBRARY}
	${GLEW_LIBRARIES}
	${GLFW_LIBRARIES}
)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)

message("Output copied to ${OUTPUT_LIB_DIR}")
