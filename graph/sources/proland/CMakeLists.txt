set(LIBNAME "proland-graph")

# Sources
message(STATUS "****    Proland graph    ****")
message(STATUS "PROJECT_SOURCE_DIR: " ${PROJECT_SOURCE_DIR})
set(PROLAND_GRAPH_SOURCES "${PROJECT_SOURCE_DIR}/graph/sources" CACHE INTERNAL "")
message(STATUS "PROLAND_GRAPH_SOURCES: ${PROLAND_GRAPH_SOURCES}")

file(GLOB SOURCE_FILES *.cpp */*.cpp graph/producer/*.cpp)

# Libraries
if(UNIX)
	set(LIBS ${LIBS} rt)
endif(UNIX)

# Static or shared?
set(LIBTYPE STATIC)
if(BUILD_SHARED_LIBS AND MSVC)
  add_definitions("-DPROLAND_API=__declspec(dllexport)")
endif(BUILD_SHARED_LIBS AND MSVC)

# Create a static library
add_library(${LIBNAME} ${SOURCE_FILES})
target_link_libraries(${LIBNAME} proland-core proland-terrain ${PROLAND_3RDPARTY_LIBRARIES})
target_include_directories(${LIBNAME} PUBLIC 
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../>
  $<INSTALL_INTERFACE:include>  # <prefix>/include/mylib
)

# Install headers
foreach(subdir dem graph graph/producer ortho ./)
  file(GLOB include-files ${subdir}/*.h)
  install(FILES ${include-files} DESTINATION include/proland/${subdir})
endforeach(subdir)

# Install library
install(TARGETS ${LIBNAME} 
        EXPORT proland-targets
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
        RUNTIME DESTINATION bin)
