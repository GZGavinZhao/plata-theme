foreach(GTK_VER ${GTK_VERSIONS})
	set(GTK_DIR "gtk-${GTK_VER}")

	foreach(SCSS_FILE ${PLATA_SCSS_FILES})
		string(REPLACE "scss" "css" CSS_FILE "${SCSS_FILE}")

		list(FIND PLATA_SCSS_FILES ${SCSS_FILE} IDX)
		list(GET PLATA_VARIANTS ${IDX} PLATA_VARIANT)

		add_custom_command(
			OUTPUT "${GTK_VER}/${CSS_FILE}"
			COMMAND "sass" "--no-source-map" "${GTK_VER}/${SCSS_FILE}:${GTK_VER}/${CSS_FILE}"
			DEPENDS "${GTK_VER}/${SCSS_FILE}"
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
	endforeach()
endforeach()

