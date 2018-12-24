cmake_minimum_required(VERSION 3.5 FATAL_ERROR)

project(f2008-parselglossy LANGUAGES Fortran)

set_property(DIRECTORY PROPERTY EP_BASE ${CMAKE_BINARY_DIR}/subprojects)

set(STAGED_INSTALL_PREFIX ${CMAKE_BINARY_DIR}/stage)
message(STATUS "${PROJECT_NAME} staged install: ${STAGED_INSTALL_PREFIX}")

add_subdirectory(external/upstream)
# f*d-up way of setting up package in upstream, hence we adapt
string(TOLOWER jsonfortran-${CMAKE_Fortran_COMPILER_ID} _jsonfortran)

include(ExternalProject)

ExternalProject_Add(${PROJECT_NAME}_core
  DEPENDS
    jsonfortran_external
  SOURCE_DIR
    ${CMAKE_CURRENT_LIST_DIR}/src
  CMAKE_ARGS
    -DCMAKE_Fortran_COMPILER=${CMAKE_Fortran_COMPILER}
    -D${_jsonfortran}_DIR=${${_jsonfortran}_DIR}
  CMAKE_CACHE_ARGS
    -DCMAKE_Fortran_FLAGS:STRING=${CMAKE_Fortran_FLAGS}
  BUILD_ALWAYS
    1
  INSTALL_COMMAND
    ""
  )
