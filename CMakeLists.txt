cmake_minimum_required(VERSION 3.0)

project(polymorphic_value)

find_package(Catch2 REQUIRED)

SET(CMAKE_CXX_STANDARD 14)
if(MSVC)
  add_compile_options("/EHsc")
else()
  set(CMAKE_CXX_FLAGS_SANITIZE "-fsanitize=address,undefined")
  set(CMAKE_EXE_LINKER_FLAGS_SANITIZE "-fsanitize=address,undefined")
endif()

if(${CMAKE_CXX_COMPILER_ID} MATCHES "Clang" OR ${CMAKE_CXX_COMPILER_ID} MATCHES "GNU")
  #add_compile_options("-Werror;-Wall;-Wno-self-assign-overloaded;-Wno-unknown-warning-option")
endif()

add_executable(test_polymorphic_value test_polymorphic_value.cpp)
target_link_libraries(test_polymorphic_value PRIVATE Catch2::Catch2)

enable_testing()
add_test(
  NAME test_polymorphic_value
  COMMAND test_polymorphic_value
  WORKING_DIRECTORY ${CMAKE_RUNTIME_OUTPUT_DIRECTORY})

add_library(polymorphic_value INTERFACE)

target_include_directories(polymorphic_value
  INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}>
    $<INSTALL_INTERFACE:include/polymorphic_value>)

install(TARGETS polymorphic_value
        EXPORT polymorphic_value-config
        INCLUDES DESTINATION include)

install(EXPORT polymorphic_value-config
        FILE polymorphic_value-config.cmake
        NAMESPACE polymorphic_value::
        DESTINATION share/polymorphic_value/cmake)

install(FILES polymorphic_value.h DESTINATION include/polymorphic_value)
