cmake_minimum_required(VERSION 2.6)

set(EXENAME demo)

# Sources
file(GLOB SOURCE_FILES *.cpp)

set(EXAMPLE_EXE_PATH demo)
set(EXECUTABLE_OUTPUT_PATH "${EXECUTABLE_OUTPUT_PATH}${EXAMPLE_EXE_PATH}")
message(STATUS "Setting example output dir: " ${EXECUTABLE_OUTPUT_PATH})

add_executable(${EXENAME} ${SOURCE_FILES})
target_link_libraries(${EXENAME}  -Wl,--whole-archive proland-core proland-terrain proland-graph proland-edit proland-atmo proland-ocean proland-river proland-forest ork -Wl,--no-whole-archive pthread GL GLU GLEW glut glfw3 rt dl Xrandr Xinerama Xxf86vm Xext Xcursor Xrender Xfixes X11 tiff AntTweakBar stb_image tinyxml)



