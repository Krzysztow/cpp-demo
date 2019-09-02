add_executable(cpp-demo-tests main.cpp)
target_link_libraries(cpp-demo-tests CONAN_PKG::gtest)

add_test(NAME cpp_demo_test COMMAND $<TARGET_FILE:cpp-demo-tests>)