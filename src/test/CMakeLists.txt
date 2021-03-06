# gtest external download
set(GTEST_INSTALL_DIR "${EXTERNAL_DIR}/gtest")
if (WIN32)
   set(GTEST_SHARED_ARG ON)
else()
   set(GTEST_SHARED_ARG OFF)
endif()

ExternalProject_Add( gtest
  PREFIX "${GTEST_INSTALL_DIR}"
  GIT_REPOSITORY "https://github.com/google/googletest.git"
  INSTALL_DIR "${GTEST_INSTALL_DIR}/include"
  BINARY_DIR "${GTEST_INSTALL_DIR}/lib"
  INSTALL_COMMAND ""
  CMAKE_ARGS 
    -Dgtest_build_tests:BOOL=OFF
    -DBUILD_GMOCK:BOOL=OFF
    -DBUILD_GTEST:BOOL=ON
    -Dgtest_force_shared_crt:BOOL=${GTEST_SHARED_ARG}
)                   
ExternalProject_Add_Step( gtest copy_deps
  COMMAND ${CMAKE_COMMAND} -E copy_directory ${GTEST_INSTALL_DIR}/src/gtest/googletest/include ${GTEST_INSTALL_DIR}/include
  DEPENDEES install
)
include_directories("${GTEST_INSTALL_DIR}/include")
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../core2d/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../core3d/include)
SET(GTEST_LIBRARY "${GTEST_INSTALL_DIR}/lib/googletest/${CMAKE_STATIC_LIBRARY_PREFIX}gtest${CMAKE_STATIC_LIBRARY_SUFFIX};${GTEST_INSTALL_DIR}/lib/googletest/${CMAKE_STATIC_LIBRARY_PREFIX}gtest_main${CMAKE_STATIC_LIBRARY_SUFFIX}")
add_subdirectory(cli)
