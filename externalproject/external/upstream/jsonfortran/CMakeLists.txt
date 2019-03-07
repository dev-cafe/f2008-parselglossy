# f*d-up way of setting up package in upstream, hence we adapt
set(_version 6.10.0)
string(TOLOWER jsonfortran-${CMAKE_Fortran_COMPILER_ID} _jsonfortran)
find_package(${_jsonfortran} ${_version} CONFIG QUIET)

if(${_jsonfortran}_FOUND)
  message(STATUS "Found jsonfortran: ${jsonfortran_INCLUDE_DIR} (found version ${jsonfortran_VERSION})")
  add_library(jsonfortran_external INTERFACE)  # dummy
else()
  include(ExternalProject)
  message(STATUS "Suitable jsonfortran could not be located: downloading and building jsonfortran instead.")
  ExternalProject_Add(jsonfortran_external
    GIT_REPOSITORY https://github.com/jacobwilliams/json-fortran
    GIT_TAG ${_version}
    UPDATE_COMMAND ""
    CMAKE_ARGS
      -DCMAKE_INSTALL_PREFIX=${STAGED_INSTALL_PREFIX}
      -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE}
      -DCMAKE_Fortran_COMPILER=${CMAKE_Fortran_COMPILER}
      -DUSE_GNU_INSTALL_CONVENTION=ON
      -DENABLE_UNICODE=ON
      -DSKIP_DOC_GEN=ON
      -DENABLE_TESTS=OFF
    LOG_DOWNLOAD 1
    LOG_UPDATE 1
    LOG_CONFIGURE 1
    LOG_BUILD 1
    LOG_INSTALL 1
    )
  include(GNUInstallDirs)
  set(${_jsonfortran}_DIR
    ${STAGED_INSTALL_PREFIX}/${CMAKE_INSTALL_LIBDIR}/cmake/${_jsonfortran}-${_version}
    CACHE PATH "Path to internally built ${_jsonfortran}Config.cmake" FORCE)
endif()
