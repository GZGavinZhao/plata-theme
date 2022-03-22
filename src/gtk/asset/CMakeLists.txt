configure_file(
	assets-gtk2.svg.in
	assets-gtk2.svg
)

configure_file(
	assets-gtk3.svg.in
	assets-gtk3.svg
)

set(GTK2_ASSETS_DIR assets-gtk2)
file(MAKE_DIRECTORY ${GTK2_ASSETS_DIR})
set(GTK3_ASSETS_DIR assets-gtk3)
file(MAKE_DIRECTORY ${GTK3_ASSETS_DIR})

set(INK_EXPORT_OPTION "--export-filename")
set(SRC_FILE_GTK3 assets-gtk3.svg)
set(NON_SCALE_DPI 96)
set(SCALE_DPI 192)

# Get Inkscape's version
execute_process(
	COMMAND ${INKSCAPE} --version
	OUTPUT_VARIABLE INKSCAPE_RAW_VERSION
)
separate_arguments(INKSCAPE_VERSION_LIST NATIVE_COMMAND "${INKSCAPE_RAW_VERSION}")
list(GET INKSCAPE_VERSION_LIST 1 INKSCAPE_VERSION)

if(INKSCAPE_VERSION VERSION_LESS 0.92)
	set(NON_SCALE_DPI 90)
	set(NON_SCALE_DPI 180)
endif()

if(INKSCAPE_VERSION VERSION_LESS 1.0)
	set(INK_EXPORT_OPTION "--export-png")
endif()

# Get all the fricking items we need to export.
execute_process(
	COMMAND cat "${CMAKE_CURRENT_SOURCE_DIR}/assets-gtk3-scripts/assets-gtk3.txt"
	OUTPUT_VARIABLE RAW_TARGET_LIST
)
separate_arguments(TARGET_LIST NATIVE_COMMAND "${RAW_TARGET_LIST}")

foreach(TARGET ${TARGET_LIST})
	# Render non-scale assets
	add_custom_command(
		OUTPUT "${GTK3_ASSETS_DIR}/${TARGET}.png"
		COMMAND ${INKSCAPE} "--export-id=${TARGET}" "--export-dpi=${NON_SCALE_DPI}" --export-id-only "${INK_EXPORT_OPTION}=${GTK3_ASSETS_DIR}/${TARGET}.png" ${SRC_FILE_GTK3}
		DEPENDS ${SRC_FILE_GTK3}
	)

	add_custom_target(
		"gtk3-${TARGET}.png"
		ALL
		DEPENDS "${GTK3_ASSETS_DIR}/${TARGET}.png"
	)

	# Render scaled assets (x2)
	add_custom_command(
		OUTPUT "${GTK3_ASSETS_DIR}/${TARGET}@2.png"
		COMMAND ${INKSCAPE} "--export-id=${TARGET}" "--export-dpi=${SCALE_DPI}" --export-id-only "${INK_EXPORT_OPTION}=${GTK3_ASSETS_DIR}/${TARGET}@2.png" ${SRC_FILE_GTK3}
		DEPENDS ${SRC_FILE_GTK3}
	)

	add_custom_target(
		"gtk3-${TARGET}x2.png"
		ALL
		DEPENDS "${GTK3_ASSETS_DIR}/${TARGET}@2.png"
	)
endforeach()
