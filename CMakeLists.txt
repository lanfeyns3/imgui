cmake_minimum_required(VERSION 3.10)

project(imgui)

option(IMGUI_ENABLE_OPENGL3 "Enable OpenGL3 rendering backend" OFF)
option(IMGUI_ENABLE_VULKAN  "Enable Vulkan rendering backend" OFF)

set(IMGUI_SOURCES
    imgui.cpp
    imgui_draw.cpp
    imgui_tables.cpp
    imgui_widgets.cpp
    misc/cpp/imgui_stdlib.cpp
    imgui_demo.cpp
    backends/imgui_impl_glfw.cpp
)

set(BACKEND_LIBS glfw)

if(IMGUI_ENABLE_OPENGL3)
    message(STATUS "ImGui: Enabling OpenGL3 backend")
    list(APPEND IMGUI_SOURCES backends/imgui_impl_opengl3.cpp)
    
    find_package(OpenGL REQUIRED)
    list(APPEND BACKEND_LIBS ${OPENGL_LIBRARIES})
endif()

if(IMGUI_ENABLE_VULKAN)
    message(STATUS "ImGui: Enabling Vulkan backend")
    list(APPEND IMGUI_SOURCES backends/imgui_impl_vulkan.cpp)
    
    find_package(Vulkan REQUIRED)
    list(APPEND BACKEND_LIBS Vulkan::Vulkan)
endif()

add_library(imgui STATIC ${IMGUI_SOURCES})

target_include_directories(imgui PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/backends
)

target_link_libraries(imgui PUBLIC ${BACKEND_LIBS})

target_compile_definitions(imgui PUBLIC IMGUI_ENABLE_GLFW)
