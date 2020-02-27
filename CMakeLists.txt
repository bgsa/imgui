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
endif()

set (CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_LIST_DIR}/lib/)

#file(GLOB SOURCES "../imgui*.cpp")

INCLUDE_DIRECTORIES(
	/usr/include/
    /usr/local/include/
)

link_directories(
	/lib
	/usr/lib
	/usr/local/lib/
	/usr/lib/x86_64-linux-gnu/
)

add_library(imgui
                ../imgui.cpp
                ../imgui_draw.cpp
                ../imgui_widgets.cpp
                ../imgui_impl_glfw.cpp
                ../imgui_impl_opengl2.cpp
)

target_link_libraries(imgui
    ${OPENGL_LIBRARY}
    glfw
    ${GLEW_LIBRARIES}
)

target_compile_definitions(imgui 
    PUBLIC IMGUI_IMPL_OPENGL_LOADER_GLEW
)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)

message("Output copied to ${CMAKE_ARCHIVE_OUTPUT_DIRECTORY}")
