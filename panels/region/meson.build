cappletname = 'region'
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
  'cc-format-chooser.c',
  'cc-ibus-utils.c',
  'cc-input-options.c',
  'cc-input-chooser.c',
  'cc-region-panel.c'
)

resource_data = files(
  'format-chooser.ui',
  'input-options.ui',
  'input-chooser.ui',
  'region.ui'
)

sources += gnome.compile_resources(
  'cc-' + cappletname + '-resources',
  cappletname + '.gresource.xml',
  source_dir: '.',
  c_name: 'cc_' + cappletname,
  dependencies: resource_data,
  export: true
)

deps = common_deps + [
  accounts_dep,
  gnome_desktop_dep,
  libgd_dep,
  polkit_gobject_dep
]

if enable_ibus
  deps += dependency('ibus-1.0', version: '>= 1.5.2')
endif

libregion = static_library(
  cappletname,
  sources: sources,
  include_directories: [top_inc, common_inc],
  dependencies: deps,
  c_args: cflags,
  link_with: liblanguage
)
