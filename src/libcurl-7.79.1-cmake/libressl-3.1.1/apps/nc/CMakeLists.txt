if(BUILD_NC)

set(
	NC_SRC
	atomicio.c
	netcat.c
	socks.c
	compat/socket.c
)

check_function_exists(b64_ntop HAVE_B64_NTOP)
if(HAVE_B64_NTOP)
	add_definitions(-DHAVE_B64_NTOP)
else()
	set(NC_SRC ${NC_SRC} compat/base64.c)
endif()

check_function_exists(accept4 HAVE_ACCEPT4)
if(HAVE_ACCEPT4)
	add_definitions(-DHAVE_ACCEPT4)
else()
	set(NC_SRC ${NC_SRC} compat/accept4.c)
endif()

check_function_exists(readpassphrase HAVE_READPASSPHRASE)
if(HAVE_READPASSPHRASE)
	add_definitions(-DHAVE_READPASSPHRASE)
else()
	set(NC_SRC ${NC_SRC} compat/readpassphrase.c)
endif()

check_function_exists(strtonum HAVE_STRTONUM)
if(HAVE_STRTONUM)
	add_definitions(-DHAVE_STRTONUM)
else()
	set(NC_SRC ${NC_SRC} compat/strtonum.c)
endif()

if(NOT "${OPENSSLDIR}" STREQUAL "")
	add_definitions(-DDEFAULT_CA_FILE=\"${OPENSSLDIR}/cert.pem\")
else()
	add_definitions(-DDEFAULT_CA_FILE=\"${CMAKE_INSTALL_PREFIX}/etc/ssl/cert.pem\")
endif()

add_executable(re_nc ${NC_SRC})
target_include_directories(re_nc PRIVATE . ./compat ../../include/compat)

if (MSVC)
	list(APPEND project_name_cflags /W4)
	list(APPEND project_name_defines WIN32_LEAN_AND_MEAN _WIN32_WINNT=0x0501)
	MESSAGE(STATUS "CMAKE_BUILD_TYPE=" ${CMAKE_BUILD_TYPE})

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
	
	target_compile_definitions(re_nc
							PRIVATE ${project_name_defines}
							)
	target_compile_options(re_nc 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)

target_link_libraries(re_nc re_tls_static ${REOPENSSL_LIBS})

if(ENABLE_NC)
	if(ENABLE_LIBRESSL_INSTALL)
		install(TARGETS re_nc DESTINATION ${CMAKE_INSTALL_BINDIR})
		install(FILES re_nc.1 DESTINATION ${CMAKE_INSTALL_MANDIR}/man1)
	endif(ENABLE_LIBRESSL_INSTALL)
endif()

endif()
