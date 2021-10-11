cmake_minimum_required(VERSION 3.19)

# Replace with local paths before building
set(MAX_SDK_DIR "<your-max-sdk-root>/source/max-sdk-base")
set(LIBLO_DIR "<your-liblo-root>")
set(C74_SUPPORT_DIR "${MAX_SDK_DIR}/c74support")
set(LIBLO_BUILD_DIR "${LIBLO_DIR}/cmake/build/Debug")
set(LIBLO_INCLUDES "${LIBLO_DIR}/cmake/build;${LIBLO_DIR}")

project(oscmulticast)

set(Liblo_LIB ${LIBLO_BUILD_DIR}/liblo.lib)
mark_as_advanced(Liblo_LIB)

set(THIS_FOLDER_NAME "oscmulticast")

include(${MAX_SDK_DIR}/script/max-pretarget.cmake)

message("Generating: ${THIS_FOLDER_NAME}")

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/build")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG "${CMAKE_LIBRARY_OUTPUT_DIRECTORY}/Debug")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE "${CMAKE_LIBRARY_OUTPUT_DIRECTORY}/Release")

add_definitions(
    -D_WINSOCK_DEPRECATED_NO_WARNINGS
    -DHAVE_WINSOCK2_H
    -DNODEFAULTLIB
    -DMAXMSP
)

#############################################################
# MAX EXTERNAL
#############################################################

include_directories( 
  "${MAX_SDK_INCLUDES}"
  "${MAX_SDK_MSP_INCLUDES}"
  "${LIBLO_INCLUDES}"
)

file(GLOB PROJECT_SRC
   "${CMAKE_CURRENT_SOURCE_DIR}/*.h"
   "${CMAKE_CURRENT_SOURCE_DIR}/*.c"
   "${CMAKE_CURRENT_SOURCE_DIR}/*.cpp"
)
add_library( 
  ${PROJECT_NAME} 
  MODULE
  ${PROJECT_SRC}
)

include(${MAX_SDK_DIR}/script/max-posttarget.cmake)

target_link_libraries(${PROJECT_NAME} PUBLIC ${Liblo_LIB})
target_link_libraries(${PROJECT_NAME} PUBLIC wsock32.lib ws2_32.lib iphlpapi.lib)

set_target_properties(${PROJECT_NAME} PROPERTIES LINK_FLAGS "/NODEFAULTLIB:MSVCRTD")