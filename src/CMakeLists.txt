add_library(r3 STATIC
  node.c
  edge.c
  str.c
  token.c
  match_entry.c
  slug.c
  memory.c)

target_link_libraries(r3 zmalloc pcre)

target_include_directories(r3 PUBLIC
  "${PROJECT_SOURCE_DIR}/include"
  PRIVATE
  "${CONFIG_INCLUDE_DIR}"
  "${CMAKE_CURRENT_SOURCE_DIR}")

set_property(TARGET r3 PROPERTY PUBLIC_HEADER
  ${PROJECT_SOURCE_DIR}/include/memory.h
  ${PROJECT_SOURCE_DIR}/include/r3.h
  ${PROJECT_SOURCE_DIR}/include/r3.hpp
  ${PROJECT_SOURCE_DIR}/include/r3_gvc.h
  ${PROJECT_SOURCE_DIR}/include/r3_json.h
  ${PROJECT_SOURCE_DIR}/include/r3_list.h
  ${PROJECT_SOURCE_DIR}/include/r3_slug.h
  ${PROJECT_SOURCE_DIR}/include/str_array.h)

install(TARGETS r3
  ARCHIVE DESTINATION lib
  PUBLIC_HEADER DESTINATION include/r3)
