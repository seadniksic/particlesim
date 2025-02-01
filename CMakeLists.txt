cmake_minimum_required(VERSION 3.7...3.27)

if (${CMAKE_VERSION} VERSION_LESS 3.12)
	cmake_policy(VERSION ${CMAKE_MAJOR_VERSION}.${CMAKE_MINOR_VERSION}) # if the version is less than 3.12 then use the policy for the current version.
endif()

set(CMAKE_INSTALL_PREFIX "install" CACHE STRING "Description")

project(particlesim VERSION 1.0 DESCRIPTION "Simulates particles" LANGUAGES CXX)

add_executable(particlesim src/main.cpp)

# --------- ImGUI ------------

add_library(imgui STATIC external/imgui/imconfig.h external/imgui/imgui.cpp external/imgui/imgui.h external/imgui/imgui_demo.cpp external/imgui/imgui_draw.cpp external/imgui/imgui_internal.h external/imgui/imgui_tables.cpp external/imgui/imgui_widgets.cpp external/imgui/imstb_rectpack.h external/imgui/imstb_textedit.h external/imgui/imstb_truetype.h external/imgui/backends/imgui_impl_opengl3.h external/imgui/backends/imgui_impl_opengl3.cpp external/imgui/backends/imgui_impl_glfw.cpp external/imgui/backends/imgui_impl_glfw.h)
target_include_directories(imgui PUBLIC external/imgui external/glfw/include)

# ---------- GLFW ------------

set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)

add_subdirectory(external/glfw)

# ----------- OpenGL ------------

find_package(OpenGL REQUIRED)

message(opengl library location : "${OPENGL_gl_LIBRARY}")
message(found glx : "${OpenGL_GLX_FOUND}")

# -------- Linking Libraries ----------------
target_link_libraries(particlesim PUBLIC glfw imgui GL)
target_include_directories(particlesim PUBLIC external/imgui/backends external/glfw/include)

