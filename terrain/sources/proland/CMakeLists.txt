set(LIBNAME "proland-terrain")

# Sources
message(STATUS "****    Proland terrain    ****")
message(STATUS "Project Source Dir: " ${PROJECT_SOURCE_DIR})
set(PROLAND_TERRAIN_SOURCES "${PROJECT_SOURCE_DIR}/terrain/sources" CACHE INTERNAL "")
include_directories(${PROLAND_CORE_SOURCES} ${PROJECT_SOURCE_DIR}/libraries)

file(GLOB SOURCE_FILES *.cpp */*.cpp preprocess/terrain/*.cpp
                       *.h */*.h preprocess/terrain/*.h)

# Libraries
if(UNIX)
	set(LIBS ${LIBS} rt)
endif(UNIX)

if(BUILD_SHARED_LIBS AND MSVC)
  add_definitions("-DPROLAND_API=__declspec(dllexport)")
else()
  add_definitions("-DPROLAND_API=")
endif(BUILD_SHARED_LIBS AND MSVC)

# Create a library
add_library(${LIBNAME} ${SOURCE_FILES})

target_link_libraries(${LIBNAME} proland-core ${PROLAND_3RDPARTY_LIBRARIES})

target_include_directories(${LIBNAME} PUBLIC 
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../>
  $<INSTALL_INTERFACE:include>  # <prefix>/include/mylib
)

# Install headers
foreach(subdir dem ortho preprocess/terrain ./)
  file(GLOB include-files ${subdir}/*.h)
  install(FILES ${include-files} DESTINATION include/proland/${subdir})
ENDFOREACH(subdir)

# Install library
install(TARGETS ${LIBNAME} 
        EXPORT proland-targets
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
        RUNTIME DESTINATION bin)
