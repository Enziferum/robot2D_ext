cmake_minimum_required(VERSION 3.11)
project(robot2D_ext C)

set(CMAKE_C_STANDARD 99)

set(CONFIG_FILE "${CMAKE_SOURCE_DIR}/cmake/generated/${PROJECT_NAME}-config.cmake")
set(CONFIG_DEST "lib${LIB_SUFFIX}/cmake/${PROJECT_NAME}")
set(TARGETS_EXPORT_NAME "${PROJECT_NAME}Targets")


set(SRC
        include/robot2D/internal/glad.h
        include/robot2D/internal/khrplatform.h
        include/robot2D/internal/stb_image_aug.h
        include/robot2D/internal/stb_image.h
        include/robot2D/internal/stb_image_write.h
        )

set(SRC
        ${SRC}
        src/glad.c
        src/stb_image_aug.c
        )

add_library(${PROJECT_NAME} ${SRC})

target_include_directories(${PROJECT_NAME} PUBLIC
        "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/robot2D>"
        "$<INSTALL_INTERFACE:include>")


include(CMakePackageConfigHelpers)
configure_package_config_file(
        "${CMAKE_SOURCE_DIR}/cmake/robot2D_ext-config.cmake.in"
        ${CONFIG_FILE}
        INSTALL_DESTINATION ${CONFIG_DEST}
)

# Install
install(DIRECTORY ${CMAKE_SOURCE_DIR}/include/robot2D/internal DESTINATION include/robot2D)

install(TARGETS ${PROJECT_NAME} EXPORT ${TARGETS_EXPORT_NAME}
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib)

install(FILES ${CONFIG_FILE} DESTINATION ${CONFIG_DEST})

install( EXPORT ${TARGETS_EXPORT_NAME}
        DESTINATION "${CONFIG_DEST}")