
if (CMAKE_VERSION VERSION_GREATER 2.8.10)
  string (TIMESTAMP DATE "%Y-%m-%d %H:%M")
else ()
  set (DATE "now")
endif ()
# configure_file (version_string.ver.in ${CMAKE_CURRENT_BINARY_DIR}/version_string.ver @ONLY)

file (WRITE ${CMAKE_CURRENT_BINARY_DIR}/version_string.ver "#define __TBB_VERSION_STRINGS(N) \\\n")
file (APPEND ${CMAKE_CURRENT_BINARY_DIR}/version_string.ver "#N\": BUILD_HOST\t\tarch64 (x86_64)\" ENDL \\\n")
file (APPEND ${CMAKE_CURRENT_BINARY_DIR}/version_string.ver "#N\": BUILD_OS\t\tNAME=Arch Linux\" ENDL \\\n")
file (APPEND ${CMAKE_CURRENT_BINARY_DIR}/version_string.ver "#N\": BUILD_KERNEL\tLinux 3.18.6-1-ARCH #1 SMP PREEMPT Sat Feb 7 06:44:05 CET 2015\" ENDL \\\n")
file (APPEND ${CMAKE_CURRENT_BINARY_DIR}/version_string.ver "#N\": BUILD_GCC\t\t${CMAKE_CXX_COMPILER}\" ENDL \\\n")
file (APPEND ${CMAKE_CURRENT_BINARY_DIR}/version_string.ver "#N\": BUILD_LIBC\t2.21\" ENDL \\\n")
file (APPEND ${CMAKE_CURRENT_BINARY_DIR}/version_string.ver "#N\": BUILD_LD\t\t\" ENDL \\\n")
file (APPEND ${CMAKE_CURRENT_BINARY_DIR}/version_string.ver "#N\": BUILD_TARGET\tintel64 on cc4.9.2_libc2.21_kernel3.18.6\" ENDL \\\n")
file (APPEND ${CMAKE_CURRENT_BINARY_DIR}/version_string.ver "#N\": BUILD_COMMAND\t${CMAKE_CXX_COMPILER} ${CMAKE_CXX_FLAGS}\" ENDL \\\n")
file (APPEND ${CMAKE_CURRENT_BINARY_DIR}/version_string.ver "\n#define __TBB_DATETIME \"${DATE}\"\n")


include_directories (${CMAKE_CURRENT_SOURCE_DIR})
include_directories (${CMAKE_CURRENT_BINARY_DIR})


macro (tbb_add_library libname)
  set (sources)
  foreach(arg ${ARGN})
    list (APPEND sources ${arg})
  endforeach()

  add_library (${libname} ${sources})
  set_target_properties (${libname} PROPERTIES SOVERSION 2)
  install (TARGETS ${libname}
          RUNTIME DESTINATION bin
          LIBRARY DESTINATION lib${LIB_SUFFIX}
          ARCHIVE DESTINATION lib${LIB_SUFFIX})
endmacro()



if ("${CMAKE_BUILD_TYPE}" MATCHES Debug)
  add_definitions (-DTBB_USE_DEBUG)
endif ()

find_package (Threads)
if (Threads_FOUND)
  link_libraries (${CMAKE_THREAD_LIBS_INIT})
  if (UNIX)
    add_definitions (-DUSE_PTHREAD)
  endif ()
endif ()
if (UNIX)
  link_libraries (rt)
  link_libraries (dl)
endif ()


add_definitions (-DDO_ITT_NOTIFY)

include (CheckCXXCompilerFlag)
set (RTM_FLAGS "-mrtm")
check_cxx_compiler_flag("${RTM_FLAGS}" _SUPPORT_RTM_FLAGS)
if (_SUPPORT_RTM_FLAGS)
  set (CMAKE_CXX_FLAGS "${RTM_FLAGS} ${CMAKE_CXX_FLAGS}")
endif ()

add_subdirectory (tbb)
add_subdirectory (tbbmalloc)
add_subdirectory (tbbproxy)
add_subdirectory (rml)
add_subdirectory (test)
