project(abi_check)

cmake_minimum_required(VERSION 2.6)

find_package(Qt4 4.5.0 REQUIRED)
set(ABI_CHECKER_TOOL ${CMAKE_CURRENT_SOURCE_DIR}/tools/abi-compliance-checker)
set(ABI_CHECKER_RESULT_TOOL ${CMAKE_CURRENT_SOURCE_DIR}/tools/result_checker)
add_custom_target(result_check)

add_subdirectory(apiextractor)
add_subdirectory(generatorrunner)
add_subdirectory(shiboken)
add_subdirectory(pyside)
