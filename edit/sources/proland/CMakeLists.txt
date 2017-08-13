set(LIBNAME "proland-edit")

# Sources
message(STATUS "****    Proland edit    ****")
message(STATUS "PROJECT_SOURCE_DIR: " ${PROJECT_SOURCE_DIR})
set(PROLAND_EDIT_SOURCES "${PROJECT_SOURCE_DIR}/edit/sources" CACHE INTERNAL "")
message(STATUS "PROLAND_EDIT_SOURCES: ${PROLAND_EDIT_SOURCES}")

file(GLOB SOURCE_FILES *.cpp */*.cpp ui/twbar/*.cpp *.h */*.h ui/twbar/*.h)

# Libraries

# Static or shared?
set(LIBTYPE STATIC)
if(BUILD_SHARED_LIBS AND MSVC)
  add_definitions("-DPROLAND_API=__declspec(dllexport)")
else()
  add_definitions("-DPROLAND_API=")
endif(BUILD_SHARED_LIBS AND MSVC)

# Create a static library
add_library(${LIBNAME} ${SOURCE_FILES})
target_link_libraries(${LIBNAME} proland-core proland-terrain proland-graph proland-river ${PROLAND_3RDPARTY_LIBRARIES})
target_include_directories(${LIBNAME} PUBLIC 
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../>
  $<INSTALL_INTERFACE:include>  # <prefix>/include/mylib
)

# Install headers
foreach(subdir edit ui/twbar ./)
  file(GLOB include-files ${subdir}/*.h)
  install(FILES ${include-files} DESTINATION include/proland/${subdir})
ENDFOREACH(subdir)

# Install library
install(TARGETS ${LIBNAME} 
        EXPORT proland-targets
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
        RUNTIME DESTINATION bin)
