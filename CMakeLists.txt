cmake_minimum_required(VERSION 3.26)
project(ANN)

set(CMAKE_CXX_STANDARD 11)

# Define the source files
set(SOURCES
        src/ANN.cpp
        src/brute.cpp
        src/kd_tree.cpp
        src/kd_util.cpp
        src/kd_split.cpp
        src/kd_search.cpp
        src/kd_pr_search.cpp
        src/kd_fix_rad_search.cpp
        src/kd_dump.cpp
        src/bd_tree.cpp
        src/bd_search.cpp
        src/bd_pr_search.cpp
        src/bd_fix_rad_search.cpp
        src/perf.cpp
)

# Define the header files
set(HEADERS
        src/kd_tree.h
        src/kd_split.h
        src/kd_util.h
        src/kd_search.h
        src/kd_pr_search.h
        src/kd_fix_rad_search.h
        perf.h
        src/pr_queue.h
        src/pr_queue_k.h
)

add_compile_options("-fPIC")
# Create the library
add_library(${PROJECT_NAME} ${SOURCES})

# Specify include directories
target_include_directories(${PROJECT_NAME} PUBLIC ${CMAKE_SOURCE_DIR}/include)

# Set compiler flags if needed
# target_compile_options(${PROJECT_NAME} PRIVATE ${YOUR_COMPILER_FLAGS})

# Specify the library output directory
#set(LIBRARY_OUTPUT_PATH ${CMAKE_SOURCE_DIR}/lib)

# Install the library
install(TARGETS ${PROJECT_NAME}
        DESTINATION lib)

set(HEADERS_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/include)

# Install the headers
install(DIRECTORY ${HEADERS_INCLUDE_DIR}
        DESTINATION include)

# Create and install a custom package configuration file for find_package
set(CONFIG_INSTALL_DIR lib/cmake/${PROJECT_NAME})

configure_file(
        ${CMAKE_SOURCE_DIR}/Config.cmake.in
        ${CMAKE_BINARY_DIR}/${PROJECT_NAME}Config.cmake
)

install(FILES ${CMAKE_BINARY_DIR}/${PROJECT_NAME}Config.cmake
        DESTINATION ${CONFIG_INSTALL_DIR})

# Optionally, add a 'make clean' target (equivalent to 'make realclean' in the original Makefile)
add_custom_target(clean_all
        COMMAND ${CMAKE_COMMAND} -P ${CMAKE_SOURCE_DIR}/clean_all.cmake
)
