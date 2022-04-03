cmake_minimum_required(VERSION 3.16.3)
project(o1heap)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

add_library(${PROJECT_NAME} o1heap/o1heap.c)
target_include_directories(${PROJECT_NAME} PUBLIC $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}>)
install(
    DIRECTORY "${CMAKE_SOURCE_DIR}/o1heap"
    DESTINATION "include"
    FILES_MATCHING PATTERN "*.h"
)

include(GNUInstallDirs)
install(
    TARGETS ${PROJECT_NAME}
    EXPORT ${PROJECT_NAME}Targets
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
)
    
install(
    EXPORT ${PROJECT_NAME}Targets
    FILE ${PROJECT_NAME}Targets.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
)

include(CMakePackageConfigHelpers)
configure_package_config_file(
    Config.cmake.in
    ${PROJECT_NAME}Config.cmake
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
    PATH_VARS CMAKE_INSTALL_LIBDIR
)

install(
    FILES ${PROJECT_BINARY_DIR}/${PROJECT_NAME}Config.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}
)
