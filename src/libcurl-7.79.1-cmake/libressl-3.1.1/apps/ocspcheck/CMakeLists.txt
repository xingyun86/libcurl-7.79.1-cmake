if(NOT MSVC)

set(
	OCSPCHECK_SRC
	http.c
	ocspcheck.c
)

check_function_exists(memmem HAVE_MEMMEM)
if(HAVE_MEMMEM)
        add_definitions(-DHAVE_MEMMEM)
else()
        set(OCSPCHECK_SRC ${OCSPCHECK_SRC} compat/memmem.c)
endif()

if(NOT "${OPENSSLDIR}" STREQUAL "")
	add_definitions(-DDEFAULT_CA_FILE=\"${OPENSSLDIR}/cert.pem\")
else()
	add_definitions(-DDEFAULT_CA_FILE=\"${CMAKE_INSTALL_PREFIX}/etc/ssl/cert.pem\")
endif()

add_executable(re_ocspcheck ${OCSPCHECK_SRC})
target_include_directories(re_ocspcheck PRIVATE . ./compat ../../include/compat)

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
	
	target_compile_definitions(re_ocspcheck
							PRIVATE ${project_name_defines}
							)
	target_compile_options(re_ocspcheck 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)

target_link_libraries(re_ocspcheck re_tls_static ${REOPENSSL_LIBS})

if(ENABLE_LIBRESSL_INSTALL)
	install(TARGETS re_ocspcheck DESTINATION ${CMAKE_INSTALL_BINDIR})
	install(FILES re_ocspcheck.8 DESTINATION ${CMAKE_INSTALL_MANDIR}/man8)

endif(ENABLE_LIBRESSL_INSTALL)

endif()
