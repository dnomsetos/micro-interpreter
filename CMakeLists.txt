cmake_minimum_required(VERSION 3.15)

project(micro-inter LANGUAGES CXX)

option(MICROINTER_BUILD_TESTS    "Build tests"    OFF)
option(MICROINTER_BUILD_EXAMPLES "Build examples" OFF)

file(GLOB_RECURSE MICROINTER_HEADERS 
    CONFIGURE_DEPENDS
    include/microinter/*.hpp
)

file(GLOB_RECURSE MICROINTER_SRC     
    CONFIGURE_DEPENDS 
    src/*.cpp
)

if(MICROINTER_SRC)
    add_library(microinter STATIC)
    target_sources(microinter PRIVATE ${MICROINTER_SRC})
    target_include_directories(microinter
        PUBLIC $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    )
    target_compile_features(microinter PUBLIC cxx_std_20)
else()
    add_library(microinter INTERFACE)
    target_include_directories(microinter
        INTERFACE $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    )
    target_compile_features(microinter INTERFACE cxx_std_20)
endif()

add_library(micro-inter ALIAS microinter) 

target_sources(microinter
    PRIVATE ${MICROINTER_SRC}
    PUBLIC ${MICROINTER_HEADERS}
)

if(MICROINTER_BUILD_EXAMPLES)
    add_subdirectory(examples)
endif()

if(MICROINTER_BUILD_TESTS)
    enable_testing()
    add_subdirectory(tests)
endif()

