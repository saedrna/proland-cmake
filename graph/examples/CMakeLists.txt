if(BUILD_SHARED_LIBS AND MSVC)
    add_definitions("-DPROLAND_API=__declspec(dllimport)")
else()
    add_definitions("-DPROLAND_API=")
endif(BUILD_SHARED_LIBS AND MSVC)

add_subdirectory(graph1)
