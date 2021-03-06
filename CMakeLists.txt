cmake_minimum_required(VERSION 3.9.2)

# set project name
project(cwalk
  VERSION 1.0.0
  DESCRIPTION "A simple path library"
  LANGUAGES C)

# include utilities
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")
include(EnableWarnings)
include(CTest)
include(CreateTestList)

# configure requirements
set(CMAKE_C_STANDARD 11)

# setup target and directory names
set(LIBRARY_TARGET "cwalk")
set(TEST_TARGET "cwalktest")
set(INCLUDE_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/include")
set(SOURCE_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/src")
set(TEST_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/test")

# enable coverage if requested
if(ENABLE_COVERAGE)
  message("-- Coverage enabled")
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fprofile-arcs -ftest-coverage")
  set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} --coverage")
endif()

# enable sanitizer
if(ENABLE_SANITIZER)
  message("-- Sanitizer enabled")
  set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fno-omit-frame-pointer -fsanitize=${ENABLE_SANITIZER}")
  set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -fno-omit-frame-pointer -fsanitize=${ENABLE_SANITIZER}")
endif()

# add the main executable
add_library(${LIBRARY_TARGET}
  "${INCLUDE_DIRECTORY}/cwalk.h"
  "${SOURCE_DIRECTORY}/cwalk.c")
target_include_directories(${LIBRARY_TARGET} PUBLIC "${INCLUDE_DIRECTORY}")

# add tests
enable_testing()
create_test(DEFAULT absolute simple)
create_test(DEFAULT absolute absolute_path)
create_test(DEFAULT absolute relative_base)
create_test(DEFAULT absolute mixed)
create_test(DEFAULT absolute normalization)
create_test(DEFAULT absolute too_far)
create_test(DEFAULT absolute check)
create_test(DEFAULT basename simple)
create_test(DEFAULT basename empty)
create_test(DEFAULT basename trailing_separator)
create_test(DEFAULT basename trailing_separators)
create_test(DEFAULT basename no_separators)
create_test(DEFAULT basename special_directories)
create_test(DEFAULT basename root)
create_test(DEFAULT dirname simple)
create_test(DEFAULT dirname empty)
create_test(DEFAULT dirname trailing_separator)
create_test(DEFAULT dirname trailing_separators)
create_test(DEFAULT dirname no_separators)
create_test(DEFAULT dirname special_directories)
create_test(DEFAULT dirname root)
create_test(DEFAULT extension get_simple)
create_test(DEFAULT extension get_without)
create_test(DEFAULT extension get_first)
create_test(DEFAULT extension get_last)
create_test(DEFAULT extension get_multiple)
create_test(DEFAULT extension check_simple)
create_test(DEFAULT extension check_empty)
create_test(DEFAULT extension check_without)
create_test(DEFAULT intersection simple)
create_test(DEFAULT intersection trailing_separator)
create_test(DEFAULT intersection double_separator)
create_test(DEFAULT intersection empty)
create_test(DEFAULT intersection unequal_roots)
create_test(DEFAULT intersection relative_absolute_mix)
create_test(DEFAULT intersection same_roots)
create_test(DEFAULT intersection one_root_only)
create_test(DEFAULT intersection relative_base)
create_test(DEFAULT intersection relative_other)
create_test(DEFAULT intersection skipped_end)
create_test(DEFAULT join simple)
create_test(DEFAULT join navigate_back)
create_test(DEFAULT join empty)
create_test(DEFAULT join two_absolute)
create_test(DEFAULT join two_unc)
create_test(DEFAULT join back_after_root)
create_test(DEFAULT join relative_back_after_root)
create_test(DEFAULT normalize do_nothing)
create_test(DEFAULT normalize navigate_back)
create_test(DEFAULT normalize relative_too_far)
create_test(DEFAULT normalize absolute_too_far)
create_test(DEFAULT normalize terminated)
create_test(DEFAULT normalize double_separator)
create_test(DEFAULT normalize remove_current)
create_test(DEFAULT normalize mixed)
create_test(DEFAULT normalize overlap)
create_test(DEFAULT normalize empty)
create_test(DEFAULT normalize only_separators)
create_test(DEFAULT normalize back_after_root)
create_test(DEFAULT relative simple)
create_test(DEFAULT relative relative)
create_test(DEFAULT relative long_base)
create_test(DEFAULT relative long_target)
create_test(DEFAULT relative equal)
create_test(DEFAULT relative base_skipped_end)
create_test(DEFAULT relative target_skipped_end)
create_test(DEFAULT relative base_div_skipped_end)
create_test(DEFAULT relative target_div_skipped_end)
create_test(DEFAULT relative skip_all)
create_test(DEFAULT relative different_roots)
create_test(DEFAULT relative check)
create_test(DEFAULT segment first)
create_test(DEFAULT segment last)
create_test(DEFAULT segment next)
create_test(DEFAULT segment next_too_far)
create_test(DEFAULT segment previous_absolute)
create_test(DEFAULT segment previous_relative)
create_test(DEFAULT segment previous_too_far)
create_test(DEFAULT segment previous_too_far_root)
create_test(DEFAULT segment type)
create_test(DEFAULT segment back_with_root)
create_test(DEFAULT windows change_style)
create_test(DEFAULT windows get_root)
create_test(DEFAULT windows get_unc_root)
create_test(DEFAULT windows get_root_separator)
create_test(DEFAULT windows get_root_relative)
create_test(DEFAULT windows intersection_case)
create_test(DEFAULT windows root_backslash)
create_test(DEFAULT windows root_empty)
create_test_list(DEFAULT "${TEST_DIRECTORY}/tests.h")

add_executable(${TEST_TARGET}
  "${TEST_DIRECTORY}/main.c"
  "${TEST_DIRECTORY}/absolute_test.c"
  "${TEST_DIRECTORY}/basename_test.c"
  "${TEST_DIRECTORY}/dirname_test.c"
  "${TEST_DIRECTORY}/extension_test.c"
  "${TEST_DIRECTORY}/intersection_test.c"
  "${TEST_DIRECTORY}/join_test.c"
  "${TEST_DIRECTORY}/normalize_test.c"
  "${TEST_DIRECTORY}/relative_test.c"
  "${TEST_DIRECTORY}/segment_test.c"
  "${TEST_DIRECTORY}/windows_test.c")
target_link_libraries(${TEST_TARGET} PUBLIC ${LIBRARY_TARGET})
target_include_directories(${TEST_TARGET} PUBLIC "${INCLUDE_DIRECTORY}")

# enable warnings
enable_warnings(${LIBRARY_TARGET})
