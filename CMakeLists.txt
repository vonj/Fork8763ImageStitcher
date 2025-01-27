# CMakeLists.txt
cmake_minimum_required(VERSION 3.0)
project(ImageStitcher)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake")

find_package(OpenCV REQUIRED)
find_package(Qt5Widgets REQUIRED)

# assuming QT5 in
set(CMAKE_AUTOMOC ON)	##replace the qt5_wrap_cpp
set(CMAKE_AUTOUIC ON)	##replace the qt5_wrap_ui

set(SOURCES
  src/main.cpp
  src/mainview.cpp
  src/mainview.h
  src/mainview.ui
)

# imagestitcher target
add_executable(imagestitcher)
target_sources(imagestitcher PRIVATE ${SOURCES} )
target_link_libraries(imagestitcher PRIVATE ${OpenCV_LIBS} Qt5::Widgets)
set_property(TARGET imagestitcher PROPERTY CXX_STANDARD 20)

message(STATUS "Install directory: ${CMAKE_INSTALL_BINDIR}")

install(TARGETS imagestitcher DESTINATION bin)

if(WIN32 AND NOT CMAKE_CROSSCOMPILING)
	install(
		TARGETS imagestitcher
		RUNTIME_DEPENDENCIES
			PRE_EXCLUDE_REGEXES "api-ms-" "qt"
		DESTINATION bin)
	include(WinDeployQt)
	WinDeployQt(TARGET imagestitcher COMPILER_RUNTIME INCLUDE_MODULES ${QTLIBS} EXCLUDE_MODULES webkit webkit2)
	install(DIRECTORY ${PROJECT_BINARY_DIR}/windeployqt/
		DESTINATION bin)
endif()
