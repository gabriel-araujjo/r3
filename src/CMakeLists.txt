set(R3_SOURCES 
    node.c 
    edge.c
    str.c
    token.c
    match_entry.c
    slug.c
    memory.c
)

add_library(r3 STATIC ${R3_SOURCES})
target_link_libraries(r3 zmalloc pcre)
target_include_directories(r3 PUBLIC
    "${PROJECT_SOURCE_DIR}/include"
    PRIVATE
    "${CONFIG_INCLUDE_DIR}"
    "${CMAKE_SOURCE_DIR}"
)

install(TARGETS r3 DESTINATION lib)
# install(FILES ${libswiftnav_HEADERS} DESTINATION include/libswiftnav)
