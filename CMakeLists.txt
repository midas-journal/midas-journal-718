cmake_minimum_required(VERSION 2.6)

PROJECT(PoissonReconstruction)

FIND_PACKAGE(VTK REQUIRED)
INCLUDE(${VTK_USE_FILE})


SET(BUILD_PARAVIEW_PLUGIN OFF CACHE BOOL "Build Paraview plugin?")

if(BUILD_PARAVIEW_PLUGIN)
FIND_PACKAGE(ParaView REQUIRED)
INCLUDE(${PARAVIEW_USE_FILE})

INCLUDE_DIRECTORIES(${INCLUDE_DIRECTORIES} ../)

ADD_PARAVIEW_PLUGIN(PoissonReconstructionPlugin "1.0"
  SERVER_MANAGER_XML vtkPoissonReconstruction.xml
SERVER_MANAGER_SOURCES vtkPoissonReconstruction.cxx  
SERVER_SOURCES Factor.cpp Geometry.cpp MarchingCubes.cpp MultiGridOctest.cpp MultiGridOctreeData.cpp 
)

endif(BUILD_PARAVIEW_PLUGIN)

ADD_EXECUTABLE(PoissonReconstruction Example.cxx vtkPoissonReconstruction.cxx Factor.cpp Geometry.cpp MarchingCubes.cpp MultiGridOctest.cpp MultiGridOctreeData.cpp )
TARGET_LINK_LIBRARIES(PoissonReconstruction vtkHybrid)

ADD_TEST( PoissonReconstructionTest 
  ${PoissonReconstruction_BINARY_DIR}/PoissonReconstruction 
  ${PoissonReconstruction_SOURCE_DIR}/data/horsePoints.vtp
  5 
  temp.vtp 
)

INCLUDE( CTest )
