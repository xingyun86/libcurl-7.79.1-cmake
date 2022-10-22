# CMakeList.txt : Top-level CMake project file, do global configuration
# and include sub-projects here.
#
cmake_minimum_required (VERSION 3.8)

project ("CryptTools")

set(ADMIN_LEVEL "/MANIFESTUAC:\"level='requireAdministrator' uiAccess='false'\"")

# Include sub-projects.
add_subdirectory ("zlib-1.2.11")
add_subdirectory ("cares-1.16.0")
add_subdirectory ("libressl-3.1.1")
add_subdirectory ("libssh2-1.9.0")
add_subdirectory ("libcurl-7.79.1")
