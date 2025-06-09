project(
  'RobotDevKit',
  'cpp',
  version: '1.0',
  default_options: ['cpp_std=c++20']
)

boost_dep = dependency('boost', required: true)
opencv_dep = dependency('opencv4', required: true)

core_inc = include_directories('modules/core/include')

subdir('modules/core')

install_data(
  'cmake/RobotDevKitConfig.cmake',
  install_dir: 'lib/cmake/RobotDevKit'
)
