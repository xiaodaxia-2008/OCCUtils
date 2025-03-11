cmake_minimum_required(VERSION 3.20)

project(occutils)

option(occutils_BUILD_TESTS "Build tests" OFF)

find_package(OpenCASCADE COMPONENTS FoundationClasses ModelingData ModelingAlgorithms
  Visualization ApplicationFramework DataExchange REQUIRED)

add_library(occutils
  src/Surface.cxx
  src/ShapeComponents.cxx
  src/Shape.cxx
  src/Plane.cxx
  src/Pipe.cxx
  src/IO.cxx
  src/Curve.cxx
  src/STEPExport.cxx
  src/Primitive.cxx
  src/Direction.cxx
  src/Boolean.cxx
  src/ExtendedSTEP.cxx
  src/PrintOCC.cxx
  src/Fillet.cxx
  src/Wire.cxx
  src/Edge.cxx
  src/Face.cxx
  src/Equality.cxx
  src/Axis.cxx
  src/Point.cxx
  src/Line.cxx
  src/Compound.cxx
)

target_include_directories(occutils
  PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/include>
)
target_compile_features(occutils PRIVATE cxx_std_17)

target_link_libraries(occutils PUBLIC Boost::headers
  ${OpenCASCADE_FoundationClasses_LIBRARIES}
  ${OpenCASCADE_ModelingData_LIBRARIES}
  ${OpenCASCADE_ModelingAlgorithms_LIBRARIES}
  ${OpenCASCADE_ApplicationFramework_LIBRARIES}
  ${OpenCASCADE_DataExchange_LIBRARIES}
)

if(occutils_BUILD_TESTS)
  find_package(Boost COMPONENTS unit_test_framework CONFIG REQUIRED)
  add_executable(test_occutils
    tests/TestLine.cpp
  )

  target_link_libraries(test_occutils PRIVATE occutils Boost::unit_test_framework)
  target_compile_features(test_occutils PRIVATE cxx_std_17)
  add_test(test_occutils test_occutils)
endif()
