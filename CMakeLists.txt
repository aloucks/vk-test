cmake_minimum_required(VERSION 3.14)

project(LANGUAGES C CXX ASM)

if (NOT EXISTS glslang/External/spirv-tools)
    execute_process(COMMAND python update_glslang_sources.py WORKING_DIRECTORY glslang RESULT_VARIABLE update_glslang_ret)
    if (NOT ${update_glslang_ret} EQUAL 0)
        message(FATAL_ERROR "Could not update glslang sources. Is python installed?")
    endif()
endif()

add_subdirectory("Vulkan-Headers")
add_subdirectory("Vulkan-Loader")
add_subdirectory("glslang")
add_subdirectory("swiftshader")
add_subdirectory("Vulkan-ValidationLayers")

# Enables the vulkan loader to have access to environment variables
# when checking for ICD and layer file paths. This may be needed if
# the process is executing in a container with elevated permissions.
target_compile_options(vulkan PUBLIC -DUSE_UNSAFE_FILE_SEARCH)
if (WIN32)
    target_compile_options(loader-norm PUBLIC -DUSE_UNSAFE_FILE_SEARCH)
endif()
