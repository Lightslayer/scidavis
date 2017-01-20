# Define an SciDAVis executable
set(TARGET_NAME scidavis)
add_definitions(-DLEGACY_CODE_0_2_x)
add_executable(${TARGET_NAME} src/main.cpp)
target_include_directories(${TARGET_NAME} PUBLIC 
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../libscidavis/src/future>
	$<INSTALL_INTERFACE:include>
)

# Define the libraries this project depends upon
set(SCIDAVIS_LIBS
	lib${TARGET_NAME} 
	liborigin
	libmuparser libpng 
	libqwtplot3d 
	libqwt 
	libz
)
target_link_libraries(${TARGET_NAME} ${SCIDAVIS_LIBS})