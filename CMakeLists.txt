#################################################################################
# Copyright (c) 2021, Savely Pototsky (SavaLione)
# All rights reserved.
#################################################################################
# Checking the minimum version
cmake_minimum_required (VERSION 3.13.1)

# Project name
set(IMPLOT_DEMO_PROJECT "implot_demo")
set(IMPLOT_DEMO_PROJECT_VERSION "0.0.1")

project(${IMPLOT_DEMO_PROJECT}
    VERSION ${IMPLOT_DEMO_PROJECT_VERSION}
    LANGUAGES CXX
)

# flags
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2")               # default
#set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2 -g")           # debug

#################################################################################
#                                    Libraries                                  #
#################################################################################

# OpenGL
find_package(OpenGL REQUIRED)

# GLFW
set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
set(BUILD_SHARED_LIBS ON CACHE BOOL "" FORCE)

set(GLFW_INCLUDE_DIR "${PROJECT_SOURCE_DIR}/lib/glfw/include" CACHE PATH "GLFW lib include")
add_subdirectory(${PROJECT_SOURCE_DIR}/lib/glfw)

# imgui
set(IMGUI_BUILD_BACKEND_OPENGL3 ON CACHE BOOL "" FORCE)
set(IMGUI_BUILD_BACKEND_GLFW ON CACHE BOOL "" FORCE)

set(IMGUI_INCLUDE_DIR "${PROJECT_SOURCE_DIR}/lib/imgui/include" CACHE PATH "imgui lib include")
add_subdirectory(${PROJECT_SOURCE_DIR}/lib/imgui)

# implot
set(IMPLOT_INCLUDE_DIR "${PROJECT_SOURCE_DIR}/lib/implot/include" CACHE PATH "implot lib include")
add_subdirectory(${PROJECT_SOURCE_DIR}/lib/implot)

#################################################################################
#                                     Sources                                   #
#################################################################################

set(IMPLOT_DEMO_SRC
    src/implot_demo.cpp
)

# implot demo executable
add_executable(${IMPLOT_DEMO_PROJECT} ${IMPLOT_DEMO_SRC})
target_include_directories(${IMPLOT_DEMO_PROJECT} PUBLIC ${PROJECT_SOURCE_DIR}/src)

#################################################################################
#                                Libraries include                              #
#################################################################################

# OpenGL
target_link_libraries(${IMPLOT_DEMO_PROJECT} ${OPENGL_gl_LIBRARY})
target_include_directories(${IMPLOT_DEMO_PROJECT} PUBLIC ${OPENGL_INCLUDE_DIR})

# GLFW
target_link_libraries(${IMPLOT_DEMO_PROJECT} glfw)
target_include_directories(${IMPLOT_DEMO_PROJECT} PUBLIC ${GLFW_INCLUDE_DIR})

# imgui
target_link_libraries(${IMPLOT_DEMO_PROJECT} imgui imgui_glfw imgui_opengl3)
target_include_directories(${IMPLOT_DEMO_PROJECT} PUBLIC ${IMGUI_INCLUDE_DIR})

# implot
target_link_libraries(${IMPLOT_DEMO_PROJECT} implot)
target_include_directories(${IMPLOT_DEMO_PROJECT} PUBLIC ${IMPLOT_INCLUDE_DIR})

#################################################################################
#                                 Output directory                              #
#################################################################################

# implot demo
set_target_properties(${IMPLOT_DEMO_PROJECT}
    PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin" # lib
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin" # lib
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin"
)