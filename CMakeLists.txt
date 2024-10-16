#-----------------------------------------------------------------------------------------
# ya_getopt  - Yet another getopt
# https://github.com/kubo/ya_getopt
#
# Copyright 2015 Kubo Takehiro <kubo@jiubao.org>
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
project (ya_getopt
	LANGUAGES C
	HOMEPAGE_URL https://github.com/dakka/ya_getopt
	DESCRIPTION "Yet another getopt"
	VERSION 1.2.0
)

set(name ya_getopt)
add_library(objlib OBJECT ${name}.c)
set_property(TARGET objlib PROPERTY POSITION_INDEPENDENT_CODE 1)
add_library(${name} SHARED $<TARGET_OBJECTS:objlib>)
add_library(${name}static STATIC $<TARGET_OBJECTS:objlib>)
set_target_properties(${name}static PROPERTIES OUTPUT_NAME ${name})

#add_executable(b2 b2.cpp ${deps}/ya_getopt-src/ya_getopt.c)
#set_target_properties(b2 PROPERTIES CXX_STANDARD 20 CXX_STANDARD_REQUIRED true)
#string(TIMESTAMP B2_COPYRIGHT_YEAR "%y")
#configure_file(b2.h.in b2.h @ONLY)
#target_include_directories(b2 PRIVATE ${CMAKE_CURRENT_BINARY_DIR} ${deps}/sndfile-build ${deps}/ya_getopt-src)
#target_link_libraries(b2 PRIVATE sndfile)
