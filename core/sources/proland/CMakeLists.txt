set(LIBNAME "proland-core")

# Sources
message(STATUS "****    Proland core    ****")
message(STATUS "PROJECT_SOURCE_DIR: " ${PROJECT_SOURCE_DIR})
set(PROLAND_CORE_SOURCES "${PROJECT_SOURCE_DIR}/core/sources" CACHE INTERNAL "")
message(STATUS "PROLAND_CORE_SOURCES: ${PROLAND_CORE_SOURCES}")
include_directories(${PROLAND_CORE_SOURCES} ${PROJECT_SOURCE_DIR}/libraries)

file(GLOB SOURCE_FILES *.cpp */*.cpp ui/twbar/*.cpp particles/screen/*.cpp particles/terrain/*.cpp)

# Libraries
if(UNIX)
	set(LIBS ${LIBS} rt)
endif(UNIX)

# Create a static library
add_library(${LIBNAME} ${SOURCE_FILES})
target_link_libraries(${LIBNAME} ${PROLAND_3RDPARTY_LIBRARIES})

if(BUILD_SHARED_LIBS AND MSVC)
  target_compile_definitions(${LIBNAME} 
    INTERFACE "-DPROLAND_API=__declspec(dllimport)"
		PRIVATE   "-DPROLAND_API=__declspec(dllexport)")
else(BUILD_SHARED_LIBS AND MSVC)
	target_compile_definitions(${LIBNAME} PUBLIC -DPROLAND_API=)
endif(BUILD_SHARED_LIBS AND MSVC)

target_include_directories(proland-core INTERFACE 
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
  $<INSTALL_INTERFACE:include>  # <prefix>/include/mylib
)

# Adds SO Version and subversion. To be added to ensure ABI/API compatibility.
#SET_TARGET_PROPERTIES(${LIBNAME} PROPERTIES SOVERSION ${ORK_VERSION_MAJOR} VERSION ${ORK_VERSION})

# Install headers
foreach(subdir math particles particles/screen particles/terrain producer terrain ui ui/twbar util)
  file(GLOB include-files ${subdir}/*.h)
  install(FILES ${include-files} DESTINATION include/proland/${subdir})
endforeach(subdir)

# Install library
install(TARGETS ${LIBNAME} 
        EXPORT proland-targets
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
        RUNTIME DESTINATION bin)

