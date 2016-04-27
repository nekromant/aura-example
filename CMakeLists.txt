CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

PROJECT(aura-test)
SET(PROJECT_VERSION   0.1.2)

SET(SRCS main.c)

FIND_PACKAGE(PkgConfig)
PKG_CHECK_MODULES(AURA REQUIRED aura)

SET(CMAKE_C_FLAGS "${AURA_CFLAGS_OTHER} -Wall -Werror")

INCLUDE_DIRECTORIES(
    ${AURA_INCLUDE_DIRS}
)

message(STATUS "Generarating ${CMAKE_SOURCE_DIR}/.clang_complete")
get_property(dirs DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY INCLUDE_DIRECTORIES)
file(WRITE ${CMAKE_SOURCE_DIR}/.clang_complete "")
foreach(dir ${dirs})
  file(APPEND ${CMAKE_SOURCE_DIR}/.clang_complete "-I${dir}\n")
endforeach()
string(REPLACE "'" "" CMAKE_C_FLAGS_SPLIT ${CMAKE_C_FLAGS})
string(REPLACE " " ";" CMAKE_C_FLAGS_SPLIT ${CMAKE_C_FLAGS_SPLIT})
foreach(flag ${CMAKE_C_FLAGS_SPLIT})
  file(APPEND ${CMAKE_SOURCE_DIR}/.clang_complete "${flag}\n")
endforeach()



ADD_EXECUTABLE(aura-example ${SRCS})

TARGET_LINK_LIBRARIES(aura-example
    ${AURA_LIBRARIES}
)

INSTALL(TARGETS aura-example
        DESTINATION bin)
