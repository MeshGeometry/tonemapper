cmake_minimum_required(VERSION 2.6)

project(tonemapper)

set(CMAKE_BUILD_TYPE Release)

if(MSVC)
  if(CMAKE_CXX_FLAGS MATCHES "/W[0-4]")
    string(REGEX REPLACE "/W[0-4]" "/W4" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
  else()
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4")
  endif()
else()
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -stdlib=libc++ -Wall")
endif()

set(NANOGUI_BUILD_EXAMPLE OFF CACHE BOOL " " FORCE)
set(NANOGUI_BUILD_SHARED  OFF CACHE BOOL " " FORCE)
set(NANOGUI_BUILD_PYTHON  OFF CACHE BOOL " " FORCE)
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/ext/nanogui ext_build/nanogui)


include_directories(
	${CMAKE_CURRENT_SOURCE_DIR}/ext/nanogui/ext/eigen
	${CMAKE_CURRENT_SOURCE_DIR}/ext/nanogui/ext/glfw/include
	${CMAKE_CURRENT_SOURCE_DIR}/ext/nanogui/ext/glew/include
	${CMAKE_CURRENT_SOURCE_DIR}/ext/nanogui/ext/nanovg/src
	${CMAKE_CURRENT_SOURCE_DIR}/ext/nanogui/include
	${CMAKE_CURRENT_SOURCE_DIR}/ext/filesystem
	${CMAKE_CURRENT_SOURCE_DIR}/ext/tinyexr
	${CMAKE_CURRENT_SOURCE_DIR}/ext/stb
	${CMAKE_CURRENT_SOURCE_DIR}/src
)

file(GLOB_RECURSE ALL_FILES
	"${PROJECT_SOURCE_DIR}/src/*.cpp"
)

add_executable(tonemapper
	src/main.cpp

	src/image.cpp
)

target_link_libraries(tonemapper nanogui ${NANOGUI_EXTRA_LIBS})

set_target_properties(tonemapper PROPERTIES OUTPUT_NAME "tonemapper")