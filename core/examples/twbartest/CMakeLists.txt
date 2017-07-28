cmake_minimum_required(VERSION 2.6)

set(EXENAME twbartest)

# Sources
file(GLOB SOURCE_FILES *.cpp)

# Assign output directory for this example
set(EXAMPLE_EXE_PATH "/examples/core/twbartest")
set(EXECUTABLE_OUTPUT_PATH "${EXECUTABLE_OUTPUT_PATH}${EXAMPLE_EXE_PATH}")
message(STATUS "Setting example output dir: " ${EXECUTABLE_OUTPUT_PATH})

add_executable(${EXENAME} ${SOURCE_FILES})
target_link_libraries(${EXENAME} proland-core ${PROLAND_3RDPARTY_LIBRARIES})

# Copy all files in source tree, except this CMakeLists.txt
add_custom_command(TARGET ${EXENAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory ${CMAKE_CURRENT_SOURCE_DIR} ${EXECUTABLE_OUTPUT_PATH})
add_custom_command(TARGET ${EXENAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E remove ${EXECUTABLE_OUTPUT_PATH}/CMakeLists.txt)

