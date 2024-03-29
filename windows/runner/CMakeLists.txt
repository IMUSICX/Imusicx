cmake_minimum_required(VERSION 3.15)
project(runner LANGUAGES CXX)

add_executable(${BINARY_NAME} WIN32
  "flutter_window.cpp"
  "main.cpp"
  "utils.cpp"
  "win32_window.cpp"
  "${FLUTTER_MANAGED_DIR}/generated_plugin_registrant.cc"
  "Runner.rc"
  "runner.exe.manifest"
)
apply_standard_settings(${BINARY_NAME})
target_compile_definitions(${BINARY_NAME} PRIVATE "NOMINMAX")
target_link_libraries(${BINARY_NAME} PRIVATE flutter flutter_wrapper_app)
target_include_directories(${BINARY_NAME} PRIVATE "${CMAKE_SOURCE_DIR}")
add_dependencies(${BINARY_NAME} flutter_assemble)

add_custom_command(
  TARGET ${BINARY_NAME} PRE_BUILD
  COMMAND ${CMAKE_COMMAND} -E copy "${CMAKE_CURRENT_SOURCE_DIR}/../bin/msvcp140.dll" "$<TARGET_FILE_DIR:${BINARY_NAME}>/msvcp140.dll"
  COMMAND ${CMAKE_COMMAND} -E copy "${CMAKE_CURRENT_SOURCE_DIR}/../bin/vcruntime140.dll" "$<TARGET_FILE_DIR:${BINARY_NAME}>/vcruntime140.dll"
  COMMAND ${CMAKE_COMMAND} -E copy "${CMAKE_CURRENT_SOURCE_DIR}/../bin/vcruntime140_1.dll" "$<TARGET_FILE_DIR:${BINARY_NAME}>/vcruntime140_1.dll"
)
