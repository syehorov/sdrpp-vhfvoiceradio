cmake_minimum_required(VERSION 3.13)
project(vhfvoiceradio)

set(CMAKE_INSTALL_PREFIX "/usr/")
SET(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${PROJECT_SOURCE_DIR}/cmake/")

file(GLOB_RECURSE SRC "src/*.cpp" "src/*.h" "src/*.hpp" "src/*.c" "mbelib/*.c")

find_package(ITPP REQUIRED)
find_package(nlohmann_json 3.10.5 REQUIRED)

if(MSVC)
    # needed for M_PI macro
    add_definitions(-D_USE_MATH_DEFINES -D_CRT_SECURE_NO_WARNINGS)
endif()

if (NOT SDRPP_MODULE_CMAKE)
    set(SDRPP_MODULE_CMAKE "/usr/lib/cmake/sdrpp/sdrpp_module.cmake")
endif()

include(${SDRPP_MODULE_CMAKE})

target_link_libraries(vhfvoiceradio PRIVATE fftw3 ${ITPP_LIBRARY})
target_link_libraries(${PROJECT_NAME} PRIVATE nlohmann_json::nlohmann_json)
target_include_directories(vhfvoiceradio PRIVATE "src/" "mbelib/")
target_include_directories(${PROJECT_NAME} PRIVATE "/usr/include/nlohmann")
