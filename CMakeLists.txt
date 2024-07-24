cmake_minimum_required(VERSION 3.28)
project(
        LearnWebGPU # name of the project, which will also be the name of the visual studio solution if you use it
        VERSION 0.1.0 # any version number
        LANGUAGES CXX C # programming languages used by the project
)
set(CMAKE_CXX_STANDARD 23)

add_subdirectory(vendor/webgpu)

add_executable(App main.cpp)

target_link_libraries(App PRIVATE webgpu)
target_copy_webgpu_binaries(App)

set_target_properties(App PROPERTIES
        CXX_STANDARD 23
        CXX_STANDARD_REQUIRED ON
        CXX_EXTENSIONS OFF
        COMPILE_WARNING_AS_ERROR ON
)

if (MSVC)
    target_compile_options(App PRIVATE /W4)
else()
    target_compile_options(App PRIVATE -Wall -Wextra -pedantic)
endif()

# Options that are specific to Emscripten
if (EMSCRIPTEN)
    # Generate a full web page rather than a simple WebAssembly module
    set_target_properties(App PROPERTIES SUFFIX ".html")
endif()