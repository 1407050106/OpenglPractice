cmake_minimum_required(VERSION 3.4) # 指定运行此配置文件所需的 CMake 的最低版本

set(execDataCollectName GL_DAQ)
project(${execDataCollectName}) #表示项目的名称

set(CMAKE_CXX_STANDARD 11)

if(APPLE)
    add_definitions(-D__APPLE__)
elseif(WIN32)
    add_definitions(-D__WIN32__)
endif()

file(GLOB_RECURSE SRC_FILES glad.c material_2_n.cpp)
file(GLOB IMAGE_FILES stb_image.h stb_image.cpp)

find_package(OpenGL REQUIRED)
set(SYS_FRAMEWORKS_DIR /System/Library/Frameworks)
set(COCOA_LIBS ${SYS_FRAMEWORKS_DIR}/Cocoa.framework)
set(COREVIDEO_LIBS ${SYS_FRAMEWORKS_DIR}/CoreVideo.framework)
set(IOKIT_LIBS ${SYS_FRAMEWORKS_DIR}/IOKit.framework)

include_directories(
        ${CMAKE_CURRENT_SOURCE_DIR}
        "/opt/homebrew/opt/opencv/include/opencv4"
        "3rd_party/glfw/include"
        "3rd_party/glad/include"
        "includes"
        "camera"
        ${OPENGL_INCLUDE_DIR}
        )

link_directories(
        "/opt/homebrew/opt/opencv/lib"
        "3rd_party/glfw/lib"
        )


# 指定生成目标
add_executable(${execDataCollectName} ${IMAGE_FILES} ${SRC_FILES})
# 添加链接库
target_link_libraries(${execDataCollectName} -lopencv_core -lopencv_highgui -lopencv_imgproc -lopencv_imgcodecs 
                        glfw3 ${OPENGL_LIBRARIES} ${COCOA_LIBS} ${COREVIDEO_LIBS} ${IOKIT_LIBS})

file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/3rd_party/glfw/lib/libglfw.3.dylib 
    DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
file(GLOB GLSL_FILES ${CMAKE_CURRENT_SOURCE_DIR}/glsl/*)
file(COPY ${GLSL_FILES} DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
# file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/glsl/texturef.fs
#     DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
# file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/glsl/wall.jpg
#     DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
# file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/glsl/5.1.transform.vs
#     DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
# file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/glsl/5.1.transform.fs
#     DESTINATION ${CMAKE_CURRENT_BINARY_DIR})
# file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/glsl/laugh_face.png
#     DESTINATION ${CMAKE_CURRENT_BINARY_DIR})