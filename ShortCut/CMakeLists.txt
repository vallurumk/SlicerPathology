cmake_minimum_required(VERSION 3.0.1)
if(WIN32)
  set(CMAKE_CXX_FLAGS_DEBUG "/D_DEBUG /MDd /Zi /Ob0 /Od /RTC1" CACHE STRING "extra compiler flags for debug" FORCE )
  set(CMAKE_C_FLAGS_DEBUG "/D_DEBUG /MDd /Zi /Ob0 /Od /RTC1" CACHE STRING "extra compiler flags for debug" FORCE )
else(WIN32)
  set(CMAKE_C_FLAGS_DEBUG "-pedantic " CACHE STRING "extra compiler flags for debug" FORCE )
  if(LINUX)
    set(CMAKE_C_FLAGS "-fPIC -g3 -Wall -pipe -DVTK_EXCLUDE_STRSTREAM_HEADERS -Wl,--no-undefined" CACHE STRING "compiler flags always used" FORCE)
    set(CMAKE_CXX_FLAGS "-fPIC -Wall -pipe -DVTK_EXCLUDE_STRSTREAM_HEADERS -Wl,--no-undefined" CACHE STRING "compiler flags always used" FORCE)
  else(LINUX)
    set(CMAKE_C_FLAGS "-fPIC -g3 -Wall -pipe -DVTK_EXCLUDE_STRSTREAM_HEADERS" CACHE STRING "compiler flags always used" FORCE)
    set(CMAKE_CXX_FLAGS "-fPIC -Wall -pipe -DVTK_EXCLUDE_STRSTREAM_HEADERS" CACHE STRING "compiler flags always used" FORCE)
  endif(LINUX)
  set(CMAKE_CXX_FLAGS_DEBUG "-g3 " CACHE STRING "extra compiler flags for debug" FORCE )
endif(WIN32)

#-----------------------------------------------------------------------------
set(MODULE_NAME ShortCut)
set(MODULE_TITLE ${MODULE_NAME})

string(TOUPPER ${MODULE_NAME} MODULE_NAME_UPPER)

#-----------------------------------------------------------------------------
add_subdirectory(Logic)
add_subdirectory(Widgets)

#-----------------------------------------------------------------------------
set(MODULE_EXPORT_DIRECTIVE "Q_SLICER_QTMODULES_${MODULE_NAME_UPPER}_EXPORT")

# Current_{source,binary} and Slicer_{Libs,Base} already included
set(MODULE_INCLUDE_DIRECTORIES
  ${CMAKE_CURRENT_SOURCE_DIR}/Logic
  ${CMAKE_CURRENT_BINARY_DIR}/Logic
  ${CMAKE_CURRENT_SOURCE_DIR}/Widgets
  ${CMAKE_CURRENT_BINARY_DIR}/Widgets
  )

set(MODULE_SRCS
  qSlicer${MODULE_NAME}Module.cxx
  qSlicer${MODULE_NAME}Module.h
  qSlicer${MODULE_NAME}ModuleWidget.cxx
  qSlicer${MODULE_NAME}ModuleWidget.h
  )

set(MODULE_MOC_SRCS
  qSlicer${MODULE_NAME}Module.h
  qSlicer${MODULE_NAME}ModuleWidget.h
  )

set(MODULE_UI_SRCS
  Resources/UI/qSlicer${MODULE_NAME}ModuleWidget.ui
  )

set(MODULE_TARGET_LIBRARIES
  vtkSlicer${MODULE_NAME}ModuleLogic
  qSlicer${MODULE_NAME}ModuleWidgets
  )

set(MODULE_RESOURCES
  Resources/qSlicer${MODULE_NAME}Module.qrc
  )

#-----------------------------------------------------------------------------
slicerMacroBuildQtModule(
  NAME ${MODULE_NAME}
  TITLE ${MODULE_TITLE}
  EXPORT_DIRECTIVE ${MODULE_EXPORT_DIRECTIVE}
  INCLUDE_DIRECTORIES ${MODULE_INCLUDE_DIRECTORIES}
  SRCS ${MODULE_SRCS}
  MOC_SRCS ${MODULE_MOC_SRCS}
  UI_SRCS ${MODULE_UI_SRCS}
  TARGET_LIBRARIES ${MODULE_TARGET_LIBRARIES}
  RESOURCES ${MODULE_RESOURCES}
  WITH_GENERIC_TESTS
  )

#-----------------------------------------------------------------------------
#if(BUILD_TESTING)
#  add_subdirectory(Testing)
#endif()

