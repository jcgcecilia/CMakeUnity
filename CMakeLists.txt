cmake_minimum_required(VERSION 3.0)
project(Unity)

include(CTest)

if(UNIX)
    message("You're running CMake on UNIX")
endif(UNIX)


SET(CMAKE_C_COMPILER gcc)

SET(EXECUTABLE_OUTPUT_PATH ${PROJECT_BINARY_DIR}/bin)

set(TARGET_GROUP production CACHE STRING "What Should I Build?")


set(CMAKE_C_FLAGS "-std=c89 ${CMAKE_C_FLAGS}")
set(CMAKE_C_FLAGS "-Wall ${CMAKE_C_FLAGS}")
set(CMAKE_C_FLAGS "-Wextra ${CMAKE_C_FLAGS}")
set(CMAKE_C_FLAGS "-Wpointer-arith ${CMAKE_C_FLAGS}")
set(CMAKE_C_FLAGS "-Wwrite-strings ${CMAKE_C_FLAGS}")
set(CMAKE_C_FLAGS "-Wswitch-default ${CMAKE_C_FLAGS}")
set(CMAKE_C_FLAGS "-Wunreachable-code ${CMAKE_C_FLAGS}")
set(CMAKE_C_FLAGS "-Winit-self ${CMAKE_C_FLAGS}")
set(CMAKE_C_FLAGS "-Wmissing-field-initializers ${CMAKE_C_FLAGS}")
set(CMAKE_C_FLAGS "-Wno-unknown-pragmas ${CMAKE_C_FLAGS}")
set(CMAKE_C_FLAGS "-Wstrict-prototypes ${CMAKE_C_FLAGS}")
set(CMAKE_C_FLAGS "-Wundef ${CMAKE_C_FLAGS}")
set(CMAKE_C_FLAGS "-Wold-style-definition ${CMAKE_C_FLAGS}")

set(RUBY ruby)
set(RUNNER TestProductionCode_Runner)


add_custom_command(
    OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/test/test_runners/${RUNNER}.c
    COMMAND ${RUBY} ${CMAKE_CURRENT_SOURCE_DIR}/unity/auto/generate_test_runner.rb ${CMAKE_CURRENT_SOURCE_DIR}/test/TestProductionCode.c  ${CMAKE_CURRENT_SOURCE_DIR}/test/test_runners/${RUNNER}.c
    DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/test/TestProductionCode.c
    COMMENT "Generating code for Test runner"
)

#add_custom_target(
#    runners
#    DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/test/test_runners/${RUNNER}.c
#    )

include_directories(
    src
)

set(SOURCES 
    src/ProductionCode.c
    test/TestProductionCode.c
    test/test_runners/TestProductionCode_Runner.c
)

add_subdirectory(unity)

add_executable (test_runner1 ${SOURCES})
#add_dependencies(test_runner1 runners)
target_link_libraries(test_runner1 unity)
add_test(test1 test_runner1)