# vk-test

Pre-built binaries for testing vulkan applications in CI environments.

* Vulkan loader library
* Vulkan validation layer
* Swiftshader software driver

See the [vulkan-ci](https://github.com/aloucks/vulkan-ci) action for integration into a github workflow.

## Building

Set the `cmake` host and arch options on **Windows** (assumes a bash shell):

    export CONFIGURE_OPTS="-A x64 -Thost=x64"

### Configure

    mkdir -p target
    cmake $CONFIGURE_OPTS -B target \
        -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX="$(pwd)/target/install" \
        -DSWIFTSHADER_BUILD_EGL=OFF \
        -DSWIFTSHADER_BUILD_GLESv2=OFF \
        -DSWIFTSHADER_BUILD_GLES_CM=OFF \
        -DSWIFTSHADER_BUILD_PVR=OFF \
        -DSWIFTSHADER_GET_PVR=OFF \
        -DSWIFTSHADER_BUILD_TESTS=OFF

### Build

    cmake --build target $BUILD_OPTS --target install