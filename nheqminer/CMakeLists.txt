#cmake global
cmake_minimum_required(VERSION 2.8)
project(nheqminer)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

# LINUX
if(CMAKE_COMPILER_IS_GNUCXX)
    #endian
    add_definitions(-DHAVE_ENDIAN_H)
    add_definitions(-DHAVE_DECL_HTOBE16)
    add_definitions(-DHAVE_DECL_HTOLE16)
    add_definitions(-DHAVE_DECL_BE16TOH)
    add_definitions(-DHAVE_DECL_LE16TOH)
    add_definitions(-DHAVE_DECL_HTOBE32)
    add_definitions(-DHAVE_DECL_HTOLE32)
    add_definitions(-DHAVE_DECL_BE32TOH)
    add_definitions(-DHAVE_DECL_LE32TOH)
    add_definitions(-DHAVE_DECL_HTOBE64)
    add_definitions(-DHAVE_DECL_HTOLE64)
    add_definitions(-DHAVE_DECL_BE64TOH)
    add_definitions(-DHAVE_DECL_LE64TOH)
    #byteswap
    add_definitions(-DHAVE_BYTESWAP_H)
    add_definitions(-DHAVE_DECL_BSWAP_16)
    add_definitions(-DHAVE_DECL_BSWAP_32)
    add_definitions(-DHAVE_DECL_BSWAP_64)
endif()

# Common
include_directories(${nheqminer_SOURCE_DIR})

# CPU extension options
#add_definitions(-DHAVE_SSSE2)
#add_definitions(-DHAVE_SSSE3)
#add_definitions(-DHAVE_SSE41)
#add_definitions(-DHAVE_AVX)
#add_definitions(-DHAVE_AVX2)


add_definitions(-DBOOST_ALL_NO_LIB -DBOOST_ALL_DYN_LINK -DBOOST_LOG_DYN_LINK)

find_package(Threads REQUIRED COMPONENTS)
find_package(Boost REQUIRED COMPONENTS system log_setup log date_time filesystem thread)

if (Boost_FOUND)
  # From the offical documentation:
  # Add include directories to the build. [...] If the SYSTEM option is given,
  # the compiler will be told the directories are meant as system include
  # directories on some platforms (signalling this setting might achieve effects
  # such as the compiler skipping warnings [...])."
  include_directories (SYSTEM ${Boost_INCLUDE_DIR})

  # From the offical documentation:
  # "Specify directories in which the linker will look for libraries. [...] Note
  # that this command is rarely necessary. Library locations returned by
  # find_package() and find_library() are absolute paths. Pass these absolute
  # library file paths directly to the target_link_libraries() command. CMake
  # will ensure the linker finds them."
  link_directories (${Boost_LIBRARY_DIRS})
else()
    message("Boost_FOUND NOT FOUND")
endif ()




set(SOURCE_FILES
# SOURCES
    main.cpp
    amount.cpp
    api.cpp
    arith_uint256.cpp
    compat/strnlen.cpp
    crypto/ripemd160.cpp
    crypto/sha256.cpp
    json/json_spirit_reader.cpp
    json/json_spirit_value.cpp
    json/json_spirit_writer.cpp
    libstratum/StratumClient.cpp
    libstratum/ZcashStratum.cpp
    primitives/block.cpp
    speed.cpp
    trompequihash/blake2/blake2bx.cpp
    uint256.cpp
    utilstrencodings.cpp

# HEADERS
    amount.h
    api.hpp
    arith_uint256.h
    blake2/blake2-impl.h
    blake2/blake2.h
    blake2/blamka-round-opt.h
    blake2/blamka-round-ref.h
    compat/byteswap.h
    compat/endian.h
    compat/sanity.h
    crypto/common.h
    crypto/equihash.h
    crypto/ripemd160.h
    crypto/sha256.h
    hash.h
    json/json_spirit.h
    json/json_spirit_error_position.h
    json/json_spirit_reader.h
    json/json_spirit_reader_template.h
    json/json_spirit_stream_reader.h
    json/json_spirit_utils.h
    json/json_spirit_value.h
    json/json_spirit_writer.h
    json/json_spirit_writer_template.h
    libstratum/StratumClient.h
    libstratum/ZcashStratum.h
    primitives/block.h
    primitives/transaction.h
    script/script.h
    serialize.h
    speed.hpp
    streams.h
    support/allocators/zeroafterfree.h
    tinyformat.h
    trompequihash/blake2/blake2-config.h
    trompequihash/blake2/blake2-impl.h
    trompequihash/blake2/blake2-round.h
    trompequihash/blake2/blake2.h
    trompequihash/blake2/blake2b-load-sse2.h
    trompequihash/blake2/blake2b-load-sse41.h
    trompequihash/blake2/blake2b-round.h
    trompequihash/equi.h
    trompequihash/equi_miner.h
    trompequihash/equi_miner2.h
    trompequihash/pthreads/pthread.h
    uint252.h
    uint256.h
    utilstrencodings.h
    version.h
    zcash/JoinSplit.hpp
    zcash/NoteEncryption.hpp
    zcash/Proof.hpp
    zcash/Zcash.h
    )
#add_executable(${PROJECT_NAME} ${SRC_LIST})

set(LIBS ${LIBS} ${Threads_LIBRARIES} ${Boost_LIBRARIES})

#message("-- CXXFLAGS: ${CMAKE_CXX_FLAGS}")
#message("-- LIBS: ${LIBS}")

add_executable(${PROJECT_NAME} ${SOURCE_FILES})
#target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES})
target_link_libraries(${PROJECT_NAME} ${LIBS})