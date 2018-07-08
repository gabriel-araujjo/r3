add_executable(check_tree check_tree.c)
target_link_libraries(check_tree r3 check pcre)

target_include_directories(check_tree
    PRIVATE
    "${CONFIG_INCLUDE_DIR}"
)

add_custom_command(
    TARGET check_tree POST_BUILD
    COMMENT "Running unit tests"
    COMMAND check_tree
)
