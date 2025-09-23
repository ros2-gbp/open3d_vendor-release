cmake_minimum_required(VERSION 3.22)
project(open3d)

if(POLICY CMP0135)
  cmake_policy(SET CMP0135 NEW)
endif()

# https://github.com/isl-org/Open3D/releases
set(O3D_VERSION   0.19.0)
set(O3D_URL_MD5   08f3aee5211cbc1b2c96c66c8a82e08b)

set(O3D_URL       https://github.com/isl-org/Open3D/releases/download/v${O3D_VERSION}/open3d-devel-linux-x86_64-cxx11-abi-cuda-${O3D_VERSION}.tar.xz)

include(ExternalProject)
ExternalProject_Add(o3d_archive
  URL               ${O3D_URL}
  URL_MD5           ${O3D_URL_MD5}
  PREFIX            o3d_archive
  CONFIGURE_COMMAND ""
  BUILD_COMMAND     ""
  INSTALL_COMMAND   ""
)

ExternalProject_Get_Property(o3d_archive SOURCE_DIR)
set(O3D_PATH ${SOURCE_DIR})

# ignore "gmock" and "gtest"
# ignore "open3d_tf_ops.so" due to missing "libtensorflow_framework.so.2"
install(DIRECTORY ${O3D_PATH}/
    DESTINATION .
    PATTERN "include/gmock" EXCLUDE
    PATTERN "include/gtest" EXCLUDE
    PATTERN "*gmock*.*" EXCLUDE
    PATTERN "*gtest*.*" EXCLUDE
    PATTERN "GTest" EXCLUDE
    PATTERN "*open3d_tf_ops.*" EXCLUDE
    PATTERN "Open3DTFOps*.cmake" EXCLUDE
)
