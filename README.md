# vk-test

Pre-built binaries for testing vulkan applications in CI environments.

* Vulkan loader library
* Vulkan validation layer
* Swiftshader software driver

See the [vulkan-ci](https://github.com/aloucks/vulkan-ci) action for integration into a github workflow.

## Building

Update submodules:

    git submodule update --init --recursive --depth 1

#### Windows

Set the `cmake` host and arch options on **Windows** (assumes a `bash` shell):

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

### Test 

Requires Rust to be installed.

#### Windows

Assumes a `bash` shell.

    PATH=target/install/bin:$PATH \
    VK_LOADER_DEBUG=all \
    VK_ICD_FILENAMES=$(cygpath -w $(pwd))\\target\\Windows\\vk_swiftshader_icd.json \
    VK_LAYER_PATH=$(pwd)\\target\\install\\lib \
    SWIFTSHADER_DISABLE_DEBUGGER_WAIT_DIALOG=1 \
    cargo test --manifest-path=test/Cargo.toml -- --nocapture

#### Linux

    LD_LIBRARY_PATH=target/install/lib:$PATH \
    VK_LOADER_DEBUG=all \
    VK_ICD_FILENAMES=$(pwd)/target/Linux/vk_swiftshader_icd.json \
    VK_LAYER_PATH=$(pwd)/target/Vulkan-ValidationLayers/layers \
    SWIFTSHADER_DISABLE_DEBUGGER_WAIT_DIALOG=1 \
    cargo test --manifest-path=test/Cargo.toml -- --nocapture

#### MacOS

    DYLD_LIBRARY_PATH=target/install/lib:$PATH \
    VK_LOADER_DEBUG=all \
    VK_ICD_FILENAMES=$(pwd)/target/Darwin/vk_swiftshader_icd.json \
    VK_LAYER_PATH=$(pwd)/target/Vulkan-ValidationLayers/layers \
    SWIFTSHADER_DISABLE_DEBUGGER_WAIT_DIALOG=1 \
    cargo test --manifest-path=test/Cargo.toml -- --nocapture

> **NOTE**: The `DYLD_LIBRARY_PATH` environment variable may [not be work](https://developer.apple.com/library/archive/documentation/Security/Conceptual/System_Integrity_Protection_Guide/RuntimeProtections/RuntimeProtections.html).
See the `Test (MacOS)` section of the [build](.github/workflows/build.yml) workflow for an alternative.