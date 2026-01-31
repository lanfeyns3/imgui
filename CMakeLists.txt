cmake_minimum_required(VERSION 3.10)

project(imgui)

add_library(imgui STATIC
    imgui.cpp
    imgui_draw.cpp
    imgui_tables.cpp
    imgui_widgets.cpp
    imgui_demo.cpp

    backends/imgui_impl_glfw.cpp
    backends/imgui_impl_opengl3.cpp
)
