# CMakeList.txt : CMake project for drum_machine, include source and define
# project specific logic here.
#
cmake_minimum_required (VERSION 3.8)

# Enable Hot Reload for MSVC compilers if supported.
if (POLICY CMP0141)
  cmake_policy(SET CMP0141 NEW)
  set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT "$<IF:$<AND:$<C_COMPILER_ID:MSVC>,$<CXX_COMPILER_ID:MSVC>>,$<$<CONFIG:Debug,RelWithDebInfo>:EditAndContinue>,$<$<CONFIG:Debug,RelWithDebInfo>:ProgramDatabase>>")
endif()

project ("drum_machine")

find_package(OpenGL REQUIRED)
find_package(glfw3 CONFIG REQUIRED)
find_package(glm CONFIG REQUIRED)
find_package(GLEW REQUIRED)

set(IMGUI_DIR ./external/imgui)

add_library(imgui STATIC
    ${IMGUI_DIR}/imgui.cpp
    ${IMGUI_DIR}/imgui_demo.cpp
    ${IMGUI_DIR}/imgui_draw.cpp
    ${IMGUI_DIR}/imgui_tables.cpp
    ${IMGUI_DIR}/imgui_widgets.cpp
    ${IMGUI_DIR}/backends/imgui_impl_glfw.cpp
    ${IMGUI_DIR}/backends/imgui_impl_opengl3.cpp
)

target_include_directories(imgui PUBLIC
    ${IMGUI_DIR}
    ${IMGUI_DIR}/backends
)

target_link_libraries(imgui PRIVATE
    OpenGL::GL
    glfw
)

target_compile_definitions(imgui PUBLIC
    IMGUI_IMPL_OPENGL_LOADER_GLAD
)

# Add source to this project's executable.
add_executable (drum_machine "src/main.cpp")

target_link_libraries(drum_machine PRIVATE
    imgui
    glfw
    OpenGL::GL
    glm::glm
    GLEW::GLEW
)

if (CMAKE_VERSION VERSION_GREATER 3.12)
  set_property(TARGET drum_machine PROPERTY CXX_STANDARD 20)
endif()

