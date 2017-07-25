cappletname = 'info'
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
  'cc-info-default-apps-panel.c',
  'cc-info-overview-panel.c',
  'cc-info-panel.c',
  'cc-info-removable-media-panel.c',
  'gsd-disk-space-helper.c',
  'info-cleanup.c'
)

resource_data = files(
  'GnomeLogoVerticalMedium.svg',
  'info-default-apps.ui',
  'info-overview.ui',
  'info-removable-media.ui',
  'info.ui'
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
  polkit_gobject_dep,
  dependency('libgtop-2.0')
]

cflags += [
  '-DBINDIR="@0@"'.format(control_center_bindir),
  '-DDATADIR="@0@"'.format(control_center_datadir),
  '-DGNOME_SESSION_DIR="@0@"'.format(gnome_session_libexecdir),
  '-DGNOMELOCALEDIR="@0@"'.format(control_center_localedir)
]

libinfo = static_library(
  cappletname,
  sources: sources,
  include_directories: top_inc,
  dependencies: deps,
  c_args: cflags
)

test_name = 'info-cleanup'

test_sources = files(
  test_name + '.c',
  'test-' + test_name + '.c'
)

test_cflags = cflags + [
  '-DTEST_SRCDIR="@0@"'.format(meson.current_source_dir())
]

exe = executable(
  'test-' + test_name,
  test_sources,
  include_directories: top_inc,
  dependencies: deps,
  c_args: test_cflags
)

test(test_name, exe)

# FIXME: updates gnome-settings-daemon code !?
'''
space_files = files(
  'gsd-disk-space-helper.c',
  'gsd-disk-space-helper.h'
)

install_data(
  space_files,
  install_dir: join_paths(meson.source_root(), '..', 'gnome-settings-daemon', 'plugins', 'housekeeping')
)

update-from-gsd:
  FILES="$(SPACEFILES)" DIR="$(SPACEDIR)" $(top_srcdir)/update-from-gsd.sh && changed=true ; \
  git commit -m "info: Update from gnome-settings-daemon" $(SPACEFILES)
'''
