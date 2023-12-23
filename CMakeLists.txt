cmake_minimum_required (VERSION 2.8.11)

project(keva-stratum)

if(CMAKE_HOST_SYSTEM_NAME STREQUAL "Windows")
    set(CMAKE_EXE_LINKER_FLAGS "-static-libgcc -static-libstdc++")
    set(CMAKE_CXX_FLAGS "-static-libgcc -static-libstdc++")
endif()

add_subdirectory(cnutil)

add_custom_target(build ALL DEPENDS cnutil COMMAND go get -d ../. COMMAND go build ..)

if(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/build/www)
    add_custom_command(TARGET build PRE_BUILD COMMAND rm -r ${CMAKE_CURRENT_SOURCE_DIR}/build/www)
endif()

add_custom_command(TARGET build POST_BUILD COMMAND cp -r ${CMAKE_CURRENT_SOURCE_DIR}/www .)
add_custom_command(TARGET build POST_BUILD COMMAND cp -n ${CMAKE_CURRENT_SOURCE_DIR}/config.example.json config.json)

if(MSYS)
    add_custom_target(copy ALL COMMAND cp cnutil/libcnutil.dll .)
endif(MSYS)