add_executable(bonefish_bin
  bonefish.cpp
  daemon.cpp
  daemon_options.cpp
)

set_target_properties(bonefish_bin PROPERTIES OUTPUT_NAME bonefish)

target_link_libraries(bonefish_bin
    bonefish::bonefish
    Boost::program_options
)
