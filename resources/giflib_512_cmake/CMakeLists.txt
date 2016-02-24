# Copyright 2016, alex at staticlibs.net
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

# this file is based on https://github.com/glennrp/libpng/blob/v1.6.21/CMakeLists.txt
cmake_minimum_required ( VERSION 2.8.12 )
project ( gif C )
set ( ${PROJECT_NAME}_SOURCE_DIR ${CMAKE_CURRENT_LIST_DIR}/../../giflib )

# check deplibs cache
if ( STATICLIB_USE_DEPLIBS_CACHE )
    set ( ${PROJECT_NAME}_CACHED_LIB_PATH ${STATICLIB_DEPLIBS_CACHE_DIR}/${CMAKE_STATIC_LIBRARY_PREFIX}${PROJECT_NAME}${CMAKE_STATIC_LIBRARY_SUFFIX} )
    if ( EXISTS ${${PROJECT_NAME}_CACHED_LIB_PATH} )
        add_library( ${PROJECT_NAME} STATIC IMPORTED GLOBAL )
        set_target_properties( ${PROJECT_NAME} PROPERTIES IMPORTED_LOCATION ${${PROJECT_NAME}_CACHED_LIB_PATH} )
        message ( STATUS "Using cached library: [${${PROJECT_NAME}_CACHED_LIB_PATH}]" )
        return ( )
    endif ( )
endif ( )

# dependencies
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
        ${${PROJECT_NAME}_SOURCE_DIR}/lib/openbsd-reallocarray.c
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
        ${PROJECT_NAME}_SOURCE_DIR/lib )
        
if ( ${CMAKE_C_COMPILER_ID}x MATCHES "MSVCx" )
    target_include_directories ( ${PROJECT_NAME} BEFORE PRIVATE 
            ${CMAKE_CURRENT_LIST_DIR}/../windows )
endif ( )        

