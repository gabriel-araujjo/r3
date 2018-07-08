include_directories("${PROJECT_SOURCE_DIR}/3rdparty ${PROJECT_SOURCE_DIR}")
set(zmalloc_SRCS zmalloc.c)
add_library(zmalloc STATIC ${zmalloc_SRCS})
target_include_directories(zmalloc PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}"
    PRIVATE
    "${CONFIG_INCLUDE_DIR}"
)
# add_library(r3 SHARED ${libr3_SRCS})
# target_link_libraries(r3 cblas)
# install(FILES ${libswiftnav_HEADERS} DESTINATION include/libswiftnav)
