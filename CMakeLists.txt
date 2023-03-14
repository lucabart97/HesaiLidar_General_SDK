CMAKE_MINIMUM_REQUIRED(VERSION 3.18...3.20)

project(PandarGeneralSDK VERSION 1.0
                         DESCRIPTION "PandarGeneralSDK project"
                         LANGUAGES CXX C)

set(CMAKE_POSITION_INDEPENDENT_CODE ON)
set(CMAKE_CXX_STANDARD 11 CACHE STRING "The C++ standard to use")
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# suppress stupid cmake warnings
if(NOT DEFINED CMAKE_SUPPRESS_DEVELOPER_WARNINGS)
     set(CMAKE_SUPPRESS_DEVELOPER_WARNINGS 1 CACHE INTERNAL "No dev warnings")
endif()


#-------------------------------------------------------------------------------
# External Libraries
#-------------------------------------------------------------------------------
find_package(Boost REQUIRED COMPONENTS thread REQUIRED)
find_package(PCL REQUIRED COMPONENTS common REQUIRED)

#-------------------------------------------------------------------------------
# PandarGeneralSDK library
#-------------------------------------------------------------------------------
file(GLOB data src/*)
add_library(${PROJECT_NAME} SHARED 
    ${data}
)
target_link_libraries(${PROJECT_NAME}
    ${Boost_LIBRARIES}
    ${PCL_LIBRARIES}
    pcap
)
target_include_directories(${PROJECT_NAME} PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
    ${Boost_INCLUDE_DIRS}
    ${PCL_INCLUDE_DIRS}
)
add_library(${PROJECT_NAME}::${PROJECT_NAME} ALIAS ${PROJECT_NAME})

install(TARGETS PandarGeneralSDK)

file(GLOB header include/pandarGeneral_sdk/*)
install(FILES ${header} DESTINATION ${CMAKE_INSTALL_PREFIX})