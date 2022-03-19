project(plata-theme)
cmake_minimum_required(VERSION 3.20)

include(GNUInstallDirs)

set(THEME_DIR "${CMAKE_INSTALL_DATADIR}/themes")

# set(GTK_VERSIONS 2.0 3.20 3.22 3.24 4.0)
set(GTK_VERSIONS 3.20 3.22)
set(PLATA_VARIANTS Plata Plata-Compact Plata-Lumine Plata-Lumine-Compact Plata-Noir Plata-Noir-Compact)
set(PLATA_SCSS_FILES gtk-mixed.scss gtk-mixed-compact.scss gtk-light.scss gtk-light-compact.scss gtk-dark.scss gtk-dark-compact.scss)
set(PLATA_CSS_FILES gtk-mixed.css gtk-mixed-compact.css gtk-light.css gtk-light-compact.css gtk-dark.css gtk-dark-compact.css)

subdirs("src/gtk/sass")
