cappletname = 'mouse'
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
  'cc-mouse-caps-helper.c',
  'cc-mouse-panel.c',
  'gnome-mouse-properties.c',
  'gnome-mouse-test.c'
)

resource_data = files(
  'gnome-mouse-properties.ui',
  'gnome-mouse-test.ui',
  'scroll-test-gegl.svg',
  'scroll-test.svg'
)

resources = gnome.compile_resources(
  'cc-' + cappletname + '-resources',
  cappletname + '.gresource.xml',
  source_dir: '.',
  c_name: 'cc_' + cappletname,
  dependencies: resource_data,
  export: true
)

deps = common_deps + [
  gnome_settings_dep,
  x11_dep,
  xi_dep
]

cflags += '-DGNOMELOCALEDIR="@0@"'.format(control_center_localedir)

libmouse_properties = static_library(
  cappletname + '-properties',
  sources: sources + resources,
  include_directories: [top_inc, common_inc],
  dependencies: deps,
  c_args: cflags,
  link_with: libdevice
)

test_name = 'gnome-mouse-test'

test_sources = files(
  test_name + '.c',
  'test-' + test_name + '.c'
)

executable(
  'test-' + test_name,
  test_sources + resources,
  include_directories: top_inc,
  dependencies: deps,
  c_args: cflags
)
