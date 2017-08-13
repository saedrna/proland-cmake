cmake_minimum_required(VERSION 2.6)

set(EXENAME core-helloworld)

   
#mainline include dirs
include_directories(${PROLAND_CORE_SOURCES})

# Sources
file(GLOB SOURCE_FILES *.cpp)

# Assign output directory for this example
set(EXAMPLE_EXE_PATH examples/core/helloworld)

add_executable(${EXENAME} ${SOURCE_FILES})

target_link_libraries(${EXENAME} proland-core ${PROLAND_3RDPARTY_LIBRARIES})
install(TARGETS ${EXENAME} RUNTIME DESTINATION ${EXAMPLE_EXE_PATH})
install(DIRECTORY ${CMAKE_CURRENT_LIST_DIR}/../common/  config/ DESTINATION ${EXAMPLE_EXE_PATH})

if(MSVC)
    install(FILES $<TARGET_PDB_FILE:${EXENAME}> helloworld.xml DESTINATION ${EXAMPLE_EXE_PATH} OPTIONAL)
else(MSVC)
    install(FILES helloworld.xml DESTINATION ${EXAMPLE_EXE_PATH} OPTIONAL)
endif(MSVC)

