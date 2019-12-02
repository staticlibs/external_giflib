# Copyright 2016, akashche at redhat.com
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

cmake_minimum_required ( VERSION 2.8.12 )
project ( gif C )

if ( NOT DEFINED ${PROJECT_NAME}_LOOKASIDE_DIR )
    set ( ${PROJECT_NAME}_LOOKASIDE_DIR ${CMAKE_CURRENT_LIST_DIR}/../../../lookaside_giflib CACHE INTERNAL "" )
endif ( )

# standalone build
if ( NOT DEFINED STATICLIB_TOOLCHAIN )
    if ( NOT DEFINED ${PROJECT_NAME}_TOOLCHAIN )
        if ( WIN32 )
            if ( CMAKE_SIZEOF_VOID_P EQUAL 8 )
                set ( ${PROJECT_NAME}_TOOLCHAIN "windows_amd64_msvc" )
            else ( )
                set ( ${PROJECT_NAME}_TOOLCHAIN "windows_i386_msvc" )
            endif ( )
        elseif ( APPLE )
            set ( ${PROJECT_NAME}_TOOLCHAIN "macosx_amd64_clang" )
        elseif ( UNIX )
            set ( ${PROJECT_NAME}_TOOLCHAIN "linux_amd64_gcc" )
        else ( )
            message ( FATAL_ERROR "Cannot determine toolchain to use, please specify [${PROJECT_NAME}_TOOLCHAIN]" )
        endif ( )
    endif ( )
else ( )
    set ( ${PROJECT_NAME}_TOOLCHAIN ${STATICLIB_TOOLCHAIN} )
endif ( )

# library
set ( ${PROJECT_NAME}_HEADERS
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/lib/gif_lib.h
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/lib/gif_hash.h
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/lib/gif_lib_private.h )
set ( ${PROJECT_NAME}_SRC
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/lib/dgif_lib.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/lib/egif_lib.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/lib/gif_err.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/lib/gif_font.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/lib/gifalloc.c
        #${${PROJECT_NAME}_LOOKASIDE_DIR}/lib/openbsd-reallocarray.c
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/lib/quantize.c )
if ( ${CMAKE_C_COMPILER_ID}x MATCHES "MSVCx" )
    set ( ${PROJECT_NAME}_HEADERS ${${PROJECT_NAME}_HEADERS}
            ${CMAKE_CURRENT_LIST_DIR}/../windows/getopt.h
            ${CMAKE_CURRENT_LIST_DIR}/../windows/unistd.h )
    set ( ${PROJECT_NAME}_SRC ${${PROJECT_NAME}_SRC}
            ${CMAKE_CURRENT_LIST_DIR}/../windows/getopt.c )
endif ( )        
        
add_library ( ${PROJECT_NAME} STATIC ${${PROJECT_NAME}_SRC} ${${PROJECT_NAME}_HEADERS} )
target_include_directories ( ${PROJECT_NAME} BEFORE PRIVATE 
        ${${PROJECT_NAME}_LOOKASIDE_DIR}/lib )
target_compile_definitions ( ${PROJECT_NAME} PRIVATE
            -DHAVE_STDINT_H 
            -DHAVE_UNISTD_H
            -DHAVE_FCNTL_H
            -DUINT32=uint32_t )

if ( ${PROJECT_NAME}_TOOLCHAIN MATCHES "android_[^_]+_gcc" )
    target_compile_definitions ( ${PROJECT_NAME} PRIVATE
            -DS_IREAD=S_IRUSR
            -DS_IWRITE=S_IWUSR )
endif ( )
        
if ( ${CMAKE_C_COMPILER_ID}x MATCHES "MSVCx" )
    target_include_directories ( ${PROJECT_NAME} BEFORE PRIVATE 
            ${CMAKE_CURRENT_LIST_DIR}/../windows )
    if ( DEFINED ${PROJECT_NAME}_MSVC_ALT_CRUNTIME )
        target_compile_options ( ${PROJECT_NAME} PRIVATE
                ${${PROJECT_NAME}_MSVC_ALT_CRUNTIME} )
    endif ( )
endif ( )
