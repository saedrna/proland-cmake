project(Proland CXX C)
cmake_minimum_required(VERSION 2.6)
cmake_policy(VERSION 2.6)


SET(PROLAND_VERSION_MAJOR 4)
SET(PROLAND_VERSION_MINOR 0)
SET(PROLAND_VERSION "${PROLAND_VERSION_MAJOR}.${PROLAND_VERSION_MINOR}")

# Avoid source tree pollution
if(CMAKE_SOURCE_DIR STREQUAL CMAKE_BINARY_DIR)
	message(FATAL_ERROR "In-source builds are not permitted. Make a separate folder for building:\nmkdir build; cd build; cmake ..\nBefore that, remove the files already created:\nrm -rf CMakeCache.txt CMakeFiles")
endif(CMAKE_SOURCE_DIR STREQUAL CMAKE_BINARY_DIR)

# Add a sensible build type default and warning because empty means no optimization and no debug info.
if(NOT CMAKE_BUILD_TYPE)
	message("WARNING: CMAKE_BUILD_TYPE is not defined!\n         Defaulting to CMAKE_BUILD_TYPE=RelWithDebInfo. Use ccmake to set a proper value.")
	set(CMAKE_BUILD_TYPE RelWithDebInfo CACHE STRING "Choose the type of build, options are: None Debug Release RelWithDebInfo MinSizeRel." FORCE)
endif(NOT CMAKE_BUILD_TYPE)

# Common deps
find_package(ork REQUIRED) # ork::ork
find_package(pthread REQUIRED) # pthread::pthread
find_package(anttweakbar REQUIRED) # anttweakbar::anttweakbar
find_package(TIFF REQUIRED) # TIFF::TIFF
set(PROLAND_3RDPARTY_LIBRARIES ork::ork pthread::pthread anttweakbar::anttweakbar TIFF::TIFF)

set_target_properties(${PROLAND_3RDPARTY_LIBRARIES} PROPERTIES MAP_IMPORTED_CONFIG_RELWITHDEBINFO RELEASE)

option(BUILD_SHARED_LIBS    "Build shared library instead of static"   ON)
option(BUILD_EXAMPLES       "Build examples"                           ON )
#option(BUILD_TESTS       "Build tests"                              ON )

#General compiler options:
if(MSVC)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /wd4251 /wd4244 /wd4996 /wd4267 /wd4305")
    set(CMAKE_DEBUG_POSTFIX "-gd")
#     if(BUILD_SHARED_LIBS AND MSVC)
#   target_compile_definitions(${LIBNAME} 
#     INTERFACE "-DPROLAND_API=__declspec(dllimport)"
# 		PRIVATE   "-DPROLAND_API=__declspec(dllexport)")
# else(BUILD_SHARED_LIBS AND MSVC)
# 	target_compile_definitions(${LIBNAME} PUBLIC -DPROLAND_API=)
# endif(BUILD_SHARED_LIBS AND MSVC)
endif(MSVC)

message(STATUS "CXX FLAGS: " ${CMAKE_CXX_FLAGS})

# Sub dirs
#add_subdirectory(libraries)
add_subdirectory(core)
add_subdirectory(terrain)
add_subdirectory(atmo)
add_subdirectory(ocean)
add_subdirectory(forest)
add_subdirectory(river)
add_subdirectory(graph)
add_subdirectory(edit)

if(BUILD_EXAMPLES)
	add_subdirectory(core/examples)
    add_subdirectory(terrain/examples)
    add_subdirectory(atmo/examples)
    add_subdirectory(ocean/examples)
    add_subdirectory(forest/examples)
    add_subdirectory(graph/examples)
    add_subdirectory(river/examples)
    add_subdirectory(edit/examples)
endif(BUILD_EXAMPLES)

add_subdirectory(demo)

#if(BUILD_TESTS)
#	add_subdirectory(test)
#endif(BUILD_TESTS)

# Layout. This works for all platforms:
#   * <prefix>/lib/cmake/<PROJECT-NAME>
#   * <prefix>/lib/
#   * <prefix>/include/
set(config_install_dir "cmake/proland")
set(include_install_dir "include")

set(generated_dir "${CMAKE_CURRENT_BINARY_DIR}/generated")

# Configuration
set(version_config "${generated_dir}/proland-config-version.cmake")
set(project_config "${generated_dir}/proland-config.cmake")
set(targets_export_name "proland-targets")
set(namespace "proland::")

# Include module with fuction 'write_basic_package_version_file'
include(CMakePackageConfigHelpers)

write_basic_package_version_file(
    "${version_config}" COMPATIBILITY SameMajorVersion VERSION ${PROLAND_VERSION}
)

configure_package_config_file(
    "config.cmake.in"
    "${project_config}"
    INSTALL_DESTINATION "${config_install_dir}"
    PATH_VARS BUILD_SHARED_LIBS
)

install(
    FILES "${project_config}" "${version_config}"
    DESTINATION "${config_install_dir}"
)

install(EXPORT proland-targets 
	    NAMESPACE proland::
		DESTINATION cmake/proland)
