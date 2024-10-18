#-----------------------------------------------------------------------------------------
# ya_getopt  - Yet another getopt
# https://github.com/kubo/ya_getopt
#
# Copyright 2015 Kubo Takehiro <kubo@jiubao.org>
# Copyright (C) 2024 David L. Dight [https://github.com/dakka]
#
# Redistribution and use in source and binary forms, with or without modification, are
# permitted provided that the following conditions are met:
#
#    1. Redistributions of source code must retain the above copyright notice, this list of
#       conditions and the following disclaimer.
#
#    2. Redistributions in binary form must reproduce the above copyright notice, this list
#       of conditions and the following disclaimer in the documentation and/or other materials
#       provided with the distribution.
#
# THIS SOFTWARE IS PROVIDED BY THE AUTHORS ''AS IS'' AND ANY EXPRESS OR IMPLIED
# WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND
# FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL <COPYRIGHT HOLDER> OR
# CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
# CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR
# SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON
# ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
# NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF
# ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.
#
# The views and conclusions contained in the software and documentation are those of the
# authors and should not be interpreted as representing official policies, either expressed
# or implied, of the authors.
#
# min cmake version 3.20 (Mar 24, 2021)
#----------------------------------------------------------------------------------------
cmake_minimum_required (VERSION 3.20)
set(name ya_getopt)
project (${name}
	LANGUAGES C
	HOMEPAGE_URL https://github.com/dakka/${name}
	DESCRIPTION "Yet another getopt"
	VERSION 1.2.0
)
set(srcdir ${${name}_SOURCE_DIR})
set(lib ${name}static)
add_library(objlib OBJECT ${name}.c)
set_target_properties(objlib PROPERTIES POSITION_INDEPENDENT_CODE 1)
add_library(${lib} STATIC $<TARGET_OBJECTS:objlib>)
set_target_properties(${lib} PROPERTIES OUTPUT_NAME ${name})
add_library(${name} SHARED $<TARGET_OBJECTS:objlib>)

if(LINUX) # true for macOS
	foreach(x getopt_test getopt_long_test getopt_long_only_test)
		foreach(xt ${x} ya_${x})
			add_executable(${xt} ${srcdir}/test/${x}.c)
			target_link_libraries(${xt} PRIVATE ${lib})
			if (${xt} MATCHES "ya_*")
				target_compile_definitions(${xt} PUBLIC USE_YA_GETOPT USE_CMAKE)
				target_include_directories(${xt} PRIVATE ${srcdir})
			endif()
		endforeach()
	endforeach()
	add_test(NAME default COMMAND ${srcdir}/test/tests_on_linux.sh)
	enable_testing()
	file(GLOB OUTPUTFILES "${CMAKE_CURRENT_BINARY_DIR}/*.out")
	set_property(TARGET getopt_test PROPERTY ADDITIONAL_CLEAN_FILES ${OUTPUTFILES})
endif()
