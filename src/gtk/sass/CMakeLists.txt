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
			WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
		)

		add_custom_target(
			"${GTK_VER}-gtk-light-and-dark.scss"
			ALL
			DEPENDS "${GTK_VER}/gtk-light.css" "${GTK_VER}/gtk-dark.css"
		)
	else()
		set(OUT_VER ${GTK_VER})
		# Yes, unlike others, 3.20 is mapped to 3.0
		if(GTK_VER STREQUAL "3.20")
			set(OUT_VER 3.0)
		endif()

		set(VARIANTS_BASE_NAME "" "-compact" "-light" "-light-compact" "-dark" "-dark-compact")
		foreach(PLATA_VARIANT ${PLATA_VARIANTS})
			list(FIND PLATA_VARIANTS ${PLATA_VARIANT} IDX)
			list(GET VARIANTS_BASE_NAME ${IDX} VARIANT)

			if(PLATA_VARIANT MATCHES "Noir")
				configure_file(
					"${CMAKE_CURRENT_SOURCE_DIR}/gtk-dark.gresource.xml.in"
					"${CMAKE_BINARY_DIR}/${PLATA_VARIANT}/gtk-${OUT_VER}/gtk-dark.gresource.xml"
				)
			else()
				configure_file(
					"${CMAKE_CURRENT_SOURCE_DIR}/gtk.gresource.xml.in"
					"${CMAKE_BINARY_DIR}/${PLATA_VAR}/gtk-${OUT_VER}/gtk.gresource.xml"
				)
			endif()
		endforeach()

		foreach(SCSS_FILE ${PLATA_SCSS_FILES})
			list(FIND PLATA_SCSS_FILES ${SCSS_FILE} IDX)
			list(GET PLATA_VARIANTS ${IDX} PLATA_VARIANT)
			list(GET PLATA_CSS_FILES ${IDX} CSS_FILE)

			set(FULL_CSS_FILE "${CMAKE_CURRENT_SOURCE_DIR}/${GTK_VER}/${CSS_FILE}")
			if(PLATA_VARIANT MATCHES "Noir")
				file(CREATE_LINK
					${FULL_CSS_FILE}
					"${CMAKE_BINARY_DIR}/${PLATA_VARIANT}/gtk-${OUT_VER}/gtk-contained-dark.css"
					SYMBOLIC
				)
				if(PLATA_VARIANT MATCHES "Compact")
					foreach(LIGHT_VARIANT Plata-Compact Plata-Lumine-Compact)
						file(CREATE_LINK
							${FULL_CSS_FILE}
							"${CMAKE_BINARY_DIR}/Plata-Compact/gtk-${OUT_VER}/gtk-contained-dark.css"
							SYMBOLIC
						)
					endforeach()
				else()
					foreach(LIGHT_VARIANT PlataPlata-Lumine)
						file(CREATE_LINK
							${FULL_CSS_FILE}
							"${CMAKE_BINARY_DIR}/Plata-Compact/gtk-${OUT_VER}/gtk-contained-dark.css"
							SYMBOLIC
						)
					endforeach()
				endif()
			else()
				file(CREATE_LINK
					"${CMAKE_CURRENT_SOURCE_DIR}/${GTK_VER}/${CSS_FILE}"
					"${CMAKE_BINARY_DIR}/${PLATA_VARIANT}/gtk-${OUT_VER}/gtk-contained.css"
					SYMBOLIC
				)
			endif()

			add_custom_command(
				OUTPUT "${CMAKE_CURRENT_SOURCE_DIR}/${GTK_VER}/${CSS_FILE}"
				COMMAND "${SASSC}" ${SASSC_OPTIONS} "${GTK_VER}/${SCSS_FILE}:${GTK_VER}/${CSS_FILE}"
				DEPENDS "${GTK_VER}/${SCSS_FILE}" "${COMMON_DIR}/**"
				WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
			)

			add_custom_target(
				"${GTK_VER}-${SCSS_FILE}"
				ALL
				DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/${GTK_VER}/${CSS_FILE}"
			)

			# install(
			# 	FILES "${GTK_VER}/${CSS_FILE}"
			# 	RENAME "gtk.css"
			# 	DESTINATION "${THEME_DIR}/${PLATA_VARIANT}/${GTK_DIR}"
			# )

			# if(SCSS_FILE STREQUAL "gtk-dark.scss")
			# 	install(
			# 		FILES "${GTK_VER}/${CSS_FILE}"
			# 		RENAME "gtk-dark.css"
			# 		DESTINATION "${THEME_DIR}/Plata/${GTK_DIR}"
			# 	)
			# 	install(
			# 		FILES "${GTK_VER}/${CSS_FILE}"
			# 		RENAME "gtk-dark.css"
			# 		DESTINATION "${THEME_DIR}/Plata-Lumine/${GTK_DIR}"
			# 	)
			# elseif(SCSS_FILE STREQUAL "gtk-dark-compact.scss")
			# 	install(
			# 		FILES "${GTK_VER}/${CSS_FILE}"
			# 		RENAME "gtk-dark.css"
			# 		DESTINATION "${THEME_DIR}/Plata-Compact/${GTK_DIR}"
			# 	)
			# 	install(
			# 		FILES "${GTK_VER}/${CSS_FILE}"
			# 		RENAME "gtk-dark.css"
			# 		DESTINATION "${THEME_DIR}/Plata-Lumine-Compact/${GTK_DIR}"
			# 	)
			# endif()

		endforeach()
	endif()
endforeach()
