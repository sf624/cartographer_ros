google_library(assets_writer
  USES_CARTOGRAPHER
  SRCS
    assets_writer.cc
  HDRS
    assets_writer.h
  DEPENDS
    map_writer
    node_options
    occupancy_grid
)

google_library(map_builder_bridge
  USES_CARTOGRAPHER
  SRCS
    map_builder_bridge.cc
  HDRS
    map_builder_bridge.h
  DEPENDS
    assets_writer
    msg_conversion
    node_options
    occupancy_grid
    sensor_bridge
    tf_bridge
)

google_library(map_writer
  USES_GLOG
  USES_YAMLCPP
  SRCS
    map_writer.cc
  HDRS
    map_writer.h
)

google_library(msg_conversion
  USES_CARTOGRAPHER
  USES_GLOG
  USES_PCL
  SRCS
    msg_conversion.cc
  HDRS
    msg_conversion.h
  DEPENDS
    time_conversion
)

google_library(node
  USES_CARTOGRAPHER
  USES_EIGEN
  USES_GLOG
  SRCS
    node.cc
  HDRS
    node.h
  DEPENDS
    map_builder_bridge
    msg_conversion
    node_options
    sensor_bridge
    tf_bridge
    time_conversion
)

google_library(node_options
  USES_CARTOGRAPHER
  USES_GLOG
  SRCS
    node_options.cc
  HDRS
    node_options.h
  DEPENDS
    sensor_bridge
)

google_library(occupancy_grid
  USES_CARTOGRAPHER
  USES_GLOG
  SRCS
    occupancy_grid.cc
  HDRS
    occupancy_grid.h
  DEPENDS
    node_options
    time_conversion
)

google_library(ros_log_sink
  USES_GLOG
  SRCS
    ros_log_sink.cc
  HDRS
    ros_log_sink.h
)

google_library(sensor_bridge
  USES_CARTOGRAPHER
  SRCS
    sensor_bridge.cc
  HDRS
    sensor_bridge.h
  DEPENDS
    msg_conversion
    tf_bridge
    time_conversion
)

google_library(tf_bridge
  USES_CARTOGRAPHER
  SRCS
    tf_bridge.cc
  HDRS
    tf_bridge.h
  DEPENDS
    msg_conversion
    time_conversion
)

google_library(time_conversion
  USES_CARTOGRAPHER
  SRCS
    time_conversion.cc
  HDRS
    time_conversion.h
)

google_library(urdf_reader
  USES_CARTOGRAPHER
  SRCS
    urdf_reader.cc
  HDRS
    urdf_reader.h
  DEPENDS
    msg_conversion
)

google_test(configuration_files_test
  USES_CARTOGRAPHER
  SRCS
    configuration_files_test.cc
  DEPENDS
    node_options
)

google_test(time_conversion_test
  USES_CARTOGRAPHER
  SRCS
    time_conversion_test.cc
  DEPENDS
    time_conversion
)

google_binary(cartographer_assets_writer
  USES_CARTOGRAPHER
  USES_GFLAGS
  USES_GLOG
  SRCS
    assets_writer_main.cc
  DEPENDS
    msg_conversion
    time_conversion
    urdf_reader
)

google_binary(cartographer_node
  USES_CARTOGRAPHER
  SRCS
    node_main.cc
  DEPENDS
    node
    ros_log_sink
)

install(TARGETS cartographer_node
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(TARGETS cartographer_assets_writer
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
google_binary(cartographer_offline_node
  USES_CARTOGRAPHER
  SRCS
    offline_node_main.cc
  DEPENDS
    node
    ros_log_sink
    urdf_reader
)
