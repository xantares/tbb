include_directories (${CMAKE_CURRENT_SOURCE_DIR})
include_directories (${CMAKE_SOURCE_DIR}/src/tbb)

add_definitions (-D__TBBMALLOC_BUILD=1)

tbb_add_library (tbbmalloc
  backend.cpp
  large_objects.cpp
  backref.cpp
  tbbmalloc.cpp
  itt_notify_malloc.cpp
  frontend.cpp)