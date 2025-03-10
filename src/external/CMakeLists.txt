# Copyright 2019, 2020, Collabora, Ltd.
# Copyright 2019, 2020, Visual Computing Lab, ISTI - Italian National Research Council
# SPDX-License-Identifier: BSL-1.0

option(
	MESHLAB_ALLOW_OPTIONAL_EXTERNAL_LIBRARIES
	"Allow to use/build optional external libraries"
	ON)

option(
	MESHLAB_BUILD_EXTERNAL_SOURCES_WITHOUT_WARNINGS
	"Should warnings be disabled on external libraries code?"
	ON)

set(MESHLAB_EXTERNAL_BINARY_DIR "${CMAKE_CURRENT_BINARY_DIR}/downloads")
set(MESHLAB_EXTERNAL_DIR ${CMAKE_CURRENT_SOURCE_DIR})
set(MESHLAB_EXTERNAL_DOWNLOAD_DIR "${MESHLAB_EXTERNAL_DIR}/downloads")

add_library(external-disable-warnings INTERFACE)
if(MESHLAB_BUILD_EXTERNAL_SOURCES_WITHOUT_WARNINGS)
	if(MSVC)
		# TODO
	elseif("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU" OR "${CMAKE_CXX_COMPILER_ID}" MATCHES "Clang")
		target_compile_options(external-disable-warnings INTERFACE -w)
	endif()
endif()

## REQUIRED EXTERNAL LIBS ##

# GLEW - required
include(${CMAKE_CURRENT_SOURCE_DIR}/glew.cmake)

# EasyExif - required
include(${CMAKE_CURRENT_SOURCE_DIR}/easyexif.cmake)


## OPTIONAL EXTERNAL LIBS ##

if ((NOT MESHLAB_BUILD_MINI) AND MESHLAB_ALLOW_OPTIONAL_EXTERNAL_LIBRARIES)

	message(STATUS "Searching for optional components")

	# boost - optional for filter_mesh_booleans
	include(${CMAKE_CURRENT_SOURCE_DIR}/boost.cmake)

	# cgal - optional for filter_mesh_booleans
	include(${CMAKE_CURRENT_SOURCE_DIR}/cgal.cmake)

	# embree - optional for filter_embree
	include(${CMAKE_CURRENT_SOURCE_DIR}/embree.cmake)

	# levmar - optional, for several plugins
	include(${CMAKE_CURRENT_SOURCE_DIR}/levmar.cmake)

	# lib3ds - optional, for io_3ds
	include(${CMAKE_CURRENT_SOURCE_DIR}/lib3ds.cmake)

	# libigl - optional for filter_mesh_booleans
	include(${CMAKE_CURRENT_SOURCE_DIR}/libigl.cmake)

	# muparser - optional, for filter_func
	include(${CMAKE_CURRENT_SOURCE_DIR}/muparser.cmake)

	# nexus - optional. for io_nxs
	include(${CMAKE_CURRENT_SOURCE_DIR}/nexus.cmake)

	# newuoa - optional and header-only, for several plugins including all that use levmar
	include(${CMAKE_CURRENT_SOURCE_DIR}/newuoa.cmake)

	# OpenCTM - optional, for io_ctm
	include(${CMAKE_CURRENT_SOURCE_DIR}/openctm.cmake)

	# qhull - optional, for filter_qhull
	include(${CMAKE_CURRENT_SOURCE_DIR}/qhull.cmake)

	# structure-synth - optional, for filter_ssynth
	include(${CMAKE_CURRENT_SOURCE_DIR}/ssynth.cmake)

	# tiny glTF - optional, for io_gltf
	include(${CMAKE_CURRENT_SOURCE_DIR}/tinygltf.cmake)

	# u3d - optional, for io_u3d
	include(${CMAKE_CURRENT_SOURCE_DIR}/u3d.cmake)

	# xerces library - optional, needed by libe57
	include(${CMAKE_CURRENT_SOURCE_DIR}/xerces.cmake)

	# libe57Format - optional, for io_e57
	include(${CMAKE_CURRENT_SOURCE_DIR}/e57.cmake)
endif()
