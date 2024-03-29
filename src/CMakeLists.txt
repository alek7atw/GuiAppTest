cmake_minimum_required(VERSION 3.0.0)

file(GLOB_RECURSE SRC_CXX_FILES CONFIGURE_DEPENDS *.cpp)
file(GLOB_RECURSE SRC_C_FILES CONFIGURE_DEPENDS *.c)
file(GLOB_RECURSE INC_CXX_FILES CONFIGURE_DEPENDS ${PROJECT_SOURCE_DIR}/include*.cpp)
file(GLOB_RECURSE INC_C_FILES CONFIGURE_DEPENDS ${PROJECT_SOURCE_DIR}/include*.c)

file(GLOB_RECURSE Walnut ${PROJECT_SOURCE_DIR}/external/Walnut/Walnut/src/Walnut/*.cpp)

# Add source files to be compiled here
if(WIN32)
     if(CMAKE_BUILD_TYPE STREQUAL "Debug" OR CMAKE_BUILD_TYPE STREQUAL "RelWithDebInfo")
          add_executable(${PROJECT_NAME}
               ${SRC_CXX_FILES}
               ${SRC_C_FILES}
               ${INC_CXX_FILES}
               ${INC_C_FILES}

               ${Walnut}
          )

     else()
          add_executable(${PROJECT_NAME} WIN32
               ${SRC_CXX_FILES}
               ${SRC_C_FILES}
               ${INC_CXX_FILES}
               ${INC_C_FILES}

               ${Walnut}
          )
     endif()

else()
     add_executable(${PROJECT_NAME}
          ${SRC_CXX_FILES}
          ${SRC_C_FILES}
          ${INC_CXX_FILES}
          ${INC_C_FILES}
     )
endif()

message(STATUS `$<CONFIG> = ${CMAKE_BUILD_TYPE}`)

# Copy Binary to DEV specified location
add_custom_command(
     TARGET ${PROJECT_NAME} POST_BUILD
     COMMAND ${CMAKE_COMMAND} -E copy_directory
     $<TARGET_FILE_DIR:${PROJECT_NAME}>
     ${CMAKE_SOURCE_DIR}/bin/${CMAKE_GENERATOR_PLATFORM} # Change this to your Prefered location (I prefer ${CMAKE_SOURCE_DIR}/bin/$<CONFIG>/ or ${CMAKE_SOURCE_DIR}/bin/$<CMAKE_GENERATOR_PLATFORM>/)
)

# add_custom_command(
# TARGET ${PROJECT_NAME} POST_BUILD
# COMMAND ${CMAKE_COMMAND} -E create_symlink ${link_src} ${link_dst}
# DEPENDS ${link_dst}
# COMMENT "symbolic link ${link_src} => ${link_dst}"
# )
set(CMAKE_INCLUDE_CURRENT_DIR ON)