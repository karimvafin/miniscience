cmake_minimum_required(VERSION 3.17)

set(CMAKE_CXX_FLAGS "-std=c++11 -g")

project(miniscience)
add_executable(torus lab_gmsh/torus.cpp)
add_executable(serpent lab_gmsh/serpent.cpp)

add_library(gmsh SHARED IMPORTED)
set_target_properties(gmsh PROPERTIES
    IMPORTED_LOCATION "/Users/karimvafin/opt/anaconda3/envs/python006/lib/libgmsh.4.9.dylib"
    INTERFACE_INCLUDE_DIRECTORIES "/Users/karimvafin/opt/anaconda3/envs/python006/include"
)
set(GMSH_LIBRARY gmsh)

# Укажем библиотеки для линковки
target_link_libraries(torus ${GMSH_LIBRARY})
target_link_libraries(serpent ${GMSH_LIBRARY})
