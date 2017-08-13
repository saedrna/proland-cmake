cmake_minimum_required(VERSION 2.8)

set(EXENAME terrain5)

# Sources
file(GLOB SOURCE_FILES *.cpp)

set(EXAMPLE_EXE_PATH examples/terrain/terrain5)

add_executable(${EXENAME} ${SOURCE_FILES})
target_link_libraries(${EXENAME} proland-terrain ${PROLAND_3RDPARTY_LIBRARIES})

# Copy all files in source tree, except this CMakeLists.txt and source files
file(GLOB MESH_AND_SHADER *.glsl *.mesh *.xml *.png *.bat)
install(TARGETS ${EXENAME} RUNTIME DESTINATION ${EXAMPLE_EXE_PATH})
install(DIRECTORY ${CMAKE_CURRENT_LIST_DIR}/../common/ DESTINATION ${EXAMPLE_EXE_PATH})
install(FILES $<TARGET_PDB_FILE:${EXENAME}> ${MESH_AND_SHADER} DESTINATION ${EXAMPLE_EXE_PATH} OPTIONAL)
