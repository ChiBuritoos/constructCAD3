file(GLOB SOURCES_DIR src/*.cpp src/*.h src/gui/*.cpp src/gui/*.h src/System/*.cpp src/System/*.h)
#####
if(UNIX)
cmake_minimum_required (VERSION 2.6)
project (constructCAD)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -std=c++11")
set(CMAKE_EXE_LINKER_FLAGS "-static-libgcc -static-libstdc++ ")

add_definitions(-DGLEW_STATIC)


find_package(OpenGL REQUIRED)



# Compile external dependencies 
add_subdirectory (lib)

# On Visual 2005 and above, this module can set the debug working directory
cmake_policy(SET CMP0026 OLD)
list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/lib/rpavlik-cmake-modules-fe2273")
include(CreateLaunchers)
include(MSVCMultipleProcessCompile) # /MP
 


include_directories(
	
	lib/glm-0.9.7.1/
	lib/glew-1.13.0/include/
	lib/glew-1.13.0/include/GL
	)


find_package(SDL2 REQUIRED)
include_directories(${SDL2_INCLUDE_DIR})

find_package(SDL_net REQUIRED)
include_directories(${SDL_NET_INCLUDE_DIR})


set(ALL_LIBS
	${OPENGL_LIBRARY}
	${SDL2_LIBRARY}
	${SDL_NET_LIBRARY}
	GLEW_1130
)

add_definitions(
	-DTW_STATIC
	-DTW_NO_LIB_PRAGMA
	-DTW_NO_DIRECT3D
	-DGLEW_STATIC
	-D_CRT_SECURE_NO_WARNINGS
)


add_executable(${PROJECT_NAME} ${SOURCES_DIR} ${PROJECT_SOURCE_DIR}/resources/${PROJECT_NAME}.rc)

target_link_libraries(${PROJECT_NAME} ${ALL_LIBS} )
# Xcode and Visual working directories
set_target_properties(${PROJECT_NAME} PROPERTIES XCODE_ATTRIBUTE_CONFIGURATION_BUILD_DIR "${CMAKE_CURRENT_SOURCE_DIR}/${PROJECT_NAME}/")
create_target_launcher(${PROJECT_NAME} WORKING_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/${PROJECT_NAME}/")
endif(UNIX)
############################################################











#############################################################
if(WIN32)
cmake_minimum_required (VERSION 3.0.0)
#-std=c++11
project(constructCAD LANGUAGES C CXX)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -mwindows -std=c++11")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall ")
set(CMAKE_EXE_LINKER_FLAGS "-static-libgcc -static-libstdc++ -static ")

add_definitions(-DGLEW_STATIC)

 




add_executable(${PROJECT_NAME} ${SOURCES_DIR} ${PROJECT_SOURCE_DIR}/resources/${PROJECT_NAME}.rc)


set(GLEW_INCLUDE_DIRS ${PROJECT_SOURCE_DIR}/lib/glew-1.13.0/include)
set(GLEW_LIBRARY ${PROJECT_SOURCE_DIR}/lib/glew-1.13.0/lib/libglew32.a)
set(OPENGL_LIBRARY ${PROJECT_SOURCE_DIR}/lib/glew-1.13.0/lib/OpenGL32.Lib)


set(GLM_INCLUDE_DIRS ${PROJECT_SOURCE_DIR}/lib/glm-0.9.7.1)

set(SDL_INCLUDE_DIRS ${PROJECT_SOURCE_DIR}/lib/SDL2-2.0.4/i686-w64-mingw32/include/SDL2)
      
	 
	  set(PLATFORM_LIBRARIES
                  ${PROJECT_SOURCE_DIR}/lib/MinGW/lib/libmingw32.a
		  ${PROJECT_SOURCE_DIR}/lib/glew-1.13.0/lib/libglew32.a
		  ${PROJECT_SOURCE_DIR}/lib/SDL2-2.0.4/i686-w64-mingw32/lib/libSDL2main.a
		  ${PROJECT_SOURCE_DIR}/lib/SDL2-2.0.4/i686-w64-mingw32/lib/libSDL2.a
                  ${PROJECT_SOURCE_DIR}/lib/MinGW/lib/libgdi32.a
                  ${PROJECT_SOURCE_DIR}/lib/MinGW/lib/libimm32.a
                  ${PROJECT_SOURCE_DIR}/lib/MinGW/lib/libole32.a
                  ${PROJECT_SOURCE_DIR}/lib/MinGW/lib/liboleaut32.a
                  ${PROJECT_SOURCE_DIR}/lib/MinGW/lib/libuuid.a
                  ${PROJECT_SOURCE_DIR}/lib/MinGW/lib/libversion.a
                  ${PROJECT_SOURCE_DIR}/lib/MinGW/lib/libwinmm.a)

include_directories( ${GLEW_INCLUDE_DIRS} ${SDL_INCLUDE_DIRS} ${GLM_INCLUDE_DIRS}) 


target_link_libraries(${PROJECT_NAME} ${GLEW_LIBRARY} ${OPENGL_LIBRARY} ${PLATFORM_LIBRARIES})
endif(WIN32)




