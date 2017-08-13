cmake_minimum_required(VERSION 2.6)

set(EXENAME twbartest)

# Sources
file(GLOB SOURCE_FILES *.cpp)

# Assign output directory for this example
set(EXAMPLE_EXE_PATH examples/core/twbartest)

add_executable(${EXENAME} ${SOURCE_FILES})
target_link_libraries(${EXENAME} proland-core ${PROLAND_3RDPARTY_LIBRARIES})

install(TARGETS ${EXENAME} RUNTIME DESTINATION ${EXAMPLE_EXE_PATH})
install(DIRECTORY ${CMAKE_CURRENT_LIST_DIR}/../common/ DESTINATION ${EXAMPLE_EXE_PATH})
if(MSVC)
    install(FILES $<TARGET_PDB_FILE:${EXENAME}> helloworld.xml quad.mesh terrainShader.glsl DESTINATION ${EXAMPLE_EXE_PATH} OPTIONAL)
else(MSVC)
    install(FILES helloworld.xml quad.mesh terrainShader.glsl DESTINATION ${EXAMPLE_EXE_PATH} OPTIONAL)
endif(MSVC)
