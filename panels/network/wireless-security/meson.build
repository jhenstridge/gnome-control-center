name = 'wireless-security'

wireless_security_inc = include_directories('.')

nm_applet_sources = files(
  'eap-method-fast.c',
  'eap-method-leap.c',
  'eap-method-peap.c',
  'eap-method-simple.c',
  'eap-method-tls.c',
  'eap-method-ttls.c',
  'eap-method.c',
  'helpers.c',
  'wireless-security.c',
  'ws-leap.c',
  'ws-dynamic-wep.c',
  'ws-wep-key.c',
  'ws-wpa-eap.c',
  'ws-wpa-psk.c'
)

sources = nm_applet_sources + files('utils.c')

resource_data = files(
  'eap-method-fast.ui',
  'eap-method-leap.ui',
  'eap-method-peap.ui',
  'eap-method-simple.ui',
  'eap-method-tls.ui',
  'eap-method-ttls.ui',
  'ws-dynamic-wep.ui',
  'ws-leap.ui',
  'ws-wep-key.ui',
  'ws-wpa-eap.ui',
  'ws-wpa-psk.ui'
)

sources += gnome.compile_resources(
  name + '-resources',
  name + '.gresource.xml',
  source_dir: '.',
  c_name: name.underscorify(),
  dependencies: resource_data,
  export: true
)

# FIXME: is this uidir from ./panels/sharing/Makefile.am??
cflags = '-DUIDIR="@0@"'.format(control_center_pkgdatadir, 'ui', 'sharing')

libwireless_security = static_library(
  name,
  sources: sources,
  include_directories: top_inc,
  dependencies: deps,
  c_args: cflags
)

# FIXME: updates gnome-settings-daemon code !?
'''
wireless_security_dir = join_paths(meson.source_root(), '..', 'network-manager-applet', 'src', 'wireless-security')

update-from-nma:
  FILES="$(nm_applet_sources)" DIR="$(wireless_security_dir)" $(top_srcdir)/update-from-gsd.sh && \
        patch -p4 < $(srcdir)/nm-connection-editor-to-network-panel.patch && \
        git add $(nm_applet_sources) && \
        git commit -m "network: Update wireless-security from network-manager-applet"
  FILES="$(resource_data)" DIR="$(wireless_security_dir)" $(top_srcdir)/update-from-gsd.sh && \
        patch -p4 < $(srcdir)/nm-connection-editor-ui-to-network-panel.patch && \
        git add $(resource_data) && \
        git commit -m "network: Update wireless-security UI from network-manager-applet"
'''
