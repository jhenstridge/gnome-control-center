cappletname = 'wacom'
cflags = []
foreach template: panel_cflag_templates
  cflags += template.format(cappletname)
endforeach

deps = common_deps + [
  clutter_dep,
  clutter_gtk_dep,
  gnome_desktop_dep,
  gnome_settings_dep,
  libwacom_dep,
  x11_dep,
  xi_dep
]

cflags += '-DGNOMELOCALEDIR="@0@"'.format(control_center_localedir)

test_cflags = cflags + ['-DFAKE_AREA']

subdir('calibrator')

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

common_sources = files(
  'cc-tablet-tool-map.c',
  'cc-wacom-button-row.c',
  'cc-wacom-device.c',
  'cc-wacom-mapping-panel.c',
  'cc-wacom-nav-button.c',
  'cc-wacom-page.c',
  'cc-wacom-stylus-page.c',
  'cc-wacom-tool.c',
  'gsd-wacom-key-shortcut-button.c'
)

resource_data = files(
  'button-mapping.ui',
  'gnome-wacom-properties.ui',
  'wacom-stylus-airbrush.svg',
  'wacom-stylus-art-pen.svg',
  'wacom-stylus-classic.svg',
  'wacom-stylus-inking.svg',
  'wacom-stylus-no-eraser.svg',
  'wacom-stylus-page.ui',
  'wacom-stylus.svg',
  'wacom-tablet-cintiq.svg',
  'wacom-tablet-pc.svg',
  'wacom-tablet.svg'
)

common_sources += gnome.compile_resources(
  'cc-' + cappletname + '-resources',
  cappletname + '.gresource.xml',
  source_dir: '.',
  c_name: 'cc_' + cappletname,
  dependencies: resource_data,
  export: true
)

sources = common_sources + files(
  'cc-drawing-area.c',
  'cc-wacom-panel.c'
)

incs = [
  top_inc,
  common_inc,
  calibrator_inc
]

libwacom_properties = static_library(
  cappletname + '-properties',
  sources: sources,
  include_directories: incs,
  dependencies: deps,
  c_args: cflags,
  link_with: [
    libdevice,
    libwacom_calibrator
  ]
)

test_name = 'test-wacom'

executable(
  test_name,
  [test_name + '.c'] + common_sources,
  include_directories: incs,
  dependencies: deps,
  c_args: test_cflags,
  link_with: [
    libdevice,
    libwacom_calibrator_test
  ]
)
