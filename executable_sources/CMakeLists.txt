cmake_minimum_required(VERSION 3.14...3.22)

project(olcTemplateExecutable LANGUAGES CXX)

# --- Import tools ----

include(../cmake/tools.cmake)

# ---- Dependencies ----

include(../cmake/CPM.cmake)


# ---- Create standalone executable ----

# Adds the custom library to CPM
CPMAddPackage(NAME olcTemplateLibraries SOURCE_DIR ${CMAKE_CURRENT_LIST_DIR}/../lib_sources)

file(GLOB sources CONFIGURE_DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/source/*.cpp)

add_executable(${PROJECT_NAME} ${sources})

set_target_properties(${PROJECT_NAME} PROPERTIES CXX_STANDARD 17 OUTPUT_NAME "olcTemplate")

target_link_libraries(${PROJECT_NAME} olcTemplateLibraries)

# If on Windows link to the Windows subsystem else link to the Linux subsystem
if(WIN32)
  target_link_libraries(${PROJECT_NAME} -luser32 -lgdi32 -lopengl32 -lgdiplus -lShlwapi -ldwmapi -lstdc++fs -std=c++17)
else()
  target_link_libraries(${PROJECT_NAME} -lX11 -lGL -lpthread -lpng -lstdc++fs -std=c++17)
endif()

target_include_directories(
  ${PROJECT_NAME} PUBLIC $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/../include>
                         $<INSTALL_INTERFACE:include/${PROJECT_NAME}-${PROJECT_VERSION}>
)
