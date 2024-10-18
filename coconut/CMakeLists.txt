set(MUTATOR_NAME "command_proto_mutator")
set(TEST_PROTO_CREATOR_NAME "test_proto_creator")

# Mutator lib
protobuf_generate_cpp(
            TEST_PROTO_SRC 
            TEST_PROTO_HDR
            command.proto)

add_library(${MUTATOR_NAME} STATIC
    ${TEST_PROTO_SRC} 
    command_proto_mutator.cpp
)

target_compile_options(${MUTATOR_NAME}
    PRIVATE -g -O1 -fno-omit-frame-pointer # disable afl instrumentation for protobuf lib
)

add_dependencies(${MUTATOR_NAME} aflpm)

target_link_libraries(${MUTATOR_NAME}
    aflpm
)

# target_include_directories(${MUTATOR_NAME}
#     ${CMAKE_CURRENT_BINARY_DIR}
# )

# Testing binary
add_executable(${TEST_PROTO_CREATOR_NAME}
    ${TEST_PROTO_SRC} 
    test_proto_creator.cpp
)

add_dependencies(${TEST_PROTO_CREATOR_NAME}
    aflpm
)

target_link_libraries(${TEST_PROTO_CREATOR_NAME}
    aflpm
    -static
)

# target_include_directories(${TEST_PROTO_CREATOR_NAME}
#     ${CMAKE_CURRENT_BINARY_DIR}
# )