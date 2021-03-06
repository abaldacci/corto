cmake_minimum_required(VERSION 3.1)
project(corto)

SET(CORTO_SOURCE_PATH src)

SET(LIB_HEADERS
	${CORTO_SOURCE_PATH}/bitstream.h
	${CORTO_SOURCE_PATH}/color_attribute.h
	${CORTO_SOURCE_PATH}/corto.h
	${CORTO_SOURCE_PATH}/cstream.h
	${CORTO_SOURCE_PATH}/decoder.h
	${CORTO_SOURCE_PATH}/encoder.h
	${CORTO_SOURCE_PATH}/index_attribute.h
	${CORTO_SOURCE_PATH}/normal_attribute.h
	${CORTO_SOURCE_PATH}/point.h
	${CORTO_SOURCE_PATH}/tunstall.h
	${CORTO_SOURCE_PATH}/vertex_attribute.h
	${CORTO_SOURCE_PATH}/zpoint.h)

SET(LIB_SOURCES
	${CORTO_SOURCE_PATH}/bitstream.cpp
	${CORTO_SOURCE_PATH}/color_attribute.cpp
	${CORTO_SOURCE_PATH}/cstream.cpp
	${CORTO_SOURCE_PATH}/decoder.cpp
	${CORTO_SOURCE_PATH}/encoder.cpp
	${CORTO_SOURCE_PATH}/normal_attribute.cpp
	${CORTO_SOURCE_PATH}/tunstall.cpp)

SET(EXE_SOURCE
	${CORTO_SOURCE_PATH}/main.cpp
	${CORTO_SOURCE_PATH}/meshloader.cpp
	${CORTO_SOURCE_PATH}/tinyply.cpp)

SET(EXE_HEADERS
	${CORTO_SOURCE_PATH}/meshloader.h
	${CORTO_SOURCE_PATH}/tinyply.h
	${CORTO_SOURCE_PATH}/objload.h)

ADD_LIBRARY(cortolib STATIC ${LIB_SOURCES} ${LIB_HEADERS})

SET(CORTO_DEFINITIONS "") 

if(MSVC)
	SET(CORTO_CFLAGS /nologo /W3)
	LIST(APPEND CORTO_DEFINITIONS _CRT_SECURE_NO_DEPRECATE)
else()
	SET(CORTO_CFLAGS -O2 -W -Wall -c -std=c++11 -pedantic)
endif()

SET(CORTOEXE_CFLAGS ${CORTO_CFLAGS})
SET(CORTOLIB_CFLAGS ${CORTO_CFLAGS})

if("${CMAKE_SYSTEM_NAME}" MATCHES "Linux")
	LIST(APPEND CORTOLIB_CFLAGS -fPIC)
elseif(APPLE)
	LIST(APPEND CORTOLIB_CFLAGS -fvisibility=hidden)
	if(IOS)
		LIST(APPEND CORTOLIB_CFLAGS -fembed-bitcode)
	endif(IOS)
endif()

target_include_directories(cortolib PUBLIC ${CORTO_SOURCE_PATH})
target_compile_definitions(cortolib PUBLIC ${CORTO_DEFINITIONS})
target_compile_options    (cortolib PUBLIC ${CORTOLIB_CFLAGS})
set_target_properties     (cortolib PROPERTIES DEBUG_POSTFIX "d")
set_target_properties     (cortolib PROPERTIES OUTPUT_NAME "corto")

INSTALL(TARGETS cortolib
	RUNTIME DESTINATION bin
	LIBRARY DESTINATION lib
	ARCHIVE DESTINATION lib)

INSTALL(FILES
	${CORTO_SOURCE_PATH}/bitstream.h
	${CORTO_SOURCE_PATH}/color_attribute.h
	${CORTO_SOURCE_PATH}/corto.h
	${CORTO_SOURCE_PATH}/cstream.h
	${CORTO_SOURCE_PATH}/decoder.h
	${CORTO_SOURCE_PATH}/encoder.h
	${CORTO_SOURCE_PATH}/index_attribute.h
	${CORTO_SOURCE_PATH}/normal_attribute.h
	${CORTO_SOURCE_PATH}/point.h
	${CORTO_SOURCE_PATH}/tunstall.h
	${CORTO_SOURCE_PATH}/vertex_attribute.h
	${CORTO_SOURCE_PATH}/zpoint.h
	DESTINATION include/corto)
