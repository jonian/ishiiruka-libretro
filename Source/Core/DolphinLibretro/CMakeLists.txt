
add_library(dolphin_libretro SHARED
  Boot.cpp
  Host.cpp
  Input.cpp
  Log.cpp
  Main.cpp
  Options.cpp
  Video.cpp
)

if (NOT APPLE)
   target_sources(dolphin_libretro PRIVATE
      Vulkan.cpp
   )
endif()

include_directories(${CMAKE_SOURCE_DIR}/Externals/Libretro/Include)
include_directories(${CMAKE_SOURCE_DIR}/Externals/Vulkan/Include)

set(LIBRARY_OUTPUT_PATH "${CMAKE_RUNTIME_OUTPUT_DIRECTORY}")

set_target_properties(dolphin_libretro PROPERTIES PREFIX "")
if(ANDROID)
    target_sources(dolphin_libretro PRIVATE
   )
   set_target_properties(dolphin_libretro PROPERTIES SUFFIX "_android.so")
endif()

if(NOT MSVC)
   set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -Wl,--no-undefined")
endif()

target_link_libraries(dolphin_libretro PRIVATE
  core
  uicommon
  ${LIBS}
)