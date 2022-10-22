
set(UT_SRC
  unit1300.c
  unit1301.c
  unit1302.c
  unit1303.c
  unit1304.c
  unit1305.c
  unit1307.c
  unit1308.c
  unit1309.c
  unit1330.c
# Broken link on Linux
#  unit1394.c
  unit1395.c
  unit1396.c
  unit1397.c
  unit1398.c
  unit1600.c
  unit1601.c
  unit1603.c
# Broken link on Linux
#  unit1604.c
  unit1620.c
  unit1655.c
  )

set(UT_COMMON_FILES ../libtest/first.c ../libtest/test.h curlcheck.h)
include_directories(
  ${CURL_SOURCE_DIR}/lib          # To be able to reach "curl_setup_once.h"
  ${CURL_SOURCE_DIR}/tests/libtest
  ${CURL_SOURCE_DIR}/src
  ${CURL_BINARY_DIR}/lib          # To be able to reach "curl_config.h"
  ${CURL_BINARY_DIR}/include      # To be able to reach "curl/curl.h"
)

foreach(_testfile ${UT_SRC})

if (MSVC)
	#set(CMAKE_CXX_FLAGS "$ENV{CXXFLAGS} ${CMAKE_CXX_FLAGS} /std:c++17 /std:c++latest -D_HAS_CXX17=1 -D_HAS_CXX20=1")
	#set(CMAKE_CXX_FLAGS "$ENV{CXXFLAGS} ${CMAKE_CXX_FLAGS} -DUWS_NO_ZLIB /std:c++17 /std:c++latest -D_HAS_CXX17=1 -D_HAS_CXX20=1")
    set(CompilerFlags
        CMAKE_CXX_FLAGS
        CMAKE_CXX_FLAGS_DEBUG
        CMAKE_CXX_FLAGS_RELEASE
        CMAKE_C_FLAGS
        CMAKE_C_FLAGS_DEBUG
        CMAKE_C_FLAGS_RELEASE
        )
    foreach(CompilerFlag ${CompilerFlags})
        string(REPLACE "/MD" "/MT" ${CompilerFlag} "${${CompilerFlag}}")
    endforeach()
	#list(APPEND uws_libraries
    #        libiconv
    #        libcurl
	#		zlibstatic)
else()
	#set(CMAKE_CXX_FLAGS "$ENV{CXXFLAGS} ${CMAKE_CXX_FLAGS} -std=c++17")
	#list(APPEND uws_libraries
	#		pthread
	#		stdc++fs
    #        libiconv
    #        libcurl
	#		zlibstatic)
    set(CMAKE_C_FLAGS "$ENV{CFLAGS} ${CMAKE_C_FLAGS} -fPIC")
    set(CMAKE_CXX_FLAGS "$ENV{CXXFLAGS} ${CMAKE_CXX_FLAGS} -fPIC")
endif(MSVC)
  get_filename_component(_testname ${_testfile} NAME_WE)
  add_executable(${_testname} ${_testfile} ${UT_COMMON_FILES})
  target_link_libraries(${_testname} libcurl_static ${CURL_LIBS})
  set_target_properties(${_testname}
      PROPERTIES COMPILE_DEFINITIONS "UNITTESTS")

  if(HIDES_CURL_PRIVATE_SYMBOLS)
    set_target_properties(${_testname}
      PROPERTIES
      EXCLUDE_FROM_ALL TRUE
      EXCLUDE_FROM_DEFAULT_BUILD TRUE
    )
  else()
    add_test(NAME ${_testname}
             COMMAND ${_testname} "http://www.google.com"
    )
  endif()
endforeach()
