project(
  'plata-theme',
  version: '0.9.8',
  license: 'GPLv2+',
  meson_version: '>= 0.58.0',
)

plata_version = meson.project_version()
ver_arr = plata_version.split('.')
plata_maj_ver = ver_arr[0]
plata_mnr_ver = ver_arr[1]
plata_mcr_ver = ver_arr[2]

# Variables

prefix = get_option('prefix')
datarootdir = join_paths(prefix, get_option('datadir'))
themedir = join_paths(get_option('datadir'), 'themes')
platadir = join_paths(prefix, themedir, 'Plata')
luminedir = join_paths(prefix, themedir, 'Plata-Lumine')
noirdir = join_paths(prefix, themedir, 'Plata-Noir')
platacompactdir = join_paths(prefix, themedir, 'Plata-Compact')
luminecompactdir = join_paths(prefix, themedir, 'Plata-Lumine-Compact')
noircompactdir = join_paths(prefix, themedir, 'Plata-Noir-Compact')

# Dependencies

gdk_pixbuf = dependency('gdk-pixbuf-2.0', version: '>=2.32.2')
glib = dependency('glib-2.0', version: '>=2.48.0')
glib_compile_resources = find_program('glib-compile-resources')
xmllint = find_program('xmllint')
inkscape = find_program('inkscape')

zip = find_program('zip', required: get_option('telegram'))
libmarco = dependency('libmarco-private', version: '>=1.22.2', required: get_option('mate'))

# (Dart) Sass

sassc = find_program('sass', required: false)
sassc_options = ['--no-source-map']

if sassc.found()
  sass_full_version = run_command(sassc, '--version').stdout()
  sass_is_ruby_sass = sass_full_version.contains('Ruby Sass')
  sass_has_module_system = sass_full_version.version_compare('>= 1.23.0')
endif

if sass_is_ruby_sass
  sassc = find_program('dart-sass', required: false)
  sass_full_version = run_command(sassc, '--version').stdout()
  sass_has_module_system = sass_full_version.version_compare('>= 1.23.0')
endif

if not sassc.found() or sass_is_ruby_sass or not sass_has_module_system
  message('dart-sass >= 1.23.0 not found, installing it locally via npm')
  npm = find_program('npm')
  run_command(npm, 'install')
  sassc = find_program('./node_modules/.bin/sass')
endif

# Set colors

# Default colors
color_file = './src/gtk/sass/common/_colors.scss'
selection_default = run_command('grep', 'Indigo500', color_file, check: true).stdout().strip().split()[2]
accent_default = run_command('grep', 'Indigo300', color_file, check: true).stdout().strip().split()[2]
suggestion_default = run_command('grep', 'DPurple500', color_file, check: true).stdout().strip().split()[2]
destruction_default = run_command('grep', 'Red500', color_file, check: true).stdout().strip().split()[2]

# From m4/plata-gnome.m4

# check glib-2.0 version for determining GNOME env version
gio_version = glib.version()
gnome_scss_version = '3.24'
if gio_version.version_compare('>=2.65')
  # switch to gnome-shell 3.38 theming if GIO >= 2.65
  gnome_scss_version = '3.38'
elif gio_version.version_compare('>=2.63')
  # switch to gnome-shell 3.36 theming if GIO >= 2.63
  gnome_scss_version = '3.36'
elif gio_version.version_compare('>=2.61')
  # switch to gnome-shell 3.34 theming if GIO >= 2.61
  gnome_scss_version = '3.34'
elif gio_version.version_compare('>=2.59')
  # switch to gnome-shell 3.32 theming if GIO >= 2.59
  gnome_scss_version = '3.32'
elif gio_version.version_compare('>=2.53')
  # switch to gnome-shell 3.26 theming if GIO >= 2.53
  gnome_scss_version = '3.26'
endif

subdir('src/gtk/sass/common/resources')
subdir('data')

report = [
  f'     Plata Theme @plata_version@',
   '     =================================================================',
   '',
  f'      prefix:                 @prefix@',
  f'      datarootdir:            @datarootdir@',
  f'      target directories:     @platadir@',
  f'                              @platacompactdir@',
  f'                              @luminedir@',
  f'                              @luminecompactdir@',
  f'                              @noirdir@',
  f'                              @noircompactdir@',
   '',
   '      glib-compile-resources: @0@'.format(glib_compile_resources.full_path()),
   '      xmllint:                @0@'.format(xmllint.full_path()),
   '      inkscape:               @0@'.format(inkscape.full_path()),
   '      sassc:                  @0@'.format(sassc.full_path()),
   '',
  #'      parallel-build:         $ENABLE_PARALLEL (default: no)',
  #'      parallel:               $PARALLEL',
  #'',
   '      zip:                    @0@ (required if -Dtelegram=true)'.format(zip.full_path()),
   '      libmarco:               @0@ (required if -Dmate=true)'.format(libmarco.version()),
   '',
   '      selection-color:        @0@ (default: @1@)'.format(get_option('key_selection_color'), selection_default),
   '      accent-color:           @0@ (default: @1@)'.format(get_option('key_accent_color'), accent_default),
   '      suggestion-color:       @0@ (default: @1@)'.format(get_option('key_suggestion_color'), suggestion_default),
   '      destruction-color:      @0@ (default: @1@)'.format(get_option('key_destruction_color'), destruction_default),
   '',
   '',
   '     -----------------------------------------------------------------',
   '     | Supported Gtk Version',
   '     -----------------------------------------------------------------',
   '',
   '      Gtk 2.0:                always',
   '      Gtk 3.20:               always',
   '      Gtk 3.22:               always',
   '      Gtk 3.24:               always',
   '      Gtk 4.0:                @0@ (experimental)'.format(get_option('gtk_next')),
   '',
   '     -----------------------------------------------------------------',
   '     | DE and WM Support',
   '     -----------------------------------------------------------------',
   '',
   '      Budgie-Desktop:         always (>= 10.4)',
   '      GNOME:                  @0@ (default: true) (SCSS ver. @1@)'.format(get_option('gnome'), gnome_scss_version),
   '      Cinnamon:               @0@ (default: true)'.format(get_option('cinnamon')),
   '      GNOME-Flashback:        @0@ (default: true)'.format(get_option('flashback')),
   '      XFce:                   @0@ (default: true)'.format(get_option('xfce')),
   '      Mate:                   @0@ (default: true)'.format(get_option('mate')),
   '      Openbox:                @0@ (default: true)'.format(get_option('openbox')),
   '',
   '     -----------------------------------------------------------------',
   '     | Extra Support',
   '     -----------------------------------------------------------------',
   '',
   '      GtkSourceView/Gedit:    always',
   '      Plank:                  @0@ (default: false)'.format(get_option('plank')),
   '      Telegram:               @0@ (default: false)'.format(get_option('telegram')),
   '      Tweetdeck-Colordeck:    @0@ (default: false)'.format(get_option('tweetdeck')),
   '      Air-for-Steam:          @0@ (default: false)'.format(get_option('airforsteam')),
   '',
   '     =================================================================',
]

message('\n\n' + '\n'.join(report) + '\n')
