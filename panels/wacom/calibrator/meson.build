calibrator_inc = include_directories('.')

common_sources = files(
  'calibrator-gui.c',
  'calibrator.c',
  'cc-clock-actor.c',
  'cc-target-actor.c'
)

calibrator_deps = deps + [m_dep]

libwacom_calibrator = static_library(
  cappletname + '-calibrator',
  sources: common_sources,
  include_directories: top_inc,
  dependencies: calibrator_deps,
  c_args: cflags
)

libwacom_calibrator_test = static_library(
  cappletname + '-calibrator-test',
  sources: common_sources,
  include_directories: top_inc,
  dependencies: calibrator_deps,
  c_args: test_cflags
)

executable(
  'test-calibrator',
  ['main.c'] + common_sources,
  include_directories: top_inc,
  dependencies: calibrator_deps,
  c_args: cflags
)
