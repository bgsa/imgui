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

set (OUTPUT_LIB_DIR ${CMAKE_CURRENT_LIST_DIR}/../lib)
set (CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${OUTPUT_LIB_DIR})

if ( ${OPERATING_SYSTEM} STREQUAL "WINDOWS" )
	set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /MDd")
	set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /MD")
endif()

#file(GLOB SOURCES "../imgui*.cpp")

INCLUDE_DIRECTORIES(
	../glfw/include
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

add_library(imgui
                ../imgui.cpp
                ../imgui_draw.cpp
                ../imgui_widgets.cpp
                ../imgui_impl_glfw.cpp
                ../imgui_impl_opengl2.cpp
)

target_link_libraries(imgui
    ${OPENGL_LIBRARY}
	${GLEW_LIBRARIES}
)

if ( ${OPERATING_SYSTEM} STREQUAL "WINDOWS" )
	target_link_libraries(imgui
		${OUTPUT_LIB_DIR}/glfw3.lib
	)
endif()

if ( ${OPERATING_SYSTEM} STREQUAL "LINUX" )
	target_link_libraries(imgui
		glfw
	)
endif()


target_compile_definitions(imgui 
    PUBLIC IMGUI_IMPL_OPENGL_LOADER_GLEW
)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)

message("Output copied to ${OUTPUT_LIB_DIR}")
