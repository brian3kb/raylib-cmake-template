cmake_minimum_required(VERSION 3.29)
project(project_name)
set(CMAKE_CXX_STANDARD 23)

# Path to the raylib source directory
set(RAYLIB_SRC_DIR /Users/brian/deps/raylib)

# Specify a binary directory for raylib (inside your project's build directory)
set(RAYLIB_BIN_DIR ${CMAKE_BINARY_DIR}/raylib_build)

# Add the raylib library using the specified source and binary directories
add_subdirectory(${RAYLIB_SRC_DIR} ${RAYLIB_BIN_DIR})

# Adding our source files
file(GLOB_RECURSE PROJECT_SOURCES CONFIGURE_DEPENDS "${CMAKE_CURRENT_LIST_DIR}/src/*.cpp")
set(PROJECT_INCLUDE "${CMAKE_CURRENT_LIST_DIR}/src/")

# Declaring our executable
add_executable(${PROJECT_NAME})
target_sources(${PROJECT_NAME} PRIVATE ${PROJECT_SOURCES})
target_include_directories(${PROJECT_NAME} PRIVATE ${PROJECT_INCLUDE})

# Link raylib to your project
target_link_libraries(${PROJECT_NAME} PRIVATE raylib)

# Setting ASSETS_PATH
target_compile_definitions(${PROJECT_NAME} PUBLIC ASSETS_PATH="${CMAKE_CURRENT_SOURCE_DIR}/assets/")

# Conditional bundle creation for release builds
if(CMAKE_BUILD_TYPE STREQUAL "Release")
    set_target_properties(${PROJECT_NAME} PROPERTIES
            MACOSX_BUNDLE TRUE
            MACOSX_BUNDLE_GUI_IDENTIFIER net.brianbar.${PROJECT_NAME}
            MACOSX_BUNDLE_BUNDLE_NAME "${PROJECT_NAME}"
            MACOSX_BUNDLE_INFO_PLIST "${CMAKE_CURRENT_SOURCE_DIR}/Info.plist"
            MACOSX_BUNDLE_ICON_FILE "icon.icns"  # Set this if you have an icon file
    )
endif()
