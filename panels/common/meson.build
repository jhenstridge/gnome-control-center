cappletname = 'common'
cflags = []
foreach template: panel_cflag_templates
  cflags += template.format(cappletname)
endforeach

common_inc = include_directories('.')

enums = 'gsd-common-enums'
enums_header = files('gsd-device-manager.h')

gsd_common_enums = []
gsd_common_enums += gnome.mkenums(
  enums + '.h',
  sources: enums_header,
  fhead: '#ifndef GSD_COMMON_ENUMS_H\n#define GSD_COMMON_ENUMS_H\n\n#include <glib-object.h>\n\nG_BEGIN_DECLS\n',
  fprod: '/* enumerations from "@filename@" */\n',
  vhead: 'GType @enum_name@_get_type (void) G_GNUC_CONST;\n#define GSD_TYPE_@ENUMSHORT@ (@enum_name@_get_type())\n',
  ftail: 'G_END_DECLS\n\n#endif /* !GSD_COMMON_ENUMS_H */'
)

gsd_common_enums += gnome.mkenums(
  enums + '.c',
  sources: enums_header,
  fhead: '#include "gsd-device-manager.h"\n#include "gsd-common-enums.h"\n',
  fprod: '\n/* enumerations from "@filename@" */',
  vhead: 'GType\n@enum_name@_get_type (void)\n{\n  static GType etype = 0;\n  if (etype == 0) {\n    static const G@Type@Value values[] = {',
  vprod: '      { @VALUENAME@, "@VALUENAME@", "@valuenick@" },',
  vtail: '      { 0, NULL, NULL }\n    };\n    etype = g_@type@_register_static ("@EnumName@", values);\n  }\n  return etype;\n}\n'
)

sources = files(
  'cc-util.c',
  'cc-common-language.c',
  'cc-language-chooser.c'
)

resource_data = files('language-chooser.ui')

sources += gnome.compile_resources(
  'cc-' + cappletname + '-resources',
  cappletname + '.gresource.xml',
  source_dir: '.',
  c_name: 'cc_' + cappletname,
  dependencies: resource_data,
  export: true
)

deps = common_deps + [
  gnome_desktop_dep,
  dependency('fontconfig')
]

liblanguage = static_library(
  'language',
  sources: sources + gsd_common_enums,
  include_directories: top_inc,
  dependencies: deps,
  c_args: cflags
)

sources = files(
  'gsd-device-manager-x11.c',
  'gsd-device-manager.c',
  'gsd-input-helper.c'
)

deps = common_deps

if have_udep
  sources += files('gsd-device-manager-udev.c')

  deps += [
    udev_dep,
    dependency('gdk-wayland-3.0', required: false)
  ]
endif

libdevice = static_library(
  'device',
  sources: sources + gsd_common_enums,
  include_directories: top_inc,
  dependencies: deps,
  c_args: cflags
)

install_data(
  'gnome-control-center.rules',
  install_dir: join_paths(control_center_datadir, 'polkit-1', 'rules.d')
)

# FIXME: updates gnome-settings-daemon code !?
'''
input_files = files(
  'gsd-device-manager-udev.c',
  'gsd-device-manager-udev.h',
  'gsd-device-manager-x11.c',
  'gsd-device-manager-x11.h',
  'gsd-device-manager.c',
  'gsd-device-manager.h',
  'gsd-input-helper.c',
  'gsd-input-helper.h'
)

install_data(
  input_files,
  install_dir: join_paths(meson.source_root(), '..', 'gnome-settings-daemon', 'plugins', 'common')
)

update-from-gsd:
  FILES="$(INPUTFILES)" DIR="$(INPUTDIR)" $(top_srcdir)/update-from-gsd.sh && changed=true ; \
  git commit -m "common: Update from gnome-settings-daemon" $(INPUTFILES)
'''
