cmake_minimum_required(VERSION 3.12)
project(CGBN LANGUAGES CXX CUDA)

# Specify the CUDA architecture to match your hardware
set(CMAKE_CUDA_ARCHITECTURES 70 75 80 86)

# Enable C++17 for both C++ and CUDA code
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CUDA_STANDARD 17)

# Add the source files for the CGBN library
file(GLOB CGBN_SOURCES src/*.cpp src/*.cu) # Update the paths as necessary

# Add the GMP library dependency (adjust path if GMP is installed locally)
find_package(GMP REQUIRED)
include_directories(${GMP_INCLUDE_DIR})

# Add the CGBN library as a static library
add_library(CGBN STATIC ${CGBN_SOURCES})
target_include_directories(CGBN PUBLIC include) # Update "include" to the actual path of the CGBN headers
target_link_libraries(CGBN PRIVATE ${GMP_LIBRARIES})

# Create an executable for testing or examples
add_executable(cgbn_example examples/example_add.cu) # Update with your example file
target_link_libraries(cgbn_example PRIVATE CGBN)

# Specify compile flags for CUDA
set_target_properties(CGBN PROPERTIES CUDA_SEPARABLE_COMPILATION ON)
