cmake_minimum_required(VERSION 3.22.1)

message("CMAKE_CURRENT_LIST_DIR = ${CMAKE_CURRENT_LIST_DIR}")

set(NATIVE_SOURCES
        ${CMAKE_CURRENT_LIST_DIR}/src/main/native-code/demo.cpp)

add_library(c_demo SHARED ${NATIVE_SOURCES})
set(LIB_NAME c_demo)

target_include_directories(${LIB_NAME} PRIVATE ${CMAKE_CURRENT_LIST_DIR}/src/main/native-code})

if (PKG_SIZE_OPT)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fvisibility=hidden")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fvisibility=hidden")
    target_compile_options(${LIB_NAME} PUBLIC -Oz -flto -faddrsig -ffunction-sections -fdata-sections -fno-rtti -fno-exceptions)
    target_link_options(${LIB_NAME} PUBLIC -Oz -flto -Wl,--icf=all -Wl,-mllvm,--enable-machine-outliner=always -Wl,--exclude-libs,ALL -Wl,--gc-sections)
endif ()

target_link_libraries(${LIB_NAME} log)