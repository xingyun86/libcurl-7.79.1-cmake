include_directories(
	.
	../crypto/modes
	../crypto/asn1
	../ssl
	../tls
	../apps/openssl
	../apps/openssl/compat
	../include/compat
)

add_definitions(-D_PATH_SSL_CA_FILE=\"${CMAKE_CURRENT_SOURCE_DIR}/../apps/openssl/cert.pem\")

file(TO_NATIVE_PATH ${CMAKE_CURRENT_SOURCE_DIR} TEST_SOURCE_DIR)

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
endif(MSVC)

# aeadtest
add_executable(aeadtest aeadtest.c)
if (MSVC)
	target_compile_definitions(aeadtest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(aeadtest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(aeadtest ${REOPENSSL_LIBS})
add_test(aeadtest aeadtest ${CMAKE_CURRENT_SOURCE_DIR}/aeadtests.txt)

# aes_wrap
add_executable(aes_wrap aes_wrap.c)
if (MSVC)
	target_compile_definitions(aes_wrap
							PRIVATE ${project_name_defines}
							)
	target_compile_options(aes_wrap 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(aes_wrap ${REOPENSSL_LIBS})
add_test(aes_wrap aes_wrap)

# arc4randomforktest
# Windows/mingw does not have fork, but Cygwin does.
if(NOT (WIN32 OR (CMAKE_SYSTEM_NAME MATCHES "MINGW")))
	add_executable(arc4randomforktest arc4randomforktest.c)
if (MSVC)
	target_compile_definitions(arc4randomforktest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(arc4randomforktest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(arc4randomforktest ${REOPENSSL_LIBS})
	add_test(arc4randomforktest ${CMAKE_CURRENT_SOURCE_DIR}/arc4randomforktest.sh)
endif()

# asn1evp
add_executable(asn1evp asn1evp.c)
if (MSVC)
	target_compile_definitions(asn1evp
							PRIVATE ${project_name_defines}
							)
	target_compile_options(asn1evp 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(asn1evp ${REOPENSSL_LIBS})
add_test(asn1evp asn1evp)

# asn1test
add_executable(asn1test asn1test.c)
if (MSVC)
	target_compile_definitions(asn1test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(asn1test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(asn1test ${REOPENSSL_LIBS})
add_test(asn1test asn1test)

# asn1time
add_executable(asn1time asn1time.c)
if (MSVC)
	target_compile_definitions(asn1time
							PRIVATE ${project_name_defines}
							)
	target_compile_options(asn1time 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(asn1time ${REOPENSSL_LIBS})
add_test(asn1time asn1time)

# base64test
add_executable(base64test base64test.c)
if (MSVC)
	target_compile_definitions(base64test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(base64test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(base64test ${REOPENSSL_LIBS})
add_test(base64test base64test)

# bftest
add_executable(bftest bftest.c)
if (MSVC)
	target_compile_definitions(bftest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(bftest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(bftest ${REOPENSSL_LIBS})
add_test(bftest bftest)

# biotest
# the BIO tests rely on resolver results that are OS and environment-specific
if(ENABLE_EXTRATESTS)
	add_executable(biotest biotest.c)
if (MSVC)
	target_compile_definitions(biotest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(biotest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(biotest ${REOPENSSL_LIBS})
	add_test(biotest biotest)
endif()

# bnaddsub
add_executable(bnaddsub bnaddsub.c)
if (MSVC)
	target_compile_definitions(bnaddsub
							PRIVATE ${project_name_defines}
							)
	target_compile_options(bnaddsub 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(bnaddsub ${REOPENSSL_LIBS})
add_test(bnaddsub bnaddsub)

# bn_rand_interval
if(NOT BUILD_SHARED_LIBS)
	add_executable(bn_rand_interval bn_rand_interval.c)
if (MSVC)
	target_compile_definitions(bn_rand_interval
							PRIVATE ${project_name_defines}
							)
	target_compile_options(bn_rand_interval 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(bn_rand_interval ${REOPENSSL_LIBS})
	add_test(bn_rand_interval bn_rand_interval)
endif()

# bntest
if(NOT BUILD_SHARED_LIBS)
	add_executable(bntest bntest.c)
	set_source_files_properties(bntest.c PROPERTIES COMPILE_FLAGS
		-ULIBRESSL_INTERNAL)
if (MSVC)
	target_compile_definitions(bntest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(bntest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(bntest ${REOPENSSL_LIBS})
	add_test(bntest bntest)
endif()

# bn_to_string
if(NOT BUILD_SHARED_LIBS)
	add_executable(bn_to_string bn_to_string.c)
if (MSVC)
	target_compile_definitions(bn_to_string
							PRIVATE ${project_name_defines}
							)
	target_compile_options(bn_to_string 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(bn_to_string ${REOPENSSL_LIBS})
	add_test(bn_to_string bn_to_string)
endif()

# buffertest
if(NOT BUILD_SHARED_LIBS)
	add_executable(buffertest buffertest.c)
if (MSVC)
	target_compile_definitions(buffertest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(buffertest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(buffertest ${REOPENSSL_LIBS})
	add_test(buffertest buffertest)
endif()

# bytestringtest
if(NOT BUILD_SHARED_LIBS)
	add_executable(bytestringtest bytestringtest.c)
if (MSVC)
	target_compile_definitions(bytestringtest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(bytestringtest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(bytestringtest ${REOPENSSL_LIBS})
	add_test(bytestringtest bytestringtest)
endif()

# casttest
add_executable(casttest casttest.c)
if (MSVC)
	target_compile_definitions(casttest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(casttest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(casttest ${REOPENSSL_LIBS})
add_test(casttest casttest)

# chachatest
add_executable(chachatest chachatest.c)
if (MSVC)
	target_compile_definitions(chachatest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(chachatest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(chachatest ${REOPENSSL_LIBS})
add_test(chachatest chachatest)

# cipher_list
if(NOT BUILD_SHARED_LIBS)
	add_executable(cipher_list cipher_list.c)
if (MSVC)
	target_compile_definitions(cipher_list
							PRIVATE ${project_name_defines}
							)
	target_compile_options(cipher_list 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(cipher_list ${REOPENSSL_LIBS})
	add_test(cipher_list cipher_list)
endif()

# cipherstest
add_executable(cipherstest cipherstest.c)
if (MSVC)
	target_compile_definitions(cipherstest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(cipherstest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(cipherstest ${REOPENSSL_LIBS})
add_test(cipherstest cipherstest)

# clienttest
# disabled
#add_executable(clienttest clienttest.c)
#target_link_libraries(clienttest ${REOPENSSL_LIBS})
#add_test(clienttest clienttest)

# cmstest
add_executable(cmstest cmstest.c)
if (MSVC)
	target_compile_definitions(cmstest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(cmstest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(cmstest ${REOPENSSL_LIBS})
add_test(cmstest cmstest)

# configtest
add_executable(configtest configtest.c)
if (MSVC)
	target_compile_definitions(configtest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(configtest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(configtest ${REOPENSSL_LIBS})
add_test(configtest configtest)

# cts128test
add_executable(cts128test cts128test.c)
if (MSVC)
	target_compile_definitions(cts128test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(cts128test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(cts128test ${REOPENSSL_LIBS})
add_test(cts128test cts128test)

# destest
add_executable(destest destest.c)
if (MSVC)
	target_compile_definitions(destest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(destest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(destest ${REOPENSSL_LIBS})
add_test(destest destest)

# dhtest
add_executable(dhtest dhtest.c)
if (MSVC)
	target_compile_definitions(dhtest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(dhtest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(dhtest ${REOPENSSL_LIBS})
add_test(dhtest dhtest)

# dsatest
add_executable(dsatest dsatest.c)
if (MSVC)
	target_compile_definitions(dsatest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(dsatest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(dsatest ${REOPENSSL_LIBS})
add_test(dsatest dsatest)

# ecdhtest
add_executable(ecdhtest ecdhtest.c)
if (MSVC)
	target_compile_definitions(ecdhtest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(ecdhtest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(ecdhtest ${REOPENSSL_LIBS})
add_test(ecdhtest ecdhtest)

# ecdsatest
add_executable(ecdsatest ecdsatest.c)
if (MSVC)
	target_compile_definitions(ecdsatest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(ecdsatest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(ecdsatest ${REOPENSSL_LIBS})
add_test(ecdsatest ecdsatest)

# ectest
add_executable(ectest ectest.c)
if (MSVC)
	target_compile_definitions(ectest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(ectest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(ectest ${REOPENSSL_LIBS})
add_test(ectest ectest)

# enginetest
add_executable(enginetest enginetest.c)
if (MSVC)
	target_compile_definitions(enginetest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(enginetest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(enginetest ${REOPENSSL_LIBS})
add_test(enginetest enginetest)

# evptest
add_executable(evptest evptest.c)
if (MSVC)
	target_compile_definitions(evptest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(evptest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(evptest ${REOPENSSL_LIBS})
add_test(evptest evptest ${CMAKE_CURRENT_SOURCE_DIR}/evptests.txt)

# explicit_bzero
# explicit_bzero relies on SA_ONSTACK, which is unavailable on Windows
if(NOT WIN32)
	if(HAVE_MEMMEM)
		add_executable(explicit_bzero explicit_bzero.c)
	else()
		add_executable(explicit_bzero explicit_bzero.c compat/memmem.c)
	endif()
if (MSVC)
	target_compile_definitions(explicit_bzero
							PRIVATE ${project_name_defines}
							)
	target_compile_options(explicit_bzero 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(explicit_bzero ${REOPENSSL_LIBS})
	add_test(explicit_bzero explicit_bzero)
endif()

# exptest
if(NOT BUILD_SHARED_LIBS)
	add_executable(exptest exptest.c)
	set_source_files_properties(exptest.c PROPERTIES COMPILE_FLAGS
		-ULIBRESSL_INTERNAL)
if (MSVC)
	target_compile_definitions(exptest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(exptest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(exptest ${REOPENSSL_LIBS})
	add_test(exptest exptest)
endif()

# freenull
add_executable(freenull freenull.c)
if (MSVC)
	target_compile_definitions(freenull
							PRIVATE ${project_name_defines}
							)
	target_compile_options(freenull 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(freenull ${REOPENSSL_LIBS})
add_test(freenull freenull)

# gcm128test
add_executable(gcm128test gcm128test.c)
if (MSVC)
	target_compile_definitions(gcm128test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(gcm128test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(gcm128test ${REOPENSSL_LIBS})
add_test(gcm128test gcm128test)

# gost2814789t
add_executable(gost2814789t gost2814789t.c)
if (MSVC)
	target_compile_definitions(gost2814789t
							PRIVATE ${project_name_defines}
							)
	target_compile_options(gost2814789t 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(gost2814789t ${REOPENSSL_LIBS})
add_test(gost2814789t gost2814789t)

# handshake_table
if(NOT BUILD_SHARED_LIBS)
	add_executable(handshake_table handshake_table.c)
if (MSVC)
	target_compile_definitions(handshake_table
							PRIVATE ${project_name_defines}
							)
	target_compile_options(handshake_table 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(handshake_table ${REOPENSSL_LIBS})
	add_test(handshake_table handshake_table)
endif()

# hkdf_test
add_executable(hkdf_test hkdf_test.c)
if (MSVC)
	target_compile_definitions(hkdf_test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(hkdf_test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(hkdf_test ${REOPENSSL_LIBS})
add_test(hkdf_test hkdf_test)

# hmactest
add_executable(hmactest hmactest.c)
if (MSVC)
	target_compile_definitions(hmactest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(hmactest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(hmactest ${REOPENSSL_LIBS})
add_test(hmactest hmactest)

# ideatest
add_executable(ideatest ideatest.c)
if (MSVC)
	target_compile_definitions(ideatest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(ideatest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(ideatest ${REOPENSSL_LIBS})
add_test(ideatest ideatest)

# igetest
add_executable(igetest igetest.c)
if (MSVC)
	target_compile_definitions(igetest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(igetest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(igetest ${REOPENSSL_LIBS})
add_test(igetest igetest)

# keypairtest
if(NOT BUILD_SHARED_LIBS)
	add_executable(key_schedule key_schedule.c)
if (MSVC)
	target_compile_definitions(key_schedule
							PRIVATE ${project_name_defines}
							)
	target_compile_options(key_schedule 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(key_schedule ${REOPENSSL_LIBS})
	add_test(key_schedule key_schedule)

	add_executable(keypairtest keypairtest.c)
if (MSVC)
	target_compile_definitions(keypairtest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(keypairtest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(keypairtest ${REOPENSSL_LIBS})
	add_test(keypairtest keypairtest
		${CMAKE_CURRENT_SOURCE_DIR}/ca.pem
		${CMAKE_CURRENT_SOURCE_DIR}/server.pem
		${CMAKE_CURRENT_SOURCE_DIR}/server.pem)
endif()

# md4test
add_executable(md4test md4test.c)
if (MSVC)
	target_compile_definitions(md4test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(md4test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(md4test ${REOPENSSL_LIBS})
add_test(md4test md4test)

# md5test
add_executable(md5test md5test.c)
if (MSVC)
	target_compile_definitions(md5test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(md5test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(md5test ${REOPENSSL_LIBS})
add_test(md5test md5test)

# mont
add_executable(mont mont.c)
if (MSVC)
	target_compile_definitions(mont
							PRIVATE ${project_name_defines}
							)
	target_compile_options(mont 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(mont ${REOPENSSL_LIBS})
add_test(mont mont)

# ocsp_test
if(ENABLE_EXTRATESTS)
	add_executable(ocsp_test ocsp_test.c)
if (MSVC)
	target_compile_definitions(ocsp_test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(ocsp_test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(ocsp_test ${REOPENSSL_LIBS})
	if(NOT MSVC)
		add_test(NAME ocsptest COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/ocsptest.sh)
	else()
		add_test(NAME ocsptest COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/ocsptest.bat $<TARGET_FILE:ocsp_test>)
	endif()
endif()

# optionstest
add_executable(optionstest optionstest.c)
if (MSVC)
	target_compile_definitions(optionstest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(optionstest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(optionstest ${REOPENSSL_LIBS})
add_test(optionstest optionstest)

# pbkdf2
add_executable(pbkdf2 pbkdf2.c)
if (MSVC)
	target_compile_definitions(pbkdf2
							PRIVATE ${project_name_defines}
							)
	target_compile_options(pbkdf2 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(pbkdf2 ${REOPENSSL_LIBS})
add_test(pbkdf2 pbkdf2)

# pidwraptest
# pidwraptest relies on an OS-specific way to give out pids and is generally
# awkward on systems with slow fork
if(ENABLE_EXTRATESTS AND NOT MSVC)
	add_executable(pidwraptest pidwraptest.c)
if (MSVC)
	target_compile_definitions(pidwraptest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(pidwraptest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(pidwraptest ${REOPENSSL_LIBS})
	add_test(pidwraptest ${CMAKE_CURRENT_SOURCE_DIR}/pidwraptest.sh)
endif()

# pkcs7test
add_executable(pkcs7test pkcs7test.c)
if (MSVC)
	target_compile_definitions(pkcs7test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(pkcs7test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(pkcs7test ${REOPENSSL_LIBS})
add_test(pkcs7test pkcs7test)

# poly1305test
add_executable(poly1305test poly1305test.c)
if (MSVC)
	target_compile_definitions(poly1305test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(poly1305test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(poly1305test ${REOPENSSL_LIBS})
add_test(poly1305test poly1305test)

# pq_test
if(NOT BUILD_SHARED_LIBS)
	add_executable(pq_test pq_test.c)
if (MSVC)
	target_compile_definitions(pq_test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(pq_test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(pq_test ${REOPENSSL_LIBS})
	if(NOT MSVC)
		add_test(NAME pq_test COMMAND
			${CMAKE_CURRENT_SOURCE_DIR}/pq_test.sh)
	else()
		add_test(NAME pq_test COMMAND
			${CMAKE_CURRENT_SOURCE_DIR}/pq_test.bat
			$<TARGET_FILE:pq_test>)
	endif()
	set_tests_properties(pq_test PROPERTIES ENVIRONMENT
		"srcdir=${TEST_SOURCE_DIR}")
endif()

# randtest
add_executable(randtest randtest.c)
if (MSVC)
	target_compile_definitions(randtest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(randtest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(randtest ${REOPENSSL_LIBS})
add_test(randtest randtest)

# rc2test
add_executable(rc2test rc2test.c)
if (MSVC)
	target_compile_definitions(rc2test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(rc2test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(rc2test ${REOPENSSL_LIBS})
add_test(rc2test rc2test)

# rc4test
add_executable(rc4test rc4test.c)
if (MSVC)
	target_compile_definitions(rc4test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(rc4test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(rc4test ${REOPENSSL_LIBS})
add_test(rc4test rc4test)

# recordtest
if(NOT BUILD_SHARED_LIBS)
	add_executable(recordtest recordtest.c)
if (MSVC)
	target_compile_definitions(recordtest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(recordtest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(recordtest ${REOPENSSL_LIBS})
	add_test(recordtest recordtest)
endif()

# record_layer_test
if(NOT BUILD_SHARED_LIBS)
	add_executable(record_layer_test record_layer_test.c)
if (MSVC)
	target_compile_definitions(record_layer_test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(record_layer_test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(record_layer_test ${REOPENSSL_LIBS})
	add_test(record_layer_test record_layer_test)
endif()

# rfc5280time
add_executable(rfc5280time rfc5280time.c)
if (MSVC)
	target_compile_definitions(rfc5280time
							PRIVATE ${project_name_defines}
							)
	target_compile_options(rfc5280time 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(rfc5280time ${REOPENSSL_LIBS})
if(SMALL_TIME_T)
	add_test(rfc5280time ${CMAKE_CURRENT_SOURCE_DIR}/rfc5280time_small.test)
else()
	add_test(rfc5280time rfc5280time)
endif()

# rmdtest
add_executable(rmdtest rmdtest.c)
if (MSVC)
	target_compile_definitions(rmdtest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(rmdtest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(rmdtest ${REOPENSSL_LIBS})
add_test(rmdtest rmdtest)

# rsa_test
add_executable(rsa_test rsa_test.c)
if (MSVC)
	target_compile_definitions(rsa_test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(rsa_test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(rsa_test ${REOPENSSL_LIBS})
add_test(rsa_test rsa_test)

# servertest
add_executable(servertest servertest.c)
if (MSVC)
	target_compile_definitions(servertest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(servertest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(servertest ${REOPENSSL_LIBS})
if(NOT MSVC)
	add_test(NAME servertest COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/servertest.sh)
else()
	add_test(NAME servertest COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/servertest.bat $<TARGET_FILE:servertest>)
endif()
set_tests_properties(servertest PROPERTIES ENVIRONMENT "srcdir=${TEST_SOURCE_DIR}")

# sha1test
add_executable(sha1test sha1test.c)
if (MSVC)
	target_compile_definitions(sha1test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(sha1test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(sha1test ${REOPENSSL_LIBS})
add_test(sha1test sha1test)

# sha256test
add_executable(sha256test sha256test.c)
if (MSVC)
	target_compile_definitions(sha256test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(sha256test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(sha256test ${REOPENSSL_LIBS})
add_test(sha256test sha256test)

# sha512test
add_executable(sha512test sha512test.c)
if (MSVC)
	target_compile_definitions(sha512test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(sha512test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(sha512test ${REOPENSSL_LIBS})
add_test(sha512test sha512test)

# sm3test
add_executable(sm3test sm3test.c)
if (MSVC)
	target_compile_definitions(sm3test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(sm3test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(sm3test ${REOPENSSL_LIBS})
add_test(sm3test sm3test)

# sm4test
add_executable(sm4test sm4test.c)
if (MSVC)
	target_compile_definitions(sm4test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(sm4test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(sm4test ${REOPENSSL_LIBS})
add_test(sm4test sm4test)

# ssl_versions
if(NOT BUILD_SHARED_LIBS)
	add_executable(ssl_versions ssl_versions.c)
if (MSVC)
	target_compile_definitions(ssl_versions
							PRIVATE ${project_name_defines}
							)
	target_compile_options(ssl_versions 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(ssl_versions ${REOPENSSL_LIBS})
	add_test(ssl_versions ssl_versions)
endif()

# ssltest
add_executable(ssltest ssltest.c)
if (MSVC)
	target_compile_definitions(ssltest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(ssltest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(ssltest ${REOPENSSL_LIBS})
if(NOT MSVC)
	add_test(NAME ssltest COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/ssltest.sh)
else()
	add_test(NAME ssltest COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/ssltest.bat $<TARGET_FILE:ssltest> $<TARGET_FILE:openssl>)
endif()
set_tests_properties(ssltest PROPERTIES ENVIRONMENT "srcdir=${TEST_SOURCE_DIR}")

# testdsa
if(NOT MSVC)
	add_test(NAME testdsa COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/testdsa.sh)
else()
	add_test(NAME testdsa COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/testdsa.bat $<TARGET_FILE:openssl>)
endif()
set_tests_properties(testdsa PROPERTIES ENVIRONMENT "srcdir=${TEST_SOURCE_DIR}")

# testenc
if(NOT MSVC)
	add_test(NAME testenc COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/testenc.sh)
else()
	add_test(NAME testenc COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/testenc.bat $<TARGET_FILE:openssl>)
endif()
set_tests_properties(testenc PROPERTIES ENVIRONMENT "srcdir=${TEST_SOURCE_DIR}")

# testrsa
if(NOT MSVC)
	add_test(NAME testrsa COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/testrsa.sh)
else()
	add_test(NAME testrsa COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/testrsa.bat $<TARGET_FILE:openssl>)
endif()
set_tests_properties(testrsa PROPERTIES ENVIRONMENT "srcdir=${TEST_SOURCE_DIR}")

# timingsafe
add_executable(timingsafe timingsafe.c)
if (MSVC)
	target_compile_definitions(timingsafe
							PRIVATE ${project_name_defines}
							)
	target_compile_options(timingsafe 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(timingsafe ${REOPENSSL_LIBS})
add_test(timingsafe timingsafe)

# tlsexttest
if(NOT BUILD_SHARED_LIBS)
	add_executable(tlsexttest tlsexttest.c)
if (MSVC)
	target_compile_definitions(tlsexttest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(tlsexttest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(tlsexttest ${REOPENSSL_LIBS})
	add_test(tlsexttest tlsexttest)
endif()

# tlstest
set(TLSTEST_SRC tlstest.c)
check_function_exists(pipe2 HAVE_PIPE2)
if(HAVE_PIPE2)
	add_definitions(-DHAVE_PIPE2)
else()
	set(TLSTEST_SRC ${TLSTEST_SRC} compat/pipe2.c)
endif()

add_executable(tlstest ${TLSTEST_SRC})
if (MSVC)
	target_compile_definitions(tlstest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(tlstest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(tlstest ${REOPENSSL_LIBS})
if(NOT MSVC)
	add_test(NAME tlstest COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/tlstest.sh)
else()
	add_test(NAME tlstest COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/tlstest.bat $<TARGET_FILE:tlstest>)
endif()
set_tests_properties(tlstest PROPERTIES ENVIRONMENT "srcdir=${TEST_SOURCE_DIR}")

# tls_ext_alpn
if(NOT BUILD_SHARED_LIBS)
	add_executable(tls_ext_alpn tls_ext_alpn.c)
if (MSVC)
	target_compile_definitions(tls_ext_alpn
							PRIVATE ${project_name_defines}
							)
	target_compile_options(tls_ext_alpn 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(tls_ext_alpn ${REOPENSSL_LIBS})
	add_test(tls_ext_alpn tls_ext_alpn)
endif()

# tls_prf
if(NOT BUILD_SHARED_LIBS)
	add_executable(tls_prf tls_prf.c)
if (MSVC)
	target_compile_definitions(tls_prf
							PRIVATE ${project_name_defines}
							)
	target_compile_options(tls_prf 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(tls_prf ${REOPENSSL_LIBS})
	add_test(tls_prf tls_prf)
endif()

# utf8test
if(NOT BUILD_SHARED_LIBS)
	add_executable(utf8test utf8test.c)
if (MSVC)
	target_compile_definitions(utf8test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(utf8test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(utf8test ${REOPENSSL_LIBS})
	add_test(utf8test utf8test)
endif()

# valid_handshakes_terminate
if(NOT BUILD_SHARED_LIBS)
	add_executable(valid_handshakes_terminate valid_handshakes_terminate.c)
if (MSVC)
	target_compile_definitions(valid_handshakes_terminate
							PRIVATE ${project_name_defines}
							)
	target_compile_options(valid_handshakes_terminate 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(valid_handshakes_terminate ${REOPENSSL_LIBS})
	add_test(valid_handshakes_terminate valid_handshakes_terminate)
endif()

# verifytest
if(NOT BUILD_SHARED_LIBS)
	add_executable(verifytest verifytest.c)
if (MSVC)
	target_compile_definitions(verifytest
							PRIVATE ${project_name_defines}
							)
	target_compile_options(verifytest 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
	target_link_libraries(verifytest ${REOPENSSL_LIBS})
	add_test(verifytest verifytest)
endif()

# x25519test
add_executable(x25519test x25519test.c)
if (MSVC)
	target_compile_definitions(x25519test
							PRIVATE ${project_name_defines}
							)
	target_compile_options(x25519test 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(x25519test ${REOPENSSL_LIBS})
add_test(x25519test x25519test)

# x509name
add_executable(x509name x509name.c)
if (MSVC)
	target_compile_definitions(x509name
							PRIVATE ${project_name_defines}
							)
	target_compile_options(x509name 
							PRIVATE ${project_name_cflags}
	)
endif(MSVC)
target_link_libraries(x509name ${REOPENSSL_LIBS})
add_test(x509name x509name)

#if(BUILD_SHARED_LIBS)
#	add_custom_command(TARGET x25519test POST_BUILD
#		COMMAND "${CMAKE_COMMAND}" -E copy
#		"$<TARGET_FILE:tls>"
#		"$<TARGET_FILE:ssl>"
#		"$<TARGET_FILE:crypto>"
#		"${CMAKE_CURRENT_BINARY_DIR}"
#		COMMENT "Copying DLLs for regression tests")
#endif()

