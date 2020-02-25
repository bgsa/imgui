cmake_minimum_required(VERSION 3.0.0)
project(imgui VERSION 1.0.0)

find_package(OpenGL REQUIRED)
find_package(imgui  REQUIRED)

if (UNIX)
    find_package(PkgConfig REQUIRED)
    pkg_search_module(GLFW REQUIRED glfw3)
endif()

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_LIST_DIR}/lib/)

INCLUDE_DIRECTORIES(
    /usr/local/include/
    ./../
)

link_directories(/usr/local/lib/)

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
