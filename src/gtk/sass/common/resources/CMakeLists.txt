configure_file(
	"${CMAKE_CURRENT_SOURCE_DIR}/_key_colors.scss.in"
	"${CMAKE_CURRENT_SOURCE_DIR}/_key_colors.scss"
)

# Maybe there's a more elegant solution, but since it's such a short line, I'm
# happy with it :)
if(MATE)
	set(MATE_SUPPORT "true")
	configure_file(
		"${CMAKE_CURRENT_SOURCE_DIR}/_mate_flag.scss.in"
		"${CMAKE_CURRENT_SOURCE_DIR}/_mate_flag.scss"
	)
else()
	set(MATE_SUPPORT "false")
	configure_file(
		"${CMAKE_CURRENT_SOURCE_DIR}/_mate_flag.scss.in"
		"${CMAKE_CURRENT_SOURCE_DIR}/_mate_flag.scss"
	)
endif()
