# 收集 core 源码
src_dir = meson.current_source_dir() / 'src'
sources = files(
  'src/container/pair/key_value_pair.cpp',
  'src/control/pid.cpp',
  'src/control/ring_pid.cpp',
  'src/imu/wit_private_protocol.cpp',
  'src/lidar/ldrobot/ld_lidar.cpp',
  'src/motor/damiao/damiao_controller.cpp',
  'src/motor/dji/c6xx_controller.cpp',
  'src/motor/dji/dji_motor.cpp',
  'src/motor/dji/m2006_motor.cpp',
  'src/motor/dji/m3508_motor.cpp',
  'src/motor/general/encoder_motor.cpp',
  'src/servo/fashion_star/fashion_star_protocol.cpp',
  'src/servo/feetech/feetech_SCS.cpp',
  'src/servo/feetech/feetech_SMS.cpp',
  'src/servo/feetech/feetech_STS.cpp',
  'src/servo/feetech/feetech_protocol.cpp',
  'src/servo/huaner/huaner_protocol.cpp',
  'src/servo/servo.cpp',
  'src/transfer/binary_transfer.cpp',
  'src/transfer/can_bus.cpp',
  'src/transfer/circular_buffer.cpp',
  'src/transfer/crc.cpp',
  'src/transfer/io_stream.cpp',
  'src/transfer/reliable_binary_transfer.cpp',
  'src/transfer/serial_port.cpp',
  'src/transfer/serial_to_can.cpp',
  'src/transfer/simple_binary_transfer.cpp',
  'src/transfer/simple_message_transfer.cpp',
  'src/transfer/tcp_port.cpp',
  'src/underpan/differential_motion.cpp',
  'src/underpan/mecanum_motion.cpp',
  'src/underpan/omnidirectional_motion.cpp',
  'src/visualization/point_cloud_view.cpp',
  'src/vofa/vofa_tcp_server.cpp'
)

rdk_core = static_library(
  'rdk_core',
  sources,
  include_directories: core_inc,
  dependencies: [boost_dep, opencv_dep],
  install: true
)

# 安装头文件
install_subdir(
  'include',
  install_dir: 'include'
)

# 自动查找所有 test_*.cpp 并生成可执行文件
test_dir = meson.current_source_dir() / 'test'
test_sources = [
  'test/test_serial_port.cpp',
  'test/test_simple_message_transfer.cpp',
  'test/test_simple_message_transfer_with_stm32.cpp',
  'test/test_simple_binary_transfer.cpp',
  'test/test_simple_binary_transfer_with_stm32.cpp',
  'test/test_reliable_binary_transfer.cpp',
  'test/test_reliable_binary_transfer_with_stm32.cpp',
  'test/test_jy901b_imu.cpp',
  'test/test_serial_to_can.cpp',
  'test/test_c6xx_controller.cpp',
  'test/test_dji_motor.cpp',
  'test/test_m3508_motor.cpp',
  'test/test_damiao_controller.cpp',
  'test/test_hik_camera.cpp',
  'test/test_fashion_star_servo.cpp',
  'test/test_feetech_servo.cpp',
  'test/test_key_value_pair.cpp',
  'test/test_ld_lidar.cpp',
  'test/test_point_cloud_view.cpp',
  'test/test_huaner_servo.cpp',
  'test/test_tcp_port.cpp',
  'test/test_tcp_port_server.cpp',
  'test/test_tcp_port_client.cpp',
  'test/test_tcp_client_message_transfer.cpp',
  'test/test_tcp_server_message_transfer.cpp',
  'test/test_tcp_client_simple_binary_transfer.cpp',
  'test/test_tcp_server_simple_binary_transfer.cpp',
  'test/test_tcp_client_binary_transfer.cpp',
  'test/test_tcp_server_binary_transfer.cpp'
]

foreach src : test_sources
  exe_name = src.split('/')[-1].split('.cpp')[0]
  executable(
    exe_name,
    src,
    include_directories: core_inc,
    dependencies: [boost_dep, opencv_dep],
    link_with: rdk_core,
    install: false
  )
endforeach
