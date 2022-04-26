configure_file(
	"${CMAKE_CURRENT_SOURCE_DIR}/assets-gtk2.svg.in"
	"${CMAKE_CURRENT_SOURCE_DIR}/assets-gtk2.svg"
)

configure_file(
	"${CMAKE_CURRENT_SOURCE_DIR}/assets-gtk3.svg.in"
	"${CMAKE_CURRENT_SOURCE_DIR}/assets-gtk3.svg"
)

# set(GTK2_ASSETS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/assets-gtk2")
# file(MAKE_DIRECTORY ${GTK2_ASSETS_DIR}/Shadows ${GTK2_ASSETS_DIR}/Others)
# set(GTK3_ASSETS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/assets-gtk3")
# file(MAKE_DIRECTORY ${GTK3_ASSETS_DIR})
# 
# foreach(PLATA_VARIANT ${PLATA_VARIANTS})
# 	foreach(GTK_VER ${GTK_INSTALL_VERSIONS})
# 		if(GTK_VER STREQUAL "2.0")
# 			continue()
# 		endif()
# 		file(MAKE_DIRECTORY "${CMAKE_BINARY_DIR}/${PLATA_VARIANT}/gtk-${GTK_VER}")
# 		file(CREATE_LINK
# 			${GTK3_ASSETS_DIR}
# 			"${CMAKE_BINARY_DIR}/${PLATA_VARIANT}/gtk-${GTK_VER}/assets"
# 			SYMBOLIC
# 		)
# 	endforeach()
# endforeach()
# 
# set(INK_EXPORT_OPTION "--export-filename")
# set(SRC_FILE_GTK2 assets-gtk2.svg)
# set(SRC_FILE_GTK3 assets-gtk3.svg)
# set(NON_SCALE_DPI 96)
# set(SCALE_DPI 192)
# 
# # Get Inkscape's version
# execute_process(
# 	COMMAND ${INKSCAPE} --version
# 	OUTPUT_VARIABLE INKSCAPE_RAW_VERSION
# )
# separate_arguments(INKSCAPE_VERSION_LIST NATIVE_COMMAND "${INKSCAPE_RAW_VERSION}")
# list(GET INKSCAPE_VERSION_LIST 1 INKSCAPE_VERSION)
# 
# if(INKSCAPE_VERSION VERSION_LESS 0.92)
# 	set(NON_SCALE_DPI 90)
# 	set(NON_SCALE_DPI 180)
# endif()
# 
# if(INKSCAPE_VERSION VERSION_LESS 1.0)
# 	set(INK_EXPORT_OPTION "--export-png")
# endif()
# 
# # Get all the fricking items we need to export.
# execute_process(
# 	COMMAND cat "${CMAKE_CURRENT_SOURCE_DIR}/assets-gtk3-scripts/assets-gtk3.txt"
# 	OUTPUT_VARIABLE RAW_GTK3_TARGET_LIST
# )
# separate_arguments(GTK3_TARGET_LIST NATIVE_COMMAND "${RAW_GTK3_TARGET_LIST}")
# execute_process(
# 	COMMAND cat "${CMAKE_CURRENT_SOURCE_DIR}/assets-gtk2-scripts/assets-gtk2.txt"
# 	OUTPUT_VARIABLE RAW_GTK2_TARGET_LIST
# )
# separate_arguments(GTK2_TARGET_LIST NATIVE_COMMAND "${RAW_GTK2_TARGET_LIST}")
# 
# foreach(TARGET ${GTK3_TARGET_LIST})
# 	# Render non-scale assets
# 	add_custom_command(
# 		OUTPUT "${GTK3_ASSETS_DIR}/${TARGET}.png"
# 		COMMAND ${INKSCAPE} "--export-id=${TARGET}" "--export-dpi=${NON_SCALE_DPI}" --export-id-only "${INK_EXPORT_OPTION}=${GTK3_ASSETS_DIR}/${TARGET}.png" ${SRC_FILE_GTK3}
# 		DEPENDS ${SRC_FILE_GTK3}
# 		WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
# 	)
# 
# 	add_custom_target(
# 		"gtk3-${TARGET}.png"
# 		ALL
# 		DEPENDS "${GTK3_ASSETS_DIR}/${TARGET}.png"
# 	)
# 
# 	# Render scaled assets (x2)
# 	add_custom_command(
# 		OUTPUT "${GTK3_ASSETS_DIR}/${TARGET}@2.png"
# 		COMMAND ${INKSCAPE} "--export-id=${TARGET}" "--export-dpi=${SCALE_DPI}" --export-id-only "${INK_EXPORT_OPTION}=${GTK3_ASSETS_DIR}/${TARGET}@2.png" ${SRC_FILE_GTK3}
# 		DEPENDS ${SRC_FILE_GTK3}
# 		WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
# 	)
# 
# 	add_custom_target(
# 		"gtk3-${TARGET}x2.png"
# 		ALL
# 		DEPENDS "${GTK3_ASSETS_DIR}/${TARGET}@2.png"
# 	)
# endforeach()
# 
# # Clone stock SVG files
# add_custom_command(
# 	OUTPUT "${GTK2_ASSETS_DIR}/Shadows/z-depth-1.svg" "${GTK2_ASSETS_DIR}/Shadows/z-depth-2.svg"
# 	COMMAND cp "${CMAKE_CURRENT_SOURCE_DIR}/assets-clone/z-depth-*" "${GTK2_ASSETS_DIR}/Shadows/"
# 	DEPENDS assets-clone/z-depth-1.svg assets-clone/z-depth-2.svg
# )
# add_custom_command(
# 	OUTPUT "${GTK2_ASSETS_DIR}/Others/null.svg"
# 	COMMAND cp "${CMAKE_CURRENT_SOURCE_DIR}/assets-clone/null.svg" "${GTK2_ASSETS_DIR}/Others/null.svg"
# 	DEPENDS assets-clone/null.svg
# )
# add_custom_target(
# 	"gtk2-copy-stock-svg"
# 	ALL
# 	DEPENDS "${GTK2_ASSETS_DIR}/Shadows/z-depth-1.svg" "${GTK2_ASSETS_DIR}/Shadows/z-depth-2.svg" "${GTK2_ASSETS_DIR}/Others/null.svg"
# )
# 
# foreach(TARGET ${GTK2_TARGET_LIST})
# 	cmake_path(GET TARGET PARENT_PATH TARGET_DIR)
# 	cmake_path(GET TARGET FILENAME IMAGE_NAME)
# 	string(REPLACE "/" "_" ID ${TARGET})
# 	file(MAKE_DIRECTORY "${GTK2_ASSETS_DIR}/${TARGET_DIR}")
# 
# 	# Render non-scale assets
# 	add_custom_command(
# 		OUTPUT "${GTK2_ASSETS_DIR}/${TARGET}.png"
# 		COMMAND ${INKSCAPE} "--export-id=${ID}" "--export-dpi=${NON_SCALE_DPI}" --export-id-only "${INK_EXPORT_OPTION}=${GTK2_ASSETS_DIR}/${TARGET}.png" ${SRC_FILE_GTK2}
# 		DEPENDS ${SRC_FILE_GTK2}
# 		WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
# 	)
# 
# 	add_custom_target(
# 		"gtk2-${TARGET_DIR}-${IMAGE_NAME}.png"
# 		ALL
# 		DEPENDS "${GTK2_ASSETS_DIR}/${TARGET}.png"
# 	)
# 
# 	foreach(PLATA_VARIANT ${PLATA_VARIANTS})
# 		install(
# 			# FILES "${CMAKE_CURRENT_SOURCE_DIR}/${GTK2_ASSETS_DIR}/${TARGET}.png"
# 			FILES "${GTK2_ASSETS_DIR}/${TARGET}.png"
# 			DESTINATION "${THEME_DIR}/${PLATA_VARIANT}/gtk-2.0/${TARGET_DIR}"
# 		)
# 	endforeach()
# endforeach()
