set(LIBNAME "proland-forest")

# Sources
message(STATUS "****    Proland forest    ****")
message(STATUS "PROJECT_SOURCE_DIR: " ${PROJECT_SOURCE_DIR})
set(PROLAND_FOREST_SOURCES "${PROJECT_SOURCE_DIR}/forest/sources" CACHE INTERNAL "")
message(STATUS "PROLAND_FOREST_SOURCES: ${PROLAND_FOREST_SOURCES}")

# Forest is not dependent on any other module
file(GLOB SOURCE_FILES *.cpp plants/*.cpp preprocess/trees/*.cpp *.h plants/*.h preprocess/trees/*.h)

# Libraries
if(UNIX)
	set(LIBS ${LIBS} rt)
endif(UNIX)

# Static or shared?
if(BUILD_SHARED_LIBS AND MSVC)
  add_definitions("-DPROLAND_API=__declspec(dllexport)")
endif(BUILD_SHARED_LIBS AND MSVC)

# Create a static library
add_library(${LIBNAME} ${SOURCE_FILES})
target_link_libraries(${LIBNAME} proland-core ${PROLAND_3RDPARTY_LIBRARIES})
target_include_directories(${LIBNAME} PUBLIC 
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../>
  $<INSTALL_INTERFACE:include>  # <prefix>/include/mylib
)

# Install headers
foreach(subdir plants preprocess/trees ./)
  file(GLOB include-files ${subdir}/*.h)
  install(FILES ${include-files} DESTINATION include/proland/${subdir})
endforeach(subdir)

# Install library
install(TARGETS ${LIBNAME} 
        EXPORT proland-targets
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
        RUNTIME DESTINATION bin)
