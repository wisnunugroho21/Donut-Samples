#
# Copyright (c) 2014-2024, NVIDIA CORPORATION. All rights reserved.
#
# Permission is hereby granted, free of charge, to any person obtaining a
# copy of this software and associated documentation files (the "Software"),
# to deal in the Software without restriction, including without limitation
# the rights to use, copy, modify, merge, publish, distribute, sublicense,
# and/or sell copies of the Software, and to permit persons to whom the
# Software is furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in
# all copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.  IN NO EVENT SHALL
# THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
# FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER
# DEALINGS IN THE SOFTWARE.

include(../../donut/compileshaders.cmake)
file(GLOB shaders "*.hlsl")
file(GLOB sources "*.cpp" "*.h")

set(project work_graphs_d3d12)
set(folder "Examples/Work Graphs")

donut_compile_shaders(
    TARGET ${project}_shaders
    PROJECT_NAME "Work Graphs"
    CONFIG ${CMAKE_CURRENT_SOURCE_DIR}/shaders.cfg
    FOLDER ${folder}
    DXIL ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}/shaders/${project}/dxil
    SOURCES ${shaders}
    SHADER_MODEL 6_8
)

add_executable(${project} WIN32 ${sources})
target_include_directories(${project} PRIVATE "${DONUT_D3D_AGILITY_SDK_PATH}/build/native/include")
target_link_libraries(${project} donut_app donut_engine)
add_dependencies(${project} ${project}_shaders)
set_target_properties(${project} PROPERTIES FOLDER ${folder})

if (MSVC)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W3 /MP")
endif()