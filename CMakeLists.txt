project(OpenSimJAMPlugin)

cmake_minimum_required(VERSION 3.2)

file(GLOB SOURCE_FILES *.cpp)
file(GLOB INCLUDE_FILES *.h)

set(OPENSIM_INSTALL_DIR "C:/OpenSim 4.6"
    CACHE PATH "Top-level directory of OpenSim install")

set(PLUGIN_NAME "osimJAMPlugin"
    CACHE STRING "Name of shared library to create")

# OpenSim 4.6 requires C++20 (std::format logging).
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(OpenSim REQUIRED PATHS "${OPENSIM_INSTALL_DIR}" "${OPENSIM_INSTALL_DIR}/cmake" NO_DEFAULT_PATH)

add_library(${PLUGIN_NAME} SHARED ${SOURCE_FILES} ${INCLUDE_FILES})

target_link_libraries(${PLUGIN_NAME} ${OpenSim_LIBRARIES})

set_target_properties(
    ${PLUGIN_NAME} PROPERTIES
    DEFINE_SYMBOL OSIMPLUGIN_EXPORTS
    PROJECT_LABEL "Libraries - ${PLUGIN_NAME}")

install(TARGETS ${PLUGIN_NAME} RUNTIME DESTINATION plugins LIBRARY DESTINATION plugins ARCHIVE DESTINATION plugins)
