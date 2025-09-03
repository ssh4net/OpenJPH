
# Add TIFF library and dependencies
############################################################
if( OJPH_ENABLE_TIFF_SUPPORT AND (NOT EMSCRIPTEN))
  # Set CMAKE_PREFIX_PATH to help find TIFF
  if(CMAKE_PREFIX_PATH)
    list(APPEND CMAKE_PREFIX_PATH ${CMAKE_PREFIX_PATH})
  endif()
  
  # Force CMake to use config files instead of Find modules
  find_package(TIFF REQUIRED CONFIG)
  
  if( TIFF_FOUND )
    set(USE_TIFF TRUE CACHE BOOL "Add TIFF support")
    add_definitions(-DOJPH_ENABLE_TIFF_SUPPORT)
    
    # Add TIFF include directories
    include_directories(${TIFF_INCLUDE_DIRS})
    if(NOT TIFF_INCLUDE_DIRS)
      include_directories(${CMAKE_INCLUDE_PATH})
    endif()
    
      # Debug: Check what targets are available
  message(STATUS "TIFF_FOUND: ${TIFF_FOUND}")
  message(STATUS "TIFF_LIBRARIES: ${TIFF_LIBRARIES}")
  message(STATUS "TIFF_INCLUDE_DIRS: ${TIFF_INCLUDE_DIRS}")
  message(STATUS "CMAKE_PREFIX_PATH: ${CMAKE_PREFIX_PATH}")
  
  # Check if TIFF::tiff target exists
  if(TARGET TIFF::tiff)
    message(STATUS "TIFF::tiff target found")
  else()
    message(STATUS "TIFF::tiff target NOT found, using TIFF_LIBRARIES")
    # Fallback to using TIFF_LIBRARIES directly
    set(TIFF_TARGET ${TIFF_LIBRARIES})
  endif()
  endif()
endif()
############################################################

if (EMSCRIPTEN)
  add_link_options(-sWASM=1 -sASSERTIONS=1 -sALLOW_MEMORY_GROWTH=1 -sNODERAWFS=1 -sENVIRONMENT=node -sEXIT_RUNTIME=1 -sEXCEPTION_CATCHING_ALLOWED=['fake'])
endif()

## Build executables
add_subdirectory(ojph_expand)
add_subdirectory(ojph_compress)
if (OJPH_BUILD_STREAM_EXPAND)
  add_subdirectory(ojph_stream_expand)
endif()