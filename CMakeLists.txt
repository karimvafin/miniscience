cmake_minimum_required(VERSION 3.17)

set(CMAKE_CXX_FLAGS "-std=c++11 -g")
set(VTK_DIR "~/vtkbase/lib/cmake/vtk-9.1")

project(miniscience)
add_executable(torus lab_gmsh/torus.cpp)
add_executable(serpent lab_gmsh/serpent.cpp)
add_executable(serpent_vtk lab_vtk/serpent_vtk.cpp)

add_executable(cubic2d lab_vtk/cubic2d.cpp)
add_executable(cubic3d lab_vtk/cubic3d.cpp)
add_executable(tetr3d lab_vtk/tetr3d.cpp)

# Найдём VTK
find_package(VTK REQUIRED)
# ... и подключим её со всеми её компонентами её нативными средствами
include(${VTK_USE_FILE})

add_library(gmsh SHARED IMPORTED)
set_target_properties(gmsh PROPERTIES
    IMPORTED_LOCATION "/Users/karimvafin/opt/anaconda3/envs/python006/lib/libgmsh.4.9.dylib"
    INTERFACE_INCLUDE_DIRECTORIES "/Users/karimvafin/opt/anaconda3/envs/python006/include"
)
set(GMSH_LIBRARY gmsh)

# Укажем библиотеки для линковки
target_link_libraries(torus ${GMSH_LIBRARY})
target_link_libraries(serpent ${GMSH_LIBRARY})

# ... тут только VTK
target_link_libraries(cubic2d ${VTK_LIBRARIES})
target_link_libraries(cubic3d ${VTK_LIBRARIES})

# ... а тут ещё и gmsh
target_link_libraries(tetr3d ${VTK_LIBRARIES})
target_link_libraries(tetr3d ${GMSH_LIBRARY})

target_link_libraries(serpent_vtk ${VTK_LIBRARIES})
target_link_libraries(serpent_vtk ${GMSH_LIBRARY})
