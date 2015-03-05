project (tbb)

cmake_minimum_required (VERSION 2.8)

if (NOT DEFINED CMAKE_BUILD_TYPE)
  set (CMAKE_BUILD_TYPE Release CACHE STRING "Build type")
endif ()

option (BUILD_SHARED_LIBS "build shared/static libs" ON)
option (BUILD_SAMPLES "build samples" OFF)

# build tarballs via package_source target
set (CPACK_PACKAGE_VERSION_MAJOR 4)
set (CPACK_PACKAGE_VERSION_MINOR 3)
set (CPACK_PACKAGE_VERSION_PATCH 3)
set (CPACK_SOURCE_GENERATOR      "ZIP;TGZ")
set (CPACK_SOURCE_IGNORE_FILES "/.git;/build;.*~;${CPACK_SOURCE_IGNORE_FILES}" )
set (PACKAGE_NAME ${PROJECT_NAME})
set (PACKAGE_VERSION        ${CPACK_PACKAGE_VERSION_MAJOR}.${CPACK_PACKAGE_VERSION_MINOR}.${CPACK_PACKAGE_VERSION_PATCH})
set (CPACK_SOURCE_PACKAGE_FILE_NAME ${PACKAGE_NAME}-${PACKAGE_VERSION})
include (CPack)

include_directories (include)
install (DIRECTORY include/tbb DESTINATION include REGEX \\.h$ )

add_subdirectory (src)
if (BUILD_SAMPLES)
  add_subdirectory (examples)
endif ()