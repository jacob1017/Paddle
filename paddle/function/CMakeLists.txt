file(GLOB h_files . *Op.h)
file(GLOB cpp_files . *Op.cpp)

list(APPEND h_files Function.h)
list(APPEND cpp_files Function.cpp)

if(WITH_GPU)
    file(GLOB cu_files . *OpGpu.cu)
    cuda_compile(cu_objs ${cu_files})
endif()

add_library(paddle_function STATIC ${cpp_files} ${cu_objs})

add_library(paddle_test_main STATIC TestMain.cpp)

if(WITH_GPU)
    # TODO:
    # file(GLOB test_files . *OpTest.cpp)
    # add_executable(${test_bin} EXCLUDE_FROM_ALL ${test_files})
    add_simple_unittest(CrossMapNormalOpTest)
endif()

add_style_check_target(paddle_function ${h_files})
add_style_check_target(paddle_function ${cpp_files})
if(WITH_GPU)
    add_style_check_target(paddle_function ${cu_files})
endif()
