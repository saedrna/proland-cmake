cmake_minimum_required(VERSION 2.6)

if(BUILD_SHARED_LIBS AND MSVC)
    add_definitions("-DPROLAND_API=__declspec(dllimport)")
else()
    add_definitions("-DPROLAND_API=")
endif(BUILD_SHARED_LIBS AND MSVC)

set(EXENAME demo)

# Sources
file(GLOB SOURCE_FILES *.cpp)

set(EXAMPLE_EXE_PATH demo)

add_executable(${EXENAME} ${SOURCE_FILES})
target_link_libraries(${EXENAME} proland-core proland-terrain proland-graph proland-edit 
    proland-atmo proland-ocean proland-river proland-forest ${PROLAND_3RDPARTY_LIBRARIES})

file(GLOB MESH_AND_SHADER *.glsl *.mesh *.xml *.png *.bat)
install(TARGETS ${EXENAME} RUNTIME DESTINATION ${EXAMPLE_EXE_PATH})
foreach(dir archives meshes methods shaders textures ui)
    install(DIRECTORY ${CMAKE_CURRENT_LIST_DIR}/${dir} DESTINATION ${EXAMPLE_EXE_PATH})    
endforeach(dir archives meshes methods shaders textures ui)
install(FILES $<TARGET_PDB_FILE:${EXENAME}> ${MESH_AND_SHADER} DESTINATION ${EXAMPLE_EXE_PATH} OPTIONAL)
