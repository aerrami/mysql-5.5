# Copyright (c) 2006, 2013, Oracle and/or its affiliates. All rights reserved.
# 
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; version 2 of the License.
# 
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
# 
# You should have received a copy of the GNU General Public License
# along with this program; if not, write to the Free Software
# Foundation, Inc., 51 Franklin St, Fifth Floor, Boston, MA  02110-1301 USA

INCLUDE_DIRECTORIES(${CMAKE_SOURCE_DIR}/include
                    ${CMAKE_SOURCE_DIR}/sql
                    ${CMAKE_SOURCE_DIR}/regex
                    ${CMAKE_SOURCE_DIR}/extra/yassl/include
                    ${CMAKE_SOURCE_DIR}/unittest/mytap)


MACRO (MY_ADD_TEST name)
  ADD_EXECUTABLE(${name}-t ${name}-t.c)
  TARGET_LINK_LIBRARIES(${name}-t mytap mysys strings)
  ADD_TEST(${name} ${name}-t)
ENDMACRO()


FOREACH(testname bitmap base64 my_vsnprintf my_atomic my_rdtsc lf my_malloc)
  MY_ADD_TEST(${testname})
ENDFOREACH()

IF(WIN32)
  ADD_EXECUTABLE(explain_filename-t explain_filename-t.cc
                                    ../../sql/nt_servc.cc)
ELSE()
  ADD_EXECUTABLE(explain_filename-t explain_filename-t.cc)
ENDIF()
IF(UNIX)
  SET_TARGET_PROPERTIES(explain_filename-t PROPERTIES LINK_FLAGS "${explain_filename-t_link_flags} -Wl,--no-as-needed")
ENDIF()
TARGET_LINK_LIBRARIES(explain_filename-t ${HOT_BACKUP_LIBS} sql mytap)
ADD_TEST(explain_filename explain_filename-t)
