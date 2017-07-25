cappletname = 'search'
cflags = []
foreach template: panel_cflag_templates
  cflags += template.format(cappletname)
endforeach

desktop = 'gnome-' + cappletname + '-panel.desktop'

desktop_in = configure_file(
  input: desktop + '.in.in',
  output: desktop + '.in',
  configuration: desktop_conf
)

custom_target(
  desktop,
  input: desktop_in,
  output: desktop,
  command: intltool_desktop_cmd,
  install: true,
  install_dir: join_paths(control_center_datadir, 'applications')
)

sources = files(
  'cc-search-locations-dialog.c',
  'cc-search-panel.c'
)

resource_data = files(
  'search-locations-dialog.ui',
  'search.ui'
)

sources += gnome.compile_resources(
  'cc-' + cappletname + '-resources',
  cappletname + '.gresource.xml',
  source_dir: '.',
  c_name: 'cc_' + cappletname,
  dependencies: resource_data,
  export: true
)

cflags += [
  '-DDATADIR="@0@"'.format(control_center_datadir),
  '-DGNOMELOCALEDIR="@0@"'.format(control_center_localedir)
]

libsearch = static_library(
  cappletname,
  sources: sources,
  include_directories: top_inc,
  dependencies: common_deps,
  c_args: cflags
)
