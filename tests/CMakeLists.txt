get_property(TEST_INCLUDES TARGET r3 PROPERTY INCLUDE_DIRECTORIES)

function(add_r3_test NAME)
  add_executable(${NAME} ${ARGN})

  target_include_directories(${NAME}
    PRIVATE
    "${CONFIG_INCLUDE_DIR}")

  target_link_libraries(${NAME} check r3)
  target_include_directories(${NAME} PRIVATE
    ${TEST_INCLUDES})
  
  add_test(NAME ${NAME} COMMAND ${NAME})
endfunction()

add_r3_test(check_tree check_tree.c)
add_r3_test(check_slug check_slug.c)
add_r3_test(check_routes check_routes.c)
add_r3_test(check_str_array check_str_array.c)

add_executable(bench bench.c)
target_link_libraries(bench r3)
target_include_directories(bench PRIVATE
    ${TEST_INCLUDES})

