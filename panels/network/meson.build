cappletname = 'network'
cflags = []
foreach template: panel_cflag_templates
  cflags += template.format(cappletname)
endforeach

deps = common_deps + network_manager_deps + [
  polkit_gobject_dep,
  dependency('gmodule-2.0')
]

subdir('wireless-security')
subdir('connection-editor')

desktops = [
  'gnome-' + cappletname + '-panel.desktop',
  'gnome-wifi-panel.desktop'
]

foreach desktop: desktops
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
endforeach

sources = files(
  'cc-network-panel.c',
  'cc-wifi-panel.c',
  'net-device-ethernet.c',
  'net-device-mobile.c',
  'net-device-simple.c',
  'net-device-wifi.c',
  'net-device.c',
  'net-object.c',
  'net-proxy.c',
  'net-vpn.c',
  'network-dialogs.c',
  'panel-common.c'
)

resource_data = files(
  'network-ethernet.ui',
  'network-mobile.ui',
  'network-proxy.ui',
  'network-simple.ui',
  'network.ui',
  'network-vpn.ui',
  'network-wifi.ui',
  'wifi.ui'
)

sources += gnome.compile_resources(
  'cc-' + cappletname + '-resources',
  cappletname + '.gresource.xml',
  source_dir: '.',
  c_name: 'cc_' + cappletname,
  dependencies: resource_data,
  export: true
)

cflags += '-DGNOMELOCALEDIR="@0@"'.format(control_center_localedir)

libnetwork = static_library(
  cappletname,
  sources: sources,
  include_directories: top_inc,
  dependencies: deps,
  c_args: cflags,
  link_with: libconnection_editor
)
