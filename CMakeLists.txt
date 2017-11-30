cmake_minimum_required(VERSION 3.0)

project(sndfile-tools C)

set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake/")

find_package(PkgConfig REQUIRED)
pkg_check_modules(SNDFILE REQUIRED sndfile)
pkg_check_modules(FFTW3 REQUIRED fftw3)
pkg_check_modules(SAMPLERATE REQUIRED samplerate)
pkg_check_modules(CAIRO REQUIRED cairo)
pkg_check_modules(JACK jack)


add_executable(sndfile-generate-chirp
  src/generate-chirp.c
  src/common.c
  src/common.h)

target_include_directories(sndfile-generate-chirp PRIVATE ${SNDFILE_INCLUDE_DIRS})
target_link_libraries(sndfile-generate-chirp ${SNDFILE_LIBRARIES} m)


add_executable(sndfile_spectrogram
  src/spectrogram.c
  src/window.c
  src/common.c
  src/window.h
  src/common.h
  src/spectrum.c
  src/spectrum.h)

target_include_directories(sndfile_spectrogram PRIVATE
  ${SNDFILE_INCLUDE_DIRS}
  ${FFTW3_INCLUDE_DIRS}
  ${CAIRO_INCLUDE_DIRS})

target_link_libraries(sndfile_spectrogram
  ${SNDFILE_LIBRARIES}
  ${FFTW3_LIBRARIES}
  ${CAIRO_LIBRARIES}
  m)


add_executable(sndfile_mix_to_mono
  src/mix-to-mono.c
  src/common.h
  src/common.c
  )

target_include_directories(sndfile_mix_to_mono PRIVATE ${SNDFILE_INCLUDE_DIRS})
target_link_libraries(sndfile_mix_to_mono ${SNDFILE_LIBRARIES} m)


add_executable(sndfile_waveform
  src/waveform.c
  src/common.c
  src/common.h
  )

target_include_directories(sndfile_waveform PRIVATE
  ${SNDFILE_INCLUDE_DIRS}
  ${CAIRO_INCLUDE_DIRS})

target_link_libraries(sndfile_waveform
  ${SNDFILE_LIBRARIES}
  ${CAIRO_LIBRARIES}
  m)
target_compile_definitions(sndfile_waveform PRIVATE PACKAGE_VERSION="cmake")


add_executable(
  sndfile_resample
  src/resample.c
  src/common.c
  src/common.h
)

target_include_directories(sndfile_resample PRIVATE
  ${SNDFILE_INCLUDE_DIRS}
  ${SAMPLERATE_INCLUDE_DIRS})

target_link_libraries(sndfile_resample
  ${SNDFILE_LIBRARIES}
  ${SAMPLERATE_LIBRARIES}
  m)



pkg_check_modules(JACK jack)


if (${JACK_FOUND})

  add_executable(
    sndfile-jackplay
    src/jackplay.c
  )

  target_include_directories(sndfile-jackplay PRIVATE
    ${SNDFILE_INCLUDE_DIRS}
    ${JACK_INCLUDE_DIRS})


  target_link_libraries(sndfile-jackplay
    ${SNDFILE_LIBRARIES}
    ${JACK_LIBRARIES}
    m)

endif(${JACK_FOUND})
  # TODO
#if HAVE_JACK
#bin_PROGRAMS += bin/sndfile-jackplay
#bin_sndfile_jackplay_SOURCES = src/jackplay.c
#bin_sndfile_jackplay_CFLAGS = $(SNDFILE_CFLAGS) $(JACK_CFLAGS)
#bin_sndfile_jackplay_LDADD = $(SNDFILE_LIBS) $(JACK_LIBS)
#endif
