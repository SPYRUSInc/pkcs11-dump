cmake_minimum_required(VERSION 2.8.9)
project(pkcs11-dump)

# Set a default build type if none was specified
set(default_build_type "Release")
if(EXISTS "${CMAKE_SOURCE_DIR}/.git")
  set(default_build_type "Debug")
endif()
 
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  message(STATUS "Setting build type to '${default_build_type}' as none was specified.")
  set(CMAKE_BUILD_TYPE "${default_build_type}" CACHE
      STRING "Choose the type of build." FORCE)
  # Set the possible values of build type for cmake-gui
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS
    "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
else()
  message(STATUS "Build type to '${CMAKE_BUILD_TYPE}'")
endif()

if (UNIX)
	SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}" )
	link_directories(/usr/local/lib)
	SET(PROJECT_LINK_LIBS dl crypto)
elseif (WIN32)
	SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}" )
	SET(PROJECT_LINK_LIBS libeay32.lib)
else()
	message(STATUS "Platform not configured.")
endif()

set(source_c_files
./pkcs11-dump.cpp
)

# the application
add_executable(pkcs11-dump ${source_c_files})
target_link_libraries(pkcs11-dump ${PROJECT_LINK_LIBS})
