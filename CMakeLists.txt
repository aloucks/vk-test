cmake_minimum_required(VERSION 3.14)

project(LANGUAGES C CXX ASM)

if (NOT EXISTS glslang/External/spirv-tools)
    execute_process(COMMAND python update_glslang_sources.py WORKING_DIRECTORY glslang)
endif()

add_subdirectory("Vulkan-Headers")
add_subdirectory("Vulkan-Loader")
add_subdirectory("glslang")
add_subdirectory("swiftshader")
add_subdirectory("Vulkan-ValidationLayers")

# Enables the vulkan loader to always have access to environment variables
# when checking for ICD and layer file paths. This is needed when executing
# in a github runner, where the process may have elevated permissions.
target_compile_options(vulkan PUBLIC -DUSE_UNSAFE_FILE_SEARCH)
if (WIN32)
    target_compile_options(loader-norm PUBLIC -DUSE_UNSAFE_FILE_SEARCH)
endif()
