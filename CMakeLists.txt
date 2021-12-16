cmake_minimum_required(VERSION 3.5)
project(spectralizer VERSION 1.3.4)
set(PLUGIN_AUTHOR "univrsal")
set(PLUGIN_GIT spectralizer)
set(LINUX_MAINTAINER_EMAIL "uni@vrsal.cf")
set(MACOS_BUNDLEID "cf.vrsal.spectralizer")
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_PREFIX_PATH "${QTDIR}")
string(TIMESTAMP TODAY "%Y.%m.%d %H:%M")
add_definitions(-DBUILD_TIME="${TODAY}")
add_definitions(-DSPECTRALIZER_VERSION="${PROJECT_VERSION}")

add_library(spectralizer MODULE)
add_library(OBS::spectralizer ALIAS spectralizer)

target_sources(spectralizer PRIVATE 
    src/spectralizer.cpp
    src/source/visualizer_source.cpp
    src/source/visualizer_source.hpp
    src/util/util.hpp
    src/util/util.cpp
    src/util/audio/spectrum_visualizer.cpp
    src/util/audio/spectrum_visualizer.hpp
    src/util/audio/bar_visualizer.cpp
    src/util/audio/bar_visualizer.hpp
    src/util/audio/circle_bar_visualizer.cpp
    src/util/audio/circle_bar_visualizer.hpp
    src/util/audio/wire_visualizer.cpp
    src/util/audio/wire_visualizer.hpp
    src/util/audio/fifo.cpp
    src/util/audio/fifo.hpp
    src/util/audio/obs_internal_source.cpp
    src/util/audio/obs_internal_source.hpp
    src/util/audio/audio_visualizer.cpp
    src/util/audio/audio_visualizer.hpp
    src/util/audio/audio_source.hpp)

find_path(FFTW_INCLUDE_DIRS fftw3.h)
find_library(FFTW_LIBRARIES fftw3)
target_link_libraries(spectralizer
    OBS::libobs
    OBS::frontend-api
    ${FFTW_LIBRARIES}
    ${spectralizer_PLATFORM_DEPS})

include_directories(${FFTW_INCLUDE_DIRS}
    "${LIBOBS_INCLUDE_DIR}/../UI/obs-frontend-api"
    ${LIBOBS_INCLUDE_DIR}
)

target_compile_features(spectralizer PRIVATE cxx_std_17)

set_target_properties(spectralizer PROPERTIES FOLDER "plugins/spectralizer" PREFIX "")

setup_plugin_target(spectralizer)
