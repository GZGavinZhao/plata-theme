subdirs(common/resources)

set(COMMON_DIR common)
set(THIRD_PARTY "${COMMON_DIR}/_3rd-party.scss")
set(COLORS_PUBLIC "${COMMON_DIR}/_colors-public.scss")
set(COLORS "${COMMON_DIR}/_colors.scss")
set(COMMON "${COMMON_DIR}/_common.scss")
set(DRAWING "${COMMON_DIR}/_drawing.scss")
set(KEY_COLORS "${COMMON_DIR}/_key_colors.scss")
set(MISC "${COMMON_DIR}/_misc.scss")
set(XFCE_NOTIFY "${COMMON_DIR}/_xfce-notify.scss")

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
		# add_custom_command(
		# 	OUTPUT "${GTK_VER}/gtk-light.css" "${GTK_VER}/gtk-dark.css"
		# 	COMMAND ${SASSC} ${SASSC_OPTIONS} ${GTK_VER}
		# 	DEPENDS "${GTK_VER}/gtk-light.scss" "${GTK_VER}/gtk-dark.scss" "${GTK_VER}/_gtk2-colors.scss" ${COLOR_PALETTE} ${FUNCTIONS} ${VARIABLES} ${COLORS} ${KEY_COLORS}
		# 	WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
		# )

		# add_custom_target(
		# 	"${GTK_VER}-gtk-light-and-dark.scss"
		# 	ALL
		# 	DEPENDS "${GTK_VER}/gtk-light.css" "${GTK_VER}/gtk-dark.css"
		# )
	else()
		set(OUT_VER ${GTK_VER})
		# Yes, unlike others, 3.20 is mapped to 3.0
		if(GTK_VER STREQUAL "3.20")
			set(OUT_VER 3.0)
		endif()

		set(FULL_CSS_FILES)
		foreach(SCSS_FILE ${PLATA_SCSS_FILES})
			list(FIND PLATA_SCSS_FILES ${SCSS_FILE} IDX)
			list(GET PLATA_CSS_FILES ${IDX} CSS_FILE)

			list(APPEND FULL_CSS_FILES "${CMAKE_CURRENT_SOURCE_DIR}/${GTK_VER}/${CSS_FILE}")
		endforeach()

		set(VARIANTS_BASE_NAME "" "-compact" "-light" "-light-compact" "-dark" "-dark-compact")
		foreach(PLATA_VARIANT ${PLATA_VARIANTS})
			list(FIND PLATA_VARIANTS ${PLATA_VARIANT} IDX)
			list(GET VARIANTS_BASE_NAME ${IDX} VARIANT)

			if(PLATA_VARIANT MATCHES "Noir")
				configure_file(
					"${CMAKE_CURRENT_SOURCE_DIR}/gtk-dark.gresource.xml.in"
					"${CMAKE_BINARY_DIR}/${PLATA_VARIANT}/gtk-${OUT_VER}/gtk.gresource.xml"
				)
			else()
				configure_file(
					"${CMAKE_CURRENT_SOURCE_DIR}/gtk.gresource.xml.in"
					"${CMAKE_BINARY_DIR}/${PLATA_VARIANT}/gtk-${OUT_VER}/gtk.gresource.xml"
				)
			endif()
		endforeach()

		foreach(SCSS_FILE ${PLATA_SCSS_FILES})
			list(FIND PLATA_SCSS_FILES ${SCSS_FILE} IDX)
			list(GET PLATA_VARIANTS ${IDX} PLATA_VARIANT)
			list(GET PLATA_CSS_FILES ${IDX} CSS_FILE)
			list(GET FULL_CSS_FILES ${IDX} FULL_CSS_FILE)

			if(PLATA_VARIANT MATCHES "Noir")
				file(CREATE_LINK
					${FULL_CSS_FILE}
					"${CMAKE_BINARY_DIR}/${PLATA_VARIANT}/gtk-${OUT_VER}/gtk-contained-dark.css"
					SYMBOLIC
				)
				configure_file(
					"${CMAKE_CURRENT_SOURCE_DIR}/gtk-dark.css.in"
					"${CMAKE_BINARY_DIR}/${PLATA_VARIANT}/gtk-${OUT_VER}/gtk-dark.css"
				)

				if(PLATA_VARIANT MATCHES "Compact")
					foreach(LIGHT_VARIANT Plata-Compact Plata-Lumine-Compact)
						file(CREATE_LINK
							${FULL_CSS_FILE}
							"${CMAKE_BINARY_DIR}/${LIGHT_VARIANT}/gtk-${OUT_VER}/gtk-contained-dark.css"
							SYMBOLIC
						)
						configure_file(
							"${CMAKE_CURRENT_SOURCE_DIR}/gtk-dark.css.in"
							"${CMAKE_BINARY_DIR}/${LIGHT_VARIANT}/gtk-${OUT_VER}/gtk-dark.css"
						)
						install(
							FILES "${CMAKE_BINARY_DIR}/${LIGHT_VARIANT}/gtk-${OUT_VER}/gtk-dark.css"
							DESTINATION "${THEME_DIR}/${LIGHT_VARIANT}/gtk-${OUT_VER}"
						)
					endforeach()
				else()
					foreach(LIGHT_VARIANT Plata Plata-Lumine)
						file(CREATE_LINK
							${FULL_CSS_FILE}
							"${CMAKE_BINARY_DIR}/${LIGHT_VARIANT}/gtk-${OUT_VER}/gtk-contained-dark.css"
							SYMBOLIC
						)
						configure_file(
							"${CMAKE_CURRENT_SOURCE_DIR}/gtk-dark.css.in"
							"${CMAKE_BINARY_DIR}/${LIGHT_VARIANT}/gtk-${OUT_VER}/gtk-dark.css"
						)
						install(
							FILES "${CMAKE_BINARY_DIR}/${LIGHT_VARIANT}/gtk-${OUT_VER}/gtk-dark.css"
							DESTINATION "${THEME_DIR}/${LIGHT_VARIANT}/gtk-${OUT_VER}"
						)
					endforeach()
				endif()
			else()
				file(CREATE_LINK
					"${CMAKE_CURRENT_SOURCE_DIR}/${GTK_VER}/${CSS_FILE}"
					"${CMAKE_BINARY_DIR}/${PLATA_VARIANT}/gtk-${OUT_VER}/gtk-contained.css"
					SYMBOLIC
				)
				
				configure_file(
					"${CMAKE_CURRENT_SOURCE_DIR}/gtk.css.in"
					"${CMAKE_BINARY_DIR}/${PLATA_VARIANT}/gtk-${OUT_VER}/gtk.css"
				)

				install(
					FILES "${CMAKE_BINARY_DIR}/${PLATA_VARIANT}/gtk-${OUT_VER}/gtk.css"
					DESTINATION "${THEME_DIR}/${PLATA_VARIANT}/gtk-${OUT_VER}"
				)
			endif()

			# Compile CSS and assets together into a bundle to some extent.
			add_custom_command(
				OUTPUT "${CMAKE_BINARY_DIR}/${PLATA_VARIANT}/gtk-${OUT_VER}/gtk.gresource"
				COMMAND ${GLIB_COMPILE_RESOURCES} gtk.gresource.xml
				# DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/${GTK_VER}/${CSS_FILE}"
				DEPENDS ${FULL_CSS_FILES}
				WORKING_DIRECTORY "${CMAKE_BINARY_DIR}/${PLATA_VARIANT}/gtk-${OUT_VER}"
			)

			add_custom_target(
				"compile-${PLATA_VARIANT}-${GTK_VER}-gresource"
				ALL
				DEPENDS "${CMAKE_BINARY_DIR}/${PLATA_VARIANT}/gtk-${OUT_VER}/gtk.gresource"
			)

			install(
				FILES "${CMAKE_BINARY_DIR}/${PLATA_VARIANT}/gtk-${OUT_VER}/gtk.gresource"
				DESTINATION "${THEME_DIR}/${PLATA_VARIANT}/gtk-${OUT_VER}"
			)

			add_custom_command(
				OUTPUT "${CMAKE_CURRENT_SOURCE_DIR}/${GTK_VER}/${CSS_FILE}"
				COMMAND ${SASSC} ${SASSC_OPTIONS} "${GTK_VER}/${SCSS_FILE}:${GTK_VER}/${CSS_FILE}"
				DEPENDS "${GTK_VER}/${SCSS_FILE}" ${COLOR_PALETTE} ${FUNCTIONS} ${VARIABLES} ${COLORS} ${DRAWING} ${COMMON} ${MISC} ${THIRD_PARTY} ${COLORS_PUBLIC}
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
