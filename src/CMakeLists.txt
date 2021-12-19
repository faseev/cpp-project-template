set(PROJECT_BIN ${PROJECT_NAME})
set(PROJECT_LIB ${PROJECT_NAME}_lib)

configure_file(config.h.in ${PROJECT_BINARY_DIR}/config.h)

file(GLOB_RECURSE SOURCES LIST_DIRECTORIES true *.h *.cpp)
list(REMOVE_ITEM SOURCES main.cpp)

add_library(${PROJECT_LIB} STATIC ${SOURCES})
target_include_directories(${PROJECT_LIB} PRIVATE ${PROJECT_BINARY_DIR} ${PROJECT_SOURCE_DIR})
target_precompile_headers(${PROJECT_LIB} PRIVATE stdafx.h)

add_executable (${PROJECT_BIN} main.cpp)
target_link_libraries(${PROJECT_BIN} ${PROJECT_LIB})
target_include_directories(${PROJECT_BIN} PRIVATE ${PROJECT_BINARY_DIR} ${PROJECT_SOURCE_DIR})
target_precompile_headers(${PROJECT_BIN} PRIVATE stdafx.h)

install(TARGETS ${PROJECT_BIN} DESTINATION ${PROJECT_INSTALL_DIR})