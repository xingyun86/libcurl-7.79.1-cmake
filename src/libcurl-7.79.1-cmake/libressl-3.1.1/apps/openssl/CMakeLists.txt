set(
	OPENSSL_SRC
	apps.c
	asn1pars.c
	ca.c
	ciphers.c
	crl.c
	crl2p7.c
	cms.c
	dgst.c
	dh.c
	dhparam.c
	dsa.c
	dsaparam.c
	ec.c
	ecparam.c
	enc.c
	errstr.c
	gendh.c
	gendsa.c
	genpkey.c
	genrsa.c
	nseq.c
	ocsp.c
	openssl.c
	passwd.c
	pkcs12.c
	pkcs7.c
	pkcs8.c
	pkey.c
	pkeyparam.c
	pkeyutl.c
	prime.c
	rand.c
	req.c
	rsa.c
	rsautl.c
	s_cb.c
	s_client.c
	s_server.c
	s_socket.c
	s_time.c
	sess_id.c
	smime.c
	speed.c
	spkac.c
	ts.c
	verify.c
	version.c
	x509.c
)

if(UNIX)
	set(OPENSSL_SRC ${OPENSSL_SRC} apps_posix.c)
	set(OPENSSL_SRC ${OPENSSL_SRC} certhash.c)
endif()

if(WIN32)
	set(OPENSSL_SRC ${OPENSSL_SRC} apps_win.c)
	set(OPENSSL_SRC ${OPENSSL_SRC} certhash_win.c)
	set(OPENSSL_SRC ${OPENSSL_SRC} compat/poll_win.c)
endif()

check_function_exists(strtonum HAVE_STRTONUM)
if(HAVE_STRTONUM)
	add_definitions(-DHAVE_STRTONUM)
else()
	set(OPENSSL_SRC ${OPENSSL_SRC} compat/strtonum.c)
endif()

if(CMAKE_SYSTEM_NAME MATCHES "Darwin")
        check_function_exists(clock_gettime HAVE_CLOCK_GETTIME)
        if(NOT HAVE_CLOCK_GETTIME)
                set(OPENSSL_SRC ${OPENSSL_SRC} compat/clock_gettime_osx.c)
        endif()
endif()

add_executable(openssl ${OPENSSL_SRC})
target_include_directories(openssl PRIVATE . ../../include/compat)

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
	
	target_compile_definitions(openssl
							PRIVATE ${project_name_defines}
							)
	target_compile_options(openssl 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)

target_link_libraries(openssl ${REOPENSSL_LIBS})

if(ENABLE_LIBRESSL_INSTALL)
	install(TARGETS openssl DESTINATION ${CMAKE_INSTALL_BINDIR})
	install(FILES openssl.1 DESTINATION ${CMAKE_INSTALL_MANDIR}/man1)
endif(ENABLE_LIBRESSL_INSTALL)

if(NOT "${OPENSSLDIR}" STREQUAL "")
	set(CONF_DIR "${OPENSSLDIR}")
else()
	set(CONF_DIR "${CMAKE_INSTALL_PREFIX}/etc/ssl")
endif()
if(ENABLE_LIBRESSL_INSTALL)
	install(FILES cert.pem openssl.cnf x509v3.cnf DESTINATION ${CONF_DIR})
	install(DIRECTORY DESTINATION ${CONF_DIR}/certs)
endif(ENABLE_LIBRESSL_INSTALL)
