project(abi_generatorrunner)

find_package(ApiExtractor 0.9.5 REQUIRED)
find_package(GeneratorRunner 0.6.7 REQUIRED)
set(LIBRARY_NAME "generatorrunner")

set(GeneratorRunner_VERSION "stable")
configure_file("${CMAKE_CURRENT_SOURCE_DIR}/${LIBRARY_NAME}-acc.xml.in"
               "${CMAKE_CURRENT_BINARY_DIR}/${LIBRARY_NAME}-acc.xml" @ONLY)

set(GeneratorRunner_VERSION "devel")
set(GENERATORRUNNER_INCLUDE_DIR "${CMAKE_SOURCE_DIR}/reference/include/${LIBRARY_NAME}")
set(GENERATORRUNNER_LIBRARY "${CMAKE_SOURCE_DIR}/reference/lib/${CMAKE_SYSTEM_PROCESSOR}/libgenrunner.so")
configure_file("${CMAKE_CURRENT_SOURCE_DIR}/${LIBRARY_NAME}-acc.xml.in"
               "${CMAKE_CURRENT_BINARY_DIR}/${LIBRARY_NAME}-acc-dev.xml" @ONLY)

add_custom_target(abi_generatorrunner_check ALL
    COMMAND    ${ABI_CHECKER_TOOL} -l genrunner -d1 ${CMAKE_CURRENT_BINARY_DIR}/${LIBRARY_NAME}-acc.xml -d2 ${CMAKE_CURRENT_BINARY_DIR}/${LIBRARY_NAME}-acc-dev.xml
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT    "Run ${LIBRARY_NAME} abi checker..."
    SOURCES    ${CMAKE_CURRENT_BINARY_DIR}/${LIBRARY_NAME}-acc.xml ${CMAKE_CURRENT_BINARY_DIR}/${LIBRARY_NAME}-acc-dev.xml
)


add_custom_target(abi_generatorrunner_result_check
    COMMAND    ${ABI_CHECKER_RESULT_TOOL} compat_reports/genrunner/stable_to_devel/abi_compat_report.html
    WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT    "Check ${LIBRARY_NAME} ABI compatilbe result..."
)

add_dependencies(result_check abi_generatorrunner_result_check)
