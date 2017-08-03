set(LIBNAME "proland-ocean")

# Sources
message(STATUS "****    Proland ocean    ****")
message(STATUS "PROJECT_SOURCE_DIR: " ${PROJECT_SOURCE_DIR})
set(PROLAND_OCEAN_SOURCES "${PROJECT_SOURCE_DIR}/ocean/sources" CACHE INTERNAL "")
message(STATUS "PROLAND_OCEAN_SOURCES: ${PROLAND_OCEAN_SOURCES}")

# Atmo is not dependent on any other module
include_directories(${PROLAND_CORE_SOURCES} ${PROJECT_SOURCE_DIR}/libraries ${PROLAND_OCEAN_SOURCES})

file(GLOB SOURCE_FILES *.cpp ocean/*.cpp)

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
foreach(subdir ocean ./)
  file(GLOB include-files ${subdir}/*.h)
  install(FILES ${include-files} DESTINATION include/proland/${subdir})
endforeach(subdir)

# Install library
install(TARGETS ${LIBNAME} 
        EXPORT proland-targets
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
        RUNTIME DESTINATION bin)