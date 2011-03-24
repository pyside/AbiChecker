project(abi_apiextractor)

find_package(ApiExtractor 0.9.5 REQUIRED)
set(LIBRARY_NAME "apiextractor")

set(ApiExtractor_VERSION "stable")
configure_file("${CMAKE_CURRENT_SOURCE_DIR}/${LIBRARY_NAME}-acc.xml.in"
               "${CMAKE_CURRENT_BINARY_DIR}/${LIBRARY_NAME}-acc.xml" @ONLY)

set(ApiExtractor_VERSION "devel")
set(APIEXTRACTOR_INCLUDE_DIR "${CMAKE_SOURCE_DIR}/reference/include/${LIBRARY_NAME}")
set(APIEXTRACTOR_LIBRARY "${CMAKE_SOURCE_DIR}/reference/lib/${CMAKE_SYSTEM_PROCESSOR}/lib${LIBRARY_NAME}.so")
configure_file("${CMAKE_CURRENT_SOURCE_DIR}/${LIBRARY_NAME}-acc.xml.in"
               "${CMAKE_CURRENT_BINARY_DIR}/${LIBRARY_NAME}-acc-dev.xml" @ONLY)

add_custom_target(abi_apiextractor_check ALL
    COMMAND    ${ABI_CHECKER_TOOL} -l apiextractor -d1 ${CMAKE_CURRENT_BINARY_DIR}/${LIBRARY_NAME}-acc.xml -d2 ${CMAKE_CURRENT_BINARY_DIR}/${LIBRARY_NAME}-acc-dev.xml
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT    "Run ${LIBRARY_NAME} abi checker..."
    SOURCES    ${CMAKE_CURRENT_BINARY_DIR}/${LIBRARY_NAME}-acc.xml ${CMAKE_CURRENT_BINARY_DIR}/${LIBRARY_NAME}-acc-dev.xml
)
   
