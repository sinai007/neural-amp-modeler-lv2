cmake_minimum_required(VERSION 3.10)

project(NeuralAmpModelerLv2 VERSION 0.0.1)

set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED OFF)
set(CMAKE_CXX_EXTENSIONS OFF)

if (CMAKE_SYSTEM_NAME STREQUAL "Darwin")
	include_directories(SYSTEM /usr/local/include)
elseif (CMAKE_SYSTEM_NAME STREQUAL "Linux")
elseif (CMAKE_SYSTEM_NAME STREQUAL "Windows")
	add_compile_definitions(NOMINMAX WIN32_LEAN_AND_MEAN)
else()
	message(FATAL_ERROR "Unrecognized Platform!")
endif()

set(NAM_LV2_ID http://github.com/mikeoliphant/neural-amp-modeler-lv2)

include_directories(SYSTEM deps/eigen)
include_directories(SYSTEM deps/lv2/include)
include_directories(SYSTEM deps/NeuralAmpModelerCore/NAM)
include_directories(SYSTEM deps/json)
include_directories(SYSTEM deps/denormal)

add_subdirectory(src)


# create neural_amp_modeler.lv2
add_custom_target(copy_binaries ALL
	${CMAKE_COMMAND} -E copy "$<TARGET_FILE:neural_amp_modeler>" neural_amp_modeler.lv2/
	DEPENDS neural_amp_modeler
)

configure_file(resources/manifest.ttl.in neural_amp_modeler.lv2/manifest.ttl)
configure_file(resources/neural_amp_modeler.ttl.in neural_amp_modeler.lv2/neural_amp_modeler.ttl)

