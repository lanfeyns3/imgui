cmake_minimum_required(VERSION 3.10)

project(imgui)

option(IMGUI_USE_VULKAN "Build ImGui with Vulkan backend" OFF)
option(IMGUI_USE_OPENGL "Build ImGui with OpenGL backend" ON)

if(IMGUI_USE_VULKAN AND IMGUI_USE_OPENGL)
    message(FATAL_ERROR "Choose either Vulkan or OpenGL, not both")
endif()

add_library(imgui STATIC
    imgui.cpp
    imgui_draw.cpp
    imgui_tables.cpp
    imgui_widgets.cpp
)

target_include_directories(imgui PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/backends
)

if(IMGUI_USE_OPENGL)
    target_sources(imgui PRIVATE
        backends/imgui_impl_glfw.cpp
        backends/imgui_impl_opengl3.cpp
    )

    target_link_libraries(imgui PUBLIC glfw)

    target_compile_definitions(imgui PUBLIC
        IMGUI_USE_OPENGL
    )
endif()

if(IMGUI_USE_VULKAN)
    target_sources(imgui PRIVATE
        backends/imgui_impl_glfw.cpp
        backends/imgui_impl_vulkan.cpp
    )

    find_package(Vulkan REQUIRED)

    target_link_libraries(imgui PUBLIC
        glfw
        Vulkan::Vulkan
    )

    target_compile_definitions(imgui PUBLIC
        IMGUI_USE_VULKAN
    )
endif()
