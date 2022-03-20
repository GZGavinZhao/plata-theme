configure_file(_key_colors.scss.in _key_colors.scss)

# Maybe there's a more elegant solution, but since it's such a short line, I'm
# happy with it :)
if(MATE)
	set(MATE_SUPPORT "true")
	configure_file(_mate_flag.scss.in _mate_flag.scss)
else()
	set(MATE_SUPPORT "false")
	configure_file(_mate_flag.scss.in _mate_flag.scss)
endif()
