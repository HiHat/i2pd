cmake_minimum_required(VERSION 3.10)
project(i2pd_tests)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(OpenSSL REQUIRED)
find_package(ZLIB REQUIRED)
find_package(Threads REQUIRED)

enable_testing()
find_package(PkgConfig REQUIRED)
pkg_check_modules(CHECK REQUIRED check)
include_directories(${CHECK_INCLUDE_DIRS})
link_directories(${CHECK_LIBRARY_DIRS})

# Compiler flags:
if(APPLE)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O0 -g -Wl,-undefined,dynamic_lookup")
else()
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O0 -g -Wl,--unresolved-symbols=ignore-in-object-files")
endif()

set(TEST_PATH ${CMAKE_CURRENT_BINARY_DIR})

include_directories(
  ../libi2pd
  ${Boost_INCLUDE_DIRS}
  ${OPENSSL_INCLUDE_DIR}
)

set(test-http-merge_chunked_SRCS
  test-http-merge_chunked.cpp
)

set(test-http-req_SRCS
  test-http-req.cpp
)

set(test-http-res_SRCS
  test-http-res.cpp
)

set(test-http-url_decode_SRCS
  test-http-url_decode.cpp
)

set(test-http-url_SRCS
  test-http-url.cpp
)

set(test-base-64_SRCS
  test-base-64.cpp
)

set(test-gost_SRCS
  test-gost.cpp
)

set(test-gost-sig_SRCS
  test-gost-sig.cpp
)

set(test-aeadchacha20poly1305_SRCS
  test-aeadchacha20poly1305.cpp
)

set(test-blinding_SRCS
  test-blinding.cpp
)

SET(test-elligator_SRCS
  test-elligator.cpp
)

set(test-eddsa_SRCS
  test-eddsa.cpp
)

set(test-aes_SRCS
  test-aes.cpp
)

add_executable(test-http-merge_chunked ${test-http-merge_chunked_SRCS})
add_executable(test-http-req ${test-http-req_SRCS})
add_executable(test-http-res ${test-http-res_SRCS})
add_executable(test-http-url_decode ${test-http-url_decode_SRCS})
add_executable(test-http-url ${test-http-url_SRCS})
add_executable(test-base-64 ${test-base-64_SRCS})
add_executable(test-gost ${test-gost_SRCS})
add_executable(test-gost-sig ${test-gost-sig_SRCS})
add_executable(test-aeadchacha20poly1305 ${test-aeadchacha20poly1305_SRCS})
add_executable(test-blinding ${test-blinding_SRCS})
add_executable(test-elligator ${test-elligator_SRCS})
add_executable(test-eddsa ${test-eddsa_SRCS})
add_executable(test-aes ${test-aes_SRCS})

set(LIBS
  libi2pd
  ${Boost_LIBRARIES}
  OpenSSL::SSL
  OpenSSL::Crypto
  ZLIB::ZLIB
  Threads::Threads
  ${CHECK_LDFLAGS}
  ${CMAKE_REQUIRED_LIBRARIES}
)

set(TEST_TARGETS
  test-http-merge_chunked
  test-http-req
  test-http-res
  test-http-url_decode
  test-http-url
  test-base-64
  test-gost
  test-gost-sig
  test-aeadchacha20poly1305
  test-blinding
  test-elligator
  test-eddsa
  test-aes
)

foreach(test_target ${TEST_TARGETS})
  target_link_libraries(${test_target} PRIVATE ${LIBS})
endforeach()

add_test(test-http-merge_chunked ${TEST_PATH}/test-http-merge_chunked)
add_test(test-http-req ${TEST_PATH}/test-http-req)
add_test(test-http-res ${TEST_PATH}/test-http-res)
add_test(test-http-url_decode ${TEST_PATH}/test-http-url_decode)
add_test(test-http-url ${TEST_PATH}/test-http-url)
add_test(test-base-64 ${TEST_PATH}/test-base-64)
add_test(test-gost ${TEST_PATH}/test-gost)
add_test(test-gost-sig ${TEST_PATH}/test-gost-sig)
add_test(test-aeadchacha20poly1305 ${TEST_PATH}/test-aeadchacha20poly1305)
add_test(test-blinding ${TEST_PATH}/test-blinding)
add_test(test-elligator ${TEST_PATH}/test-elligator)
add_test(test-eddsa ${TEST_PATH}/test-eddsa)
add_test(test-aes ${TEST_PATH}/test-aes)

# Add the directory containing libi2pd.a as a link directory
link_directories(${CMAKE_SOURCE_DIR}/../build)
