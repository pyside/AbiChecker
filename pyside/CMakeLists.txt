project(abi_shiboken)

find_package(ApiExtractor 0.9.5 REQUIRED)
find_package(GeneratorRunner 0.6.7 REQUIRED)
find_package(Shiboken 1.0.0 REQUIRED)
find_package(PySide 1.0.0 REQUIRED)
find_package(PythonLibs)

set(LIBRARY_NAME "pyside")

configure_file("${CMAKE_CURRENT_SOURCE_DIR}/${LIBRARY_NAME}-acc.xml.in"
               "${CMAKE_CURRENT_BINARY_DIR}/${LIBRARY_NAME}-acc.xml" @ONLY)

set(PySide_VERSION "${PySide_VERSION}-dev")
set(PYSIDE_INCLUDE_DIR "${CMAKE_SOURCE_DIR}/reference/include/${LIBRARY_NAME}")
set(PYSIDE_LIBRARY "${CMAKE_SOURCE_DIR}/reference/lib/${CMAKE_SYSTEM_PROCESSOR}/lib${LIBRARY_NAME}-${SHIBOKEN_PYTHON_BASENAME}.so")
configure_file("${CMAKE_CURRENT_SOURCE_DIR}/${LIBRARY_NAME}-acc.xml.in"
               "${CMAKE_CURRENT_BINARY_DIR}/${LIBRARY_NAME}-acc-dev.xml" @ONLY)

add_custom_target(abi_${LIBRARY_NAME}_check ALL
    COMMAND    ${ABI_CHECKER_TOOL} -l ${LIBRARY_NAME} -d1 ${CMAKE_CURRENT_BINARY_DIR}/${LIBRARY_NAME}-acc.xml -d2 ${CMAKE_CURRENT_BINARY_DIR}/${LIBRARY_NAME}-acc-dev.xml
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT    "Run ${LIBRARY_NAME} abi checker..."
    SOURCES    ${CMAKE_CURRENT_BINARY_DIR}/${LIBRARY_NAME}-acc.xml ${CMAKE_CURRENT_BINARY_DIR}/${LIBRARY_NAME}-acc-dev.xml
)

