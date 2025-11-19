cmake_minimum_required(VERSION 3.10)
project(BasicAppSetup)

set(source_files 
  main.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/ImGui/imgui.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/ImGui/imgui_demo.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/ImGui/imgui_draw.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/ImGui/imgui_tables.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/ImGui/imgui_widgets.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/ImGui/backends/imgui_impl_sdl3.cpp
  ${CMAKE_CURRENT_SOURCE_DIR}/ImGui/backends/imgui_impl_opengl3.cpp
)

add_executable(${PROJECT_NAME} ${source_files})

target_include_directories(${PROJECT_NAME} PRIVATE
  ${CMAKE_CURRENT_SOURCE_DIR}/
  ${CMAKE_CURRENT_SOURCE_DIR}/ImGui/
  ${CMAKE_CURRENT_SOURCE_DIR}/ImGui/backends
  ${CMAKE_CURRENT_SOURCE_DIR}/SDL3/include
)

target_link_directories(${PROJECT_NAME} PRIVATE
  ${CMAKE_CURRENT_SOURCE_DIR}/SDL3/lib
)

target_link_libraries(${PROJECT_NAME} PRIVATE
  SDL3
  opengl32
)

add_custom_command(
    TARGET ${PROJECT_NAME} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
        "${CMAKE_CURRENT_SOURCE_DIR}/dlls/SDL3.dll"
        $<TARGET_FILE_DIR:${PROJECT_NAME}>
)

add_custom_target(run
    COMMAND ${PROJECT_NAME} 
    WORKING_DIRECTORY $<TARGET_FILE_DIR:${PROJECT_NAME}>
)
add_dependencies(run ${PROJECT_NAME})
