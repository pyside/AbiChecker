project(abi_check)

cmake_minimum_required(VERSION 2.6)

find_package(Qt4 4.5.0 REQUIRED)
set(ABI_CHECKER_TOOL ${CMAKE_CURRENT_SOURCE_DIR}/tools/abi-compliance-checker)

add_subdirectory(apiextractor)
add_subdirectory(generatorrunner)
add_subdirectory(shiboken)
add_subdirectory(pyside)

message("CMAKE_SYSTEM_PROCESSOR: ${CMAKE_SYSTEM_PROCESSOR}")
