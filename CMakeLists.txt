project( SDDL LANGUAGES CXX)

cmake_minimum_required(VERSION 3.1.2)
set(CMAKE_VERBOSE_MAKEFILE TRUE)
set ( CMAKE_BUILD_TYPE Release )
set(CMAKE_CXX_FLAGS_MYREL "-O3")

set (CMAKE_CXX_STANDARD 11)

set(SDDL "${CMAKE_CURRENT_SOURCE_DIR}")
message("  Path: ${SDDL}")

SET(CPACK_PACKAGE_VERSION_MAJOR "1")
SET(CPACK_PACKAGE_VERSION_MINOR "1")
SET(CPACK_PACKAGE_VERSION_PATCH "0")

message("  System: ${CMAKE_SYSTEM}")
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

message("  Platform: Windows")
add_definitions ( -Wall) 




add_library(libsddl "")


message("Install path: ${CMAKE_INSTALL_PREFIX}")








include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)

include_directories(${CONAN_INCLUDE_DIRS_TBB}  ${CONAN_INCLUDE_DIRS_libarchive})

link_directories(${CONAN_LIB_DIRS_TBB} ${CONAN_LIB_DIRS_libarchive})

CONAN_BASIC_SETUP(TARGETS)















set (USE_JSON true)
message("USE_JSON: ${USE_JSON}")

IF (USE_JSON)

set(SDDL_PATH "${CMAKE_CURRENT_SOURCE_DIR}")
message("  Path: ${SDDL_PATH}")

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH};${SDDL_PATH}/cmake_modules)
message("  CMAKE_MODULE_PATH: ${CMAKE_MODULE_PATH}") 

# find_package ( LibArchive REQUIRED )
message("  LibArchive_INCLUDE_DIRS: ${LibArchive_INCLUDE_DIRS}")
message("  LibArchive_LIBRARIES: ${LibArchive_LIBRARIES}")

# find_package(TBB REQUIRED)

include_directories (

    ${LibArchive_INCLUDE_DIRS}
    )
ENDIF()






include_directories (
    ${CMAKE_SOURCE_DIR}
    ${CMAKE_CURRENT_BINARY_DIR}
    ${PROJECT_BINARY_DIR}/include
    )

include(src/CMakeLists.txt)

configure_file(src/global.h.in include/global.h)



IF (USE_JSON)
target_link_libraries ( sddl
    ${LibArchive_LIBRARIES}
    ${TBB_LIBRARIES})
ENDIF()


install (TARGETS libsddl DESTINATION lib
    PUBLIC_HEADER DESTINATION include/sddl)
    
install (TARGETS sddl DESTINATION bin)




