# Set color

key_colors = configuration_data()
key_colors.set('key_selection_color', get_option('key_selection_color'))
key_colors.set('key_accent_color', get_option('key_accent_color'))
key_colors.set('key_suggestion_color', get_option('key_suggestion_color'))
key_colors.set('key_destruction_color', get_option('key_destruction_color'))

configure_file(
  input: '_key_colors.scss.in',
  output: '_key_colors.scss',
  configuration: key_colors,
)

# Enable Mate support

# From m4/plata-mate.m4
if get_option('mate')
  mate_support = libmarco.version().version_compare('>=1.22.2')
  mate_support_data = configuration_data()
  mate_support_data.set('mate_support', mate_support.to_string())
  configure_file(
    input: '_mate_flag.scss.in',
    output: '_mate_flag.scss',
    configuration: mate_support_data,
  )
endif
