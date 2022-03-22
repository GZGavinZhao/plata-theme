subdirs(common/resources)

set(COMMON_DIR common)
set(RESOURCE_DIR "${COMMON_DIR}/resources")
set(ANIMATIONS "${RESOURCE_DIR}/_animations.scss")
set(COLOR_PALETTE "${RESOURCE_DIR}/_color_palette.scss")
set(FUNCTIONS "${RESOURCE_DIR}/_functions.scss")
set(KEY_COLORS "${RESOURCE_DIR}/_key_colors.scss")
set(MATE_FLAG "${RESOURCE_DIR}/_mate_flag.scss")
set(VARIABLES "${RESOURCE_DIR}/_variables.scss")

foreach(GTK_VER ${GTK_VERSIONS})
	set(GTK_DIR "gtk-${GTK_VER}")

	if(GTK_VER VERSION_LESS 3.0)
		add_custom_command(
			OUTPUT "${GTK_VER}/gtk-light.css" "${GTK_VER}/gtk-dark.css"
			COMMAND ${SASSC} ${SASSC_OPTIONS} ${GTK_VER}
			DEPENDS "${GTK_VER}/gtk-light.scss" "${GTK_VER}/gtk-dark.scss" "${COMMON_DIR}/**"
		)

		add_custom_target(
			"${GTK_VER}-gtk-light-and-dark.scss"
			ALL
			DEPENDS "${GTK_VER}/gtk-light.css" "${GTK_VER}/gtk-dark.css"
		)
	else()
		foreach(SCSS_FILE ${PLATA_SCSS_FILES})
			string(REPLACE "scss" "css" CSS_FILE "${SCSS_FILE}")

			list(FIND PLATA_SCSS_FILES ${SCSS_FILE} IDX)
			list(GET PLATA_VARIANTS ${IDX} PLATA_VARIANT)

			add_custom_command(
				OUTPUT "${GTK_VER}/${CSS_FILE}"
				COMMAND "${SASSC}" ${SASSC_OPTIONS} "${GTK_VER}/${SCSS_FILE}:${GTK_VER}/${CSS_FILE}"
				DEPENDS "${GTK_VER}/${SCSS_FILE}" "${COMMON_DIR}/**"
			)

			add_custom_target(
				"${GTK_VER}-${SCSS_FILE}"
				ALL
				DEPENDS "${GTK_VER}/${CSS_FILE}"
			)

			install(
				FILES "${GTK_VER}/${CSS_FILE}"
				RENAME "gtk.css"
				DESTINATION "${THEME_DIR}/${PLATA_VARIANT}/${GTK_DIR}"
			)

			if(SCSS_FILE STREQUAL "gtk-dark.scss")
				install(
					FILES "${GTK_VER}/${CSS_FILE}"
					RENAME "gtk-dark.css"
					DESTINATION "${THEME_DIR}/Plata/${GTK_DIR}"
				)
				install(
					FILES "${GTK_VER}/${CSS_FILE}"
					RENAME "gtk-dark.css"
					DESTINATION "${THEME_DIR}/Plata-Lumine/${GTK_DIR}"
				)
			elseif(SCSS_FILE STREQUAL "gtk-dark-compact.scss")
				install(
					FILES "${GTK_VER}/${CSS_FILE}"
					RENAME "gtk-dark.css"
					DESTINATION "${THEME_DIR}/Plata-Compact/${GTK_DIR}"
				)
				install(
					FILES "${GTK_VER}/${CSS_FILE}"
					RENAME "gtk-dark.css"
					DESTINATION "${THEME_DIR}/Plata-Lumine-Compact/${GTK_DIR}"
				)
			endif()

		endforeach()
	endif()
endforeach()

