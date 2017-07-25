cappletname = 'user-accounts'
cflags = []
foreach template: panel_cflag_templates
  cflags += template.format(cappletname)
endforeach

desktop = 'gnome-' + cappletname + '-panel.desktop'

desktop_in = configure_file(
  input: 'data/' + desktop + '.in.in',
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

image_data = files(
  'data/faces/astronaut.jpg',
  'data/faces/baseball.png',
  'data/faces/butterfly.png',
  'data/faces/cat-eye.jpg',
  'data/faces/chess.jpg',
  'data/faces/coffee.jpg',
  'data/faces/dice.jpg',
  'data/faces/energy-arc.jpg',
  'data/faces/fish.jpg',
  'data/faces/flake.jpg',
  'data/faces/flower.jpg',
  'data/faces/grapes.jpg',
  'data/faces/guitar.jpg',
  'data/faces/launch.jpg',
  'data/faces/leaf.jpg',
  'data/faces/lightning.jpg',
  'data/faces/penguin.jpg',
  'data/faces/puppy.jpg',
  'data/faces/sky.jpg',
  'data/faces/soccerball.png',
  'data/faces/sunflower.jpg',
  'data/faces/sunset.jpg',
  'data/faces/tennis-ball.png',
  'data/faces/yellow-rose.jpg'
)

install_data(
  image_data,
  install_dir: join_paths(control_center_datadir, 'pixmaps', 'faces')
)

polkit = 'org.gnome.controlcenter.' + cappletname + '.policy'

custom_target(
  polkit,
  input: polkit + '.in',
  output: polkit,
  command: intltool_xml_cmd,
  install: true,
  install_dir: join_paths(control_center_datadir, 'polkit-1', 'actions')
)

common_sources = files(
  'pw-utils.c',
  'um-account-dialog.c',
  'um-realm-manager.c',
  'um-utils.c'
)

resource_data = files(
  'data/icons/print_error.png',
  'data/icons/print_ok.png',
  'data/icons/left-index-finger.png',
  'data/icons/left-little-finger.png',
  'data/icons/left-middle-finger.png',
  'data/icons/left-ring-finger.png',
  'data/icons/left-thumb.png',
  'data/icons/right-index-finger.png',
  'data/icons/right-little-finger.png',
  'data/icons/right-middle-finger.png',
  'data/icons/right-ring-finger.png',
  'data/icons/right-thumb.png',
  'data/account-dialog.ui',
  'data/account-fingerprint.ui',
  'data/carousel.css',
  'data/carousel.ui',
  'data/history-dialog.ui',
  'data/join-dialog.ui',
  'data/password-dialog.ui',
  'data/user-accounts-dialog.css',
  'data/user-accounts-dialog.ui'
)

common_sources += gnome.compile_resources(
  'um-resources',
  cappletname + '.gresource.xml',
  source_dir: '.',
  c_name: 'um',
  dependencies: resource_data,
  export: true
)

realmd_namespace = 'org.freedesktop.realmd'

realmd_gen = 'um-realm-generated'

# FIXME: annotate option is missing
'''
um-realm-generated.c: $(srcdir)/data/org.freedesktop.realmd.xml
  $(AM_V_GEN) gdbus-codegen --interface-prefix org.freedesktop.realmd. \
    --generate-c-code um-realm-generated --c-namespace UmRealm \
    --annotate "org.freedesktop.realmd.Realm" org.gtk.GDBus.C.Name Common \
    --c-generate-object-manager $<
um-realm-generated.h: um-realm-generated.c
'''

'''
common_sources += custom_target(
  realmd_gen,
  input: 'data/' + realmd_namespace + '.xml',
  output: realmd_gen,
  command: [
    find_program('gdbus-codegen'),
    '--interface-prefix', realmd_namespace + '.',
    '--generate-c-code', '@OUTPUT@',
    '--c-namespace', 'UmRealm',
    '--annotate', '"org.freedesktop.realmd.Realm", 'org.gtk.GDBus.C.Name', 'Common',
    '--c-generate-object-manager', '@INPUT@'
  ]
)
'''

'''
common_sources += gnome.gdbus_codegen(
  'um-realm-generated',
  'data/' + realmd_namespace + '.xml',
  interface_prefix: realmd_namespace + '.',
  namespace: 'UmRealm',
  object_manager: true,
  annotate: '"org.freedesktop.realmd.Realm" org.gtk.GDBus.C.Name Common'
)
'''

common_sources += files(
  realmd_gen + '.c',
  realmd_gen + '.h'
)

sources = common_sources + files(
  'cc-crop-area.c',
  'run-passwd.c',
  'um-account-type.c',
  'um-carousel.c',
  'um-cell-renderer-user-image.c',
  'um-fingerprint-dialog.c',
  'um-history-dialog.c',
  'um-password-dialog.c',
  'um-photo-dialog.c',
  'um-user-image.c',
  'um-user-panel.c'
)

# Kerberos kerberos support
krb_dep = dependency('krb5', required: false)
assert(krb_dep.found(), 'kerberos libraries not found in your path')

deps = common_deps + [
  accounts_dep,
  gdk_pixbuf_dep,
  gnome_desktop_dep,
  libgd_dep,
  krb_dep,
  m_dep,
  polkit_gobject_dep,
  dependency('pwquality', version: '>= 1.2.2')
]

if have_cheese
  deps += cheese_deps
endif

cflags += [
  '-DGNOMELOCALEDIR="@0@"'.format(control_center_localedir),
  '-DHAVE_LIBPWQUALITY',
  '-DUM_PIXMAP_DIR="@0@"'.format(join_paths(control_center_pkgdatadir, 'pixmaps'))
]

libuser_accounts = static_library(
  cappletname,
  sources: sources,
  include_directories: [top_inc, common_inc, shell_inc],
  dependencies: deps,
  c_args: cflags,
  link_with: liblanguage
)

name = 'frob-account-dialog'

executable(
  name,
  [name + '.c'] + common_sources,
  include_directories: top_inc,
  dependencies: deps,
  c_args: cflags
)
