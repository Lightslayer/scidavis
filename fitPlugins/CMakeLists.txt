cmake_minimum_required(VERSION 2.8)

# Simply include all the predefined plugins here
foreach(FIT_PLUGIN_LIB fitRational0 fitRational1 explin exp_saturation planck_wavelength)
	set(HEADERS file(GLOB ${CMAKE_CURRENT_SOURCE_DIR}/${FIT_PLUGIN_LIB}/*.hpp))
    add_library(lib${FIT_PLUGIN_LIB} INTERFACE
		${FIT_PLUGIN_LIB}/*.cpp
		${HEADERS}
	)
	
	# Define headers for this library. PUBLIC headers are used for
	# compiling the library, and will be added to consumers' build
	# paths.
	target_include_directories(lib${FIT_PLUGIN_LIB} PUBLIC
		$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/${FIT_PLUGIN_LIB}/include>
		$<INSTALL_INTERFACE:include>
		PRIVATE src
	)

	# If we have compiler requirements for this library, list them
	# here
	#target_compile_features(lib${FIT_PLUGIN_LIB}
	#	PUBLIC cxx_auto_type
	#	PRIVATE cxx_variadic_templates)

	# Depend on a library that we defined in the top-level file
	target_link_libraries(lib${FIT_PLUGIN_LIB}
		boost
	)

	# 'make install' to the correct location
	install(TARGETS lib${FIT_PLUGIN_LIB} EXPORT ${FIT_PLUGIN_LIB}Config
		ARCHIVE  DESTINATION lib
		LIBRARY  DESTINATION lib
		RUNTIME  DESTINATION bin)  # This is for Windows
	install(FILES ${HEADERS} DESTINATION include)

	# This makes the project importable from the install directory
	# Put config file in per-project dir (name MUST match), can also
	# just go into <prefix>/cmake.
	install(EXPORT MyLibraryConfig DESTINATION share/MyLibrary/cmake)

	# This makes the project importable from the build directory
	export(TARGETS lib FILE MyLibraryConfig.cmake)
endforeach()