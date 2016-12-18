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
set ( ${PROJECT_NAME}_SOURCE_DIR ${CMAKE_CURRENT_LIST_DIR}/../../giflib )

# library
set ( ${PROJECT_NAME}_HEADERS
        ${${PROJECT_NAME}_SOURCE_DIR}/lib/gif_lib.h
        ${${PROJECT_NAME}_SOURCE_DIR}/lib/gif_hash.h
        ${${PROJECT_NAME}_SOURCE_DIR}/lib/gif_lib_private.h )
set ( ${PROJECT_NAME}_SRC
        ${${PROJECT_NAME}_SOURCE_DIR}/lib/dgif_lib.c
        ${${PROJECT_NAME}_SOURCE_DIR}/lib/egif_lib.c
        ${${PROJECT_NAME}_SOURCE_DIR}/lib/gif_err.c
        ${${PROJECT_NAME}_SOURCE_DIR}/lib/gif_font.c
        ${${PROJECT_NAME}_SOURCE_DIR}/lib/gifalloc.c
        #${${PROJECT_NAME}_SOURCE_DIR}/lib/openbsd-reallocarray.c
        ${${PROJECT_NAME}_SOURCE_DIR}/lib/quantize.c )
if ( ${CMAKE_C_COMPILER_ID}x MATCHES "MSVCx" )
    set ( ${PROJECT_NAME}_HEADERS ${${PROJECT_NAME}_HEADERS}
            ${CMAKE_CURRENT_LIST_DIR}/../windows/getopt.h
            ${CMAKE_CURRENT_LIST_DIR}/../windows/unistd.h )
    set ( ${PROJECT_NAME}_SRC ${${PROJECT_NAME}_SRC}
            ${CMAKE_CURRENT_LIST_DIR}/../windows/getopt.c )
endif ( )        
        
add_library ( ${PROJECT_NAME} STATIC ${${PROJECT_NAME}_SRC} ${${PROJECT_NAME}_HEADERS} )
target_include_directories ( ${PROJECT_NAME} BEFORE PRIVATE 
        ${${PROJECT_NAME}_SOURCE_DIR}/lib )
target_compile_definitions ( ${PROJECT_NAME} PRIVATE
            -DHAVE_STDINT_H 
            -DHAVE_UNISTD_H
            -DHAVE_FCNTL_H
            -DUINT32=uint32_t )
        
if ( ${CMAKE_C_COMPILER_ID}x MATCHES "MSVCx" )
    target_include_directories ( ${PROJECT_NAME} BEFORE PRIVATE 
            ${CMAKE_CURRENT_LIST_DIR}/../windows )
endif ( )        

