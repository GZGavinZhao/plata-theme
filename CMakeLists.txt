project(plata-theme)
cmake_minimum_required(VERSION 3.20)

include(GNUInstallDirs)

option(GTK_NEXT "Enable GTK4 support" ON)
option(GNOME "Enable GNOME support" ON)
option(CINNAMON "Enable Cinnamon support" ON)
option(FLASHBACK "Enable Flashback support" ON)
option(XFCE "Enable XFCE support" ON)
option(MATE "Enable MATE support" ON)
option(OPENBOX "Enable Openbox support" ON)
option(PLANK "Enable Plank support" OFF)
option(TELEGRAM "Enable telegram support" OFF)
option(TWEETDECK "Enable Tweetdeck-Colordeck" OFF)
option(AIRFORSTEAM "Enable Air-for-Steam" OFF)
option(SASS_ONLY "Only compile stylesheets. For development uses only." OFF)

set(SELECTION_COLOR "#3f51b5" CACHE STRING "Selection (Primary) color")
set(ACCENT_COLOR "#7986cb" CACHE STRING "Accent (Secondary) color")
set(SUGGESTION_COLOR "#673ab7" CACHE STRING "Suggestion (Secondary) color")
set(DESTRUCTION_COLOR "#f44336" CACHE STRING "Destruction (Tertiary) color")

find_package(PkgConfig)

pkg_check_modules(GDK_PIXBUF REQUIRED gdk-pixbuf-2.0>=2.32.2)
pkg_check_modules(GLIB REQUIRED glib-2.0>=2.48.0)
find_program(GLIB_COMPILE_RESOURCES glib-compile-resources REQUIRED)
find_program(XMLLINT xmllint REQUIRED)
find_program(INKSCAPE inkscape REQUIRED)

if(TELEGRAM)
	find_program(ZIP zip REQUIRED)
endif()

pkg_check_modules(MARCO libmarco-private>=1.22.2)
if(NOT MARCO_FOUND)
	set(MATE OFF)
endif()

set(THEME_DIR "${CMAKE_INSTALL_DATADIR}/themes")

set(GTK_VERSIONS 2.0 3.20 3.22 3.24 4.0)
set(GTK_INSTALL_VERSIONS 2.0 3.0 3.22 3.24 4.0)
set(PLATA_VARIANTS Plata Plata-Compact Plata-Lumine Plata-Lumine-Compact Plata-Noir Plata-Noir-Compact)
set(PLATA_SCSS_FILES gtk-mixed.scss gtk-mixed-compact.scss gtk-light.scss gtk-light-compact.scss gtk-dark.scss gtk-dark-compact.scss)
set(PLATA_CSS_FILES gtk-mixed.css gtk-mixed-compact.css gtk-light.css gtk-light-compact.css gtk-dark.css gtk-dark-compact.css)

find_program(
	SASSC # Even though we're using Dart Sass, the name is still SASSC for legacy reasons.
	NAMES dart-sass sass
	PATHS "$ENV{HOME}/.pub-cache/bin"
)
if (NOT SASSC STREQUAL "SASSC-NOTFOUND")
	execute_process(
		COMMAND ${SASSC} --version
		OUTPUT_VARIABLE SASSC_VERSION
	)
	if(${SASSC_VERSION} MATCHES "Ruby")
		set(SASSC "SASSC-NOTFOUND")
	endif()
endif()
if(SASSC STREQUAL "SASSC-NOTFOUND")
	execute_process(COMMAND npm install)
	message("Installing Dart Sass from npm...")
	set(SASSC "${CMAKE_CURRENT_SOURCE_DIR}/.bin/sass")
endif()

set(SASSC_OPTIONS "--no-source-map" "--update")

subdirs(data)
subdirs(src/gtk)

# Install license

foreach(PLATA_VARIANT ${PLATA_VARIANTS})
	install(
		FILES COPYING LICENSE_CC_BY_SA4
		DESTINATION "${THEME_DIR}/${PLATA_VARIANT}"
	)
endforeach()
