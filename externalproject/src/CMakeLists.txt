cmake_minimum_required(VERSION 3.5 FATAL_ERROR)

project(f2008-parselglossy_core LANGUAGES Fortran)

# f*d-up way of setting up package in upstream, hence we adapt
string(TOLOWER jsonfortran-${CMAKE_Fortran_COMPILER_ID} _jsonfortran)
find_package(${_jsonfortran} CONFIG REQUIRED)
#get_target_property(
#  _loc
#  ${_jsonfortran}::${_jsonfortran}
#  INTERFACE_INCLUDE_DIRECTORIES
#  )
message(STATUS "Found jsonfortran: ${_loc} (found version ${jsonfortran_VERSION})")

add_executable(example example.f90)

target_include_directories(example
  PRIVATE
    ${jsonfortran_INCLUDE_DIRS}
  )

target_link_libraries(example
  PRIVATE
    jsonfortran-static
  )
