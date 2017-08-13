set(LIBNAME "proland-river")

# Sources
message(STATUS "****    Proland river    ****")
message(STATUS "PROJECT_SOURCE_DIR: " ${PROJECT_SOURCE_DIR})
set(PROLAND_RIVER_SOURCES "${PROJECT_SOURCE_DIR}/river/sources" CACHE INTERNAL "")
message(STATUS "PROLAND_RIVER_SOURCES: ${PROLAND_RIVER_SOURCES}")

file(GLOB SOURCE_FILES *.cpp */*.cpp rivers/graph/*.cpp *.h */*.h rivers/graph/*.h)

# Libraries
if(UNIX)
	set(LIBS ${LIBS} rt)
endif(UNIX)

# Static or shared?
if(BUILD_SHARED_LIBS AND MSVC)
  add_definitions("-DPROLAND_API=__declspec(dllexport)")
else()
  add_definitions("-DPROLAND_API=")
endif(BUILD_SHARED_LIBS AND MSVC)

# Create a static library
add_library(${LIBNAME} ${SOURCE_FILES})
target_link_libraries(${LIBNAME} proland-core proland-graph ${PROLAND_3RDPARTY_LIBRARIES})
target_include_directories(${LIBNAME} PUBLIC 
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../>
  $<INSTALL_INTERFACE:include>  # <prefix>/include/mylib
)

# Install headers
foreach(subdir rivers rivers/graph ui/twbar ./)
  file(GLOB include-files ${subdir}/*.h)
  install(FILES ${include-files} DESTINATION include/proland/${subdir})
endforeach(subdir)

# Install library
install(TARGETS ${LIBNAME} 
        EXPORT proland-targets
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
        RUNTIME DESTINATION bin)
