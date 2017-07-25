panel_cflag_templates = [
  '-DG_LOG_DOMAIN="@0@-cc-panel"',
  '-DPANEL_ID="@0@"'
]

subdir('common')
subdir('background')
subdir('power')
subdir('color')
subdir('display')
subdir('mouse')
subdir('notifications')
subdir('online-accounts')
subdir('region')
subdir('info')
subdir('sound')
subdir('keyboard')
subdir('universal-access')
# FIXME: realmd file generation
subdir('user-accounts')
subdir('datetime')
subdir('search')
subdir('privacy')
subdir('sharing')
subdir('printers')

if have_wacom
  subdir('wacom')
endif

if have_network_manager
  subdir('network')
endif

if have_bluetooth
  subdir('bluetooth')
endif
