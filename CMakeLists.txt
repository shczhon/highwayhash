cmake_minimum_required(VERSION 3.13)

project(highwayhash_cmake
    DESCRIPTION "Highway Hash"
    LANGUAGES CXX
)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

include(GNUInstallDirs)

add_library(highwayhash "")

target_compile_options(highwayhash
    PRIVATE
    -fno-exceptions
    -Wall
    -Wextra
    -Wno-unused-parameter
    -ffunction-sections
    -fdata-sections
)

if (AVX2)
    target_compile_options(highwayhash
        PRIVATE
        -mavx2
        -DHH_TARGET_NAME=AVX2
    )
else()
    target_compile_options(highwayhash
        PRIVATE
        -DHH_TARGET_NAME=Portable
    )
endif()

target_include_directories(highwayhash
    PUBLIC
    ${CMAKE_CURRENT_LIST_DIR}
)

target_sources(highwayhash
    PRIVATE
    ${CMAKE_CURRENT_LIST_DIR}/highwayhash/sip_hash.h
    ${CMAKE_CURRENT_LIST_DIR}/highwayhash/sip_hash.cc
    ${CMAKE_CURRENT_LIST_DIR}/highwayhash/sip_tree_hash.h
    ${CMAKE_CURRENT_LIST_DIR}/highwayhash/sip_tree_hash.cc
    ${CMAKE_CURRENT_LIST_DIR}/highwayhash/scalar_sip_tree_hash.h
    ${CMAKE_CURRENT_LIST_DIR}/highwayhash/scalar_sip_tree_hash.cc
    ${CMAKE_CURRENT_LIST_DIR}/highwayhash/arch_specific.h
    ${CMAKE_CURRENT_LIST_DIR}/highwayhash/arch_specific.cc
    ${CMAKE_CURRENT_LIST_DIR}/highwayhash/compiler_specific.h
    ${CMAKE_CURRENT_LIST_DIR}/highwayhash/state_helpers.h
    ${CMAKE_CURRENT_LIST_DIR}/highwayhash/endianess.h
    ${CMAKE_CURRENT_LIST_DIR}/highwayhash/vector256.h
)

install(TARGETS highwayhash)

install(
    FILES
    ${CMAKE_CURRENT_LIST_DIR}/highwayhash/sip_hash.h
    ${CMAKE_CURRENT_LIST_DIR}/highwayhash/sip_tree_hash.h
    ${CMAKE_CURRENT_LIST_DIR}/highwayhash/scalar_sip_tree_hash.h
    ${CMAKE_CURRENT_LIST_DIR}/highwayhash/arch_specific.h
    ${CMAKE_CURRENT_LIST_DIR}/highwayhash/compiler_specific.h
    ${CMAKE_CURRENT_LIST_DIR}/highwayhash/state_helpers.h
    ${CMAKE_CURRENT_LIST_DIR}/highwayhash/endianess.h
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/highwayhash
)
