cmake_minimum_required (VERSION 2.8.11)

project(keva-stratum)

add_subdirectory(cnutil)

add_custom_target(build ALL DEPENDS cnutil COMMAND go get -d ../. COMMAND go build ..)
add_custom_target(copy_www ALL COMMAND cp -r ${CMAKE_CURRENT_SOURCE_DIR}/www .)

if(MSYS)
    add_custom_target(copy ALL COMMAND cp cnutil/libcnutil.dll .)
endif(MSYS)


