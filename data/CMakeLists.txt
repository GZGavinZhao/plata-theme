set(INDEX_NAMES index.theme index.theme-compact index.theme-light index.theme-light-compact index.theme-dark index.theme-dark-compact)
set(COMMENTS "" " with compact elements" " with light variant" " with light variant and compact elements" " with dark variant" " with dark variant and compact elements")

foreach(PLATA_VARIANT ${PLATA_VARIANTS})
	list(FIND PLATA_VARIANTS ${PLATA_VARIANT} IDX)
	list(GET INDEX_NAMES ${IDX} OUTPUT_FILE)
	list(GET COMMENTS ${IDX} COMMENT)

	configure_file(index.theme.in ${OUTPUT_FILE})

	install(
		FILES ${OUTPUT_FILE}
		DESTINATION "${THEME_DIR}/${PLATA_VARIANT}"
	)
endforeach()
