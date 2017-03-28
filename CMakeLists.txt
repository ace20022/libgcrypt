cmake_minimum_required(VERSION 3.0)

project(libgcrypt VERSION 1.7.6 LANGUAGES C)

#find_package(libpgp-error 1.2.7 REQUIRED NO_MODULE)

if(MSVC)
  set(CMAKE_DEBUG_POSTFIX "d")
endif()

if (EXISTS  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/mips3/mpi-asm-defs.h)
  file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/mpi/mips3/mpi-asm-defs.h 
       DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}/mpi
	   ) 

else()
  message("Could not find mpi-asm-defs.h")
  return()
endif()

set(SRCS
  ${CMAKE_CURRENT_SOURCE_DIR}/cmake/libgcrypt.def
  ${CMAKE_CURRENT_SOURCE_DIR}/cmake/gcrypt.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cmake/version.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cmake/config.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cmake/gpg-error.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cmake/headers_fixup.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cmake/mod-source-info.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cmake/gost-sb.h
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/mpi-asm-defs.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/context.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/fips.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/global.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/hmac256.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/hwf-x86.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/hwfeatures.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/misc.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/missing-string.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/secmem.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/sexp.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/stdmem.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/visibility.c
  ${CMAKE_CURRENT_SOURCE_DIR}/src/cipher-proto.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/cipher.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/context.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/ec-context.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/g10lib.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/gcrypt-int.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/hmac256.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/hwf-common.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/mpi.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/secmem.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/stdmem.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/types.h
  ${CMAKE_CURRENT_SOURCE_DIR}/src/visibility.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/arcfour.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/blowfish.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/camellia-glue.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/camellia.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/cast5.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/chacha20.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/cipher-aeswrap.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/cipher-cbc.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/cipher-ccm.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/cipher-cfb.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/cipher-cmac.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/cipher-ctr.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/cipher-gcm-intel-pclmul.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/cipher-gcm.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/cipher-ocb.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/cipher-ofb.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/cipher-poly1305.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/cipher-selftest.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/cipher.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/crc-intel-pclmul.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/crc.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/des.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/dsa-common.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/dsa.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/ecc-curves.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/ecc-ecdsa.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/ecc-eddsa.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/ecc-gost.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/ecc-misc.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/ecc.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/elgamal.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/gost-s-box.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/gost28147.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/gostr3411-94.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/hash-common.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/hmac-tests.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/idea.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/kdf.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/keccak.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/mac-cmac.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/mac-gmac.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/mac-hmac.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/mac-poly1305.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/mac.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/md.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/md4.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/md5.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/poly1305.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/primegen.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/pubkey-util.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/pubkey.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/rfc2268.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/rijndael-aesni.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/rijndael-armv8-ce.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/rijndael-padlock.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/rijndael-ssse3-amd64.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/rijndael.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/rmd160.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/rsa-common.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/rsa.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/salsa20.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/scrypt.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/seed.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/serpent.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/sha1.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/sha256.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/sha512.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/stribog.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/tiger.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/twofish.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/whirlpool.c
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/bithelp.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/bufhelp.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/camellia.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/cipher-internal.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/cipher-selftest.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/ecc-common.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/gost.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/hash-common.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/kdf-internal.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/keccak_permute_32.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/keccak_permute_64.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/mac-internal.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/poly1305-internal.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/pubkey-internal.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/rijndael-internal.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/rijndael-tables.h
  ${CMAKE_CURRENT_SOURCE_DIR}/cipher/sha1.h
  ${CMAKE_CURRENT_SOURCE_DIR}/compat/compat.c
  ${CMAKE_CURRENT_SOURCE_DIR}/compat/libcompat.h
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/ec-ed25519.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/ec.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/generic/mpih-add1.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/generic/mpih-lshift.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/generic/mpih-mul1.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/generic/mpih-mul2.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/generic/mpih-mul3.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/generic/mpih-rshift.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/generic/mpih-sub1.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/generic/udiv-w-sdiv.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/mpi-add.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/mpi-bit.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/mpi-cmp.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/mpi-div.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/mpi-gcd.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/mpi-inline.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/mpi-inv.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/mpi-mod.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/mpi-mpow.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/mpi-mul.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/mpi-pow.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/mpi-scan.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/mpicoder.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/mpih-div.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/mpih-mul.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/mpiutil.c
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/ec-internal.h
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/longlong.h
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/mpi-inline.h
  ${CMAKE_CURRENT_SOURCE_DIR}/mpi/mpi-internal.h
  ${CMAKE_CURRENT_SOURCE_DIR}/random/random-csprng.c
  ${CMAKE_CURRENT_SOURCE_DIR}/random/random-drbg.c
  ${CMAKE_CURRENT_SOURCE_DIR}/random/random-system.c
  ${CMAKE_CURRENT_SOURCE_DIR}/random/random.c
  ${CMAKE_CURRENT_SOURCE_DIR}/random/rndhw.c
  ${CMAKE_CURRENT_SOURCE_DIR}/random/rndw32.c
  ${CMAKE_CURRENT_SOURCE_DIR}/random/rand-internal.h
  ${CMAKE_CURRENT_SOURCE_DIR}/random/random.h
)

ADD_LIBRARY(libgcrypt SHARED ${SRCS})
SET_TARGET_PROPERTIES(libgcrypt PROPERTIES LINK_FLAGS "/DEF:\"libgcrypt.def\"")
target_link_libraries(libgcrypt PRIVATE "D:/Code/libpgp-error/build/Debug/libpgp-errord.lib")

target_compile_definitions(libgcrypt
  PRIVATE
  HAVE_CONFIG_H
  _CRT_SECURE_NO_WARNINGS
  _CRT_NONSTDC_NO_WARNINGS
  __i386__
  __builtin_bswap32=_byteswap_ulong
  __builtin_bswap64=_byteswap_uint64
  asm=__asm__
)

target_include_directories(libgcrypt
  PRIVATE
  $<BUILD_INTERFACE:.;cmake;src;mpi;${CMAKE_CURRENT_BINARY_DIR}>
  INTERFACE
  $<INSTALL_INTERFACE:include/libgcrypt>
)

