#
# SPDX-License-Identifier: BSD-3-Clause
# Copyright 2021, Intel Corporation
#

add_cstyle(src
	${CMAKE_CURRENT_SOURCE_DIR}/*.[ch]
	${CMAKE_CURRENT_SOURCE_DIR}/include/*.[h]
	${CMAKE_CURRENT_SOURCE_DIR}/windows/include/*.[ch]
	)

add_check_whitespace(src
	${CMAKE_CURRENT_SOURCE_DIR}/*.[ch]
	${CMAKE_CURRENT_SOURCE_DIR}/include/*.[h]
	${CMAKE_CURRENT_SOURCE_DIR}/windows/include/*.[ch]
	)

set(SOURCES
    future.c
    mover.c
    runtime.c
)

if(WIN32)
	set(SOURCES ${SOURCES} os_thread_windows.c)
	set(SOURCES ${SOURCES} os_windows.c)
else()
	set(SOURCES ${SOURCES} os_thread_posix.c)
	set(SOURCES ${SOURCES} os_posix.c)
endif()

add_library(uasync SHARED ${SOURCES})

target_include_directories(uasync PRIVATE . include)
if(WIN32)
	target_include_directories(uasync PRIVATE src/windows/include/sys)
	target_include_directories(uasync PRIVATE windows/include)
endif()


set_target_properties(uasync PROPERTIES
	PUBLIC_HEADER ${LIBUASYNC_INCLUDE_DIR}/libuasync.h
)
