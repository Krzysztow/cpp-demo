add_executable(cpp-demo-tests main.cpp)
target_link_libraries(cpp-demo-tests CONAN_PKG::gtest)