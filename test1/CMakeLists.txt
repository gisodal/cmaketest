cmake_minimum_required(VERSION 3.0)

# set directory as project name
get_filename_component(PROJECT ${CMAKE_CURRENT_LIST_DIR} NAME)
string(REPLACE " " "_" PROJECT ${PROJECT})
project(${PROJECT})

# set compiler flags
set(CMAKE_OPTIMIZATION "-O3")
if (CMAKE_BUILD_TYPE MATCHES DEBUG)
    set(CMAKE_OPTIMIZATION "-O0")
endif()
set(CMAKE_CXX_FLAGS "--std=c++11 ${CMAKE_OPTIMIZATION}")
set(CMAKE_C_FLAGS "${CMAKE_OPTIMIZATION}")
set(CMAKE_C_FLAGS_RELEASE "-DNDEBUG")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE}")
set(CMAKE_C_FLAGS_DEBUG "-ggdb3 -Wall -Wextra -DDEBUG   \
    -Wno-format -Wno-write-strings -Wno-unused-function \
    -Wno-unused-parameter -Wno-system-headers           \
    -Wno-format-security -Wno-ignored-qualifiers")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG}")

# set source files
set(SOURCE_DIR ${CMAKE_CURRENT_LIST_DIR}/src)
set(INCLUDE_DIR ${CMAKE_CURRENT_LIST_DIR}/include)

file(GLOB HEADERS "${INCLUDE_DIR}/*.h")
file(GLOB SOURCES ${SOURCE_DIR}/*.cc)

set(LIB_SOURCES ${SOURCES})

## set include directories
include_directories( ${INCLUDE_DIR} ${CMAKE_INSTALL_PREFIX}/include)

# create executable
add_executable(${PROJECT}-bin ${SOURCES})
set_target_properties(${PROJECT}-bin PROPERTIES OUTPUT_NAME ${PROJECT})

# set install locations
install(TARGETS ${PROJECT}-bin
    RUNTIME DESTINATION "${CMAKE_INSTALL_PREFIX}/bin"
)
install(FILES ${HEADERS}
    DESTINATION "${CMAKE_INSTALL_PREFIX}/include/${PROJECT}"
)

