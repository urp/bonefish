cmake_minimum_required(VERSION 3.1 )

include( CMakePackageConfigHelpers )
include( GNUInstallDirs )

project( bonefish VERSION 0.1 )

option( BUILD_SHARED_LIBS "Build shared libraries when enabled. Build static libraries when disabled." ON )

list( APPEND CMAKE_MODULE_PATH
  "${CMAKE_CURRENT_SOURCE_DIR}/cmake/Modules/"
)

# if( NOT BUILD_SHARED_LIBS )
#   set(Boost_USE_STATIC_LIBS ON)
# endif()

if(WIN32)
    add_definitions(-D_WIN32_WINNT=0x0500)
    add_definitions(-DWIN32_LEAN_AND_MEAN)
endif()

find_package( Boost REQUIRED
  COMPONENTS
    program_options
    system
    thread
)

find_package( Msgpack REQUIRED )

find_package( Websocketpp REQUIRED )

find_package( RapidJSON 1.1 REQUIRED )


set( SOURCES
  src/bonefish/broker/wamp_broker.cpp
  src/bonefish/common/wamp_message_processor.cpp
  src/bonefish/dealer/wamp_dealer.cpp
  src/bonefish/identifiers/wamp_session_id_factory.cpp
  src/bonefish/messages/wamp_call_options.cpp
  src/bonefish/messages/wamp_hello_details.cpp
  src/bonefish/messages/wamp_invocation_details.cpp
  src/bonefish/messages/wamp_message_defaults.cpp
  src/bonefish/messages/wamp_message_factory.cpp
  src/bonefish/messages/wamp_message_type.cpp
  src/bonefish/messages/wamp_result_details.cpp
  src/bonefish/messages/wamp_welcome_details.cpp
  src/bonefish/messages/wamp_yield_options.cpp
  src/bonefish/native/native_connection.cpp
  src/bonefish/native/native_server.cpp
  src/bonefish/native/native_server_impl.cpp
  src/bonefish/native/native_transport.cpp
  src/bonefish/rawsocket/rawsocket_server.cpp
  src/bonefish/rawsocket/rawsocket_server_impl.cpp
  src/bonefish/rawsocket/rawsocket_transport.cpp
  src/bonefish/rawsocket/tcp_listener.cpp
  src/bonefish/rawsocket/uds_listener.cpp
  src/bonefish/roles/wamp_role_type.cpp
  src/bonefish/router/wamp_router.cpp
  src/bonefish/router/wamp_router_impl.cpp
  src/bonefish/serialization/json_serializer.cpp
  src/bonefish/serialization/msgpack_serializer.cpp
  src/bonefish/session/wamp_session_state.cpp
  src/bonefish/trace/trace.cpp
  src/bonefish/utility/wamp_uri.cpp
  src/bonefish/websocket/websocket_protocol.cpp
  src/bonefish/websocket/websocket_server.cpp
  src/bonefish/websocket/websocket_server_impl.cpp
  src/bonefish/websocket/websocket_transport.cpp
)

set(PUBLIC_HEADERS
  src/bonefish/native/native_connector.hpp
  src/bonefish/native/native_endpoint.hpp
  src/bonefish/native/native_message_queue.hpp
  src/bonefish/native/native_server.hpp
  src/bonefish/rawsocket/rawsocket_listener.hpp
  src/bonefish/rawsocket/rawsocket_server.hpp
  src/bonefish/rawsocket/tcp_listener.hpp
  src/bonefish/rawsocket/uds_listener.hpp
  src/bonefish/router/wamp_router.hpp
  src/bonefish/router/wamp_routers.hpp
  src/bonefish/serialization/expandable_buffer.hpp
  src/bonefish/serialization/json_serializer.hpp
  src/bonefish/serialization/msgpack_serializer.hpp
  src/bonefish/serialization/wamp_serializer_type.hpp
  src/bonefish/serialization/wamp_serializer.hpp
  src/bonefish/serialization/wamp_serializers.hpp
  src/bonefish/trace/trace.hpp
  src/bonefish/websocket/websocket_server.hpp
)

set(PRIVATE_HEADERS
  src/bonefish/broker/wamp_broker.hpp
  src/bonefish/broker/wamp_broker_subscriptions.hpp
  src/bonefish/broker/wamp_broker_topic.hpp
  src/bonefish/common/wamp_connection_base.hpp
  src/bonefish/common/wamp_message_processor.hpp
  src/bonefish/dealer/wamp_dealer.hpp
  src/bonefish/dealer/wamp_dealer_invocation.hpp
  src/bonefish/dealer/wamp_dealer_registration.hpp
  src/bonefish/identifiers/wamp_publication_id.hpp
  src/bonefish/identifiers/wamp_publication_id_generator.hpp
  src/bonefish/identifiers/wamp_random_id_generator.hpp
  src/bonefish/identifiers/wamp_random_id.hpp
  src/bonefish/identifiers/wamp_registration_id.hpp
  src/bonefish/identifiers/wamp_registration_id_generator.hpp
  src/bonefish/identifiers/wamp_request_id.hpp
  src/bonefish/identifiers/wamp_request_id_generator.hpp
  src/bonefish/identifiers/wamp_sequential_id.hpp
  src/bonefish/identifiers/wamp_sequential_id_generator.hpp
  src/bonefish/identifiers/wamp_session_id.hpp
  src/bonefish/identifiers/wamp_session_id_factory.hpp
  src/bonefish/identifiers/wamp_session_id_generator.hpp
  src/bonefish/identifiers/wamp_subscription_id.hpp
  src/bonefish/identifiers/wamp_subscription_id_generator.hpp
  src/bonefish/messages/wamp_abort_message.hpp
  src/bonefish/messages/wamp_call_message.hpp
  src/bonefish/messages/wamp_call_options.hpp
  src/bonefish/messages/wamp_error_message.hpp
  src/bonefish/messages/wamp_event_message.hpp
  src/bonefish/messages/wamp_goodbye_message.hpp
  src/bonefish/messages/wamp_hello_details.hpp
  src/bonefish/messages/wamp_hello_message.hpp
  src/bonefish/messages/wamp_invocation_details.hpp
  src/bonefish/messages/wamp_invocation_message.hpp
  src/bonefish/messages/wamp_message.hpp
  src/bonefish/messages/wamp_message_defaults.hpp
  src/bonefish/messages/wamp_message_factory.hpp
  src/bonefish/messages/wamp_message_type.hpp
  src/bonefish/messages/wamp_publish_message.hpp
  src/bonefish/messages/wamp_published_message.hpp
  src/bonefish/messages/wamp_registered_message.hpp
  src/bonefish/messages/wamp_register_message.hpp
  src/bonefish/messages/wamp_result_details.hpp
  src/bonefish/messages/wamp_result_message.hpp
  src/bonefish/messages/wamp_subscribed_message.hpp
  src/bonefish/messages/wamp_subscribe_message.hpp
  src/bonefish/messages/wamp_unregistered_message.hpp
  src/bonefish/messages/wamp_unregister_message.hpp
  src/bonefish/messages/wamp_unsubscribed_message.hpp
  src/bonefish/messages/wamp_unsubscribe_message.hpp
  src/bonefish/messages/wamp_welcome_details.hpp
  src/bonefish/messages/wamp_welcome_message.hpp
  src/bonefish/messages/wamp_yield_message.hpp
  src/bonefish/messages/wamp_yield_options.hpp
  src/bonefish/native/native_connection.hpp
  src/bonefish/native/native_server_impl.hpp
  src/bonefish/native/native_transport.hpp
  src/bonefish/rawsocket/rawsocket_connection.hpp
  src/bonefish/rawsocket/rawsocket_server_impl.hpp
  src/bonefish/rawsocket/rawsocket_transport.hpp
  src/bonefish/rawsocket/tcp_connection.hpp
  src/bonefish/rawsocket/tcp_listener.hpp
  src/bonefish/rawsocket/uds_connection.hpp
  src/bonefish/rawsocket/uds_listener.hpp
  src/bonefish/roles/wamp_role.hpp
  src/bonefish/roles/wamp_role_features.hpp
  src/bonefish/roles/wamp_role_type.hpp
  src/bonefish/router/wamp_router_impl.hpp
  src/bonefish/session/wamp_session.hpp
  src/bonefish/session/wamp_session_state.hpp
  src/bonefish/transport/wamp_transport.hpp
  src/bonefish/utility/wamp_uri.hpp
  src/bonefish/websocket/websocket_config.hpp
  src/bonefish/websocket/websocket_protocol.hpp
  src/bonefish/websocket/websocket_server_impl.hpp
  src/bonefish/websocket/websocket_transport.hpp
)

add_library( bonefish ${SOURCES} )

add_library( bonefish::bonefish ALIAS bonefish )

target_link_libraries( bonefish Boost::system Boost::thread )

target_include_directories( bonefish
  PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
    $<INSTALL_INTERFACE:$<INSTALL_PREFIX>/include>
  PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/third-party/cppcodec
    ${CMAKE_CURRENT_SOURCE_DIR}/third-party/json-msgpack/include
)

foreach(h ${PUBLIC_HEADERS})
    get_filename_component(HEADER_INCLUDE_DIRECTORY ${CMAKE_INSTALL_INCLUDEDIR}/${h} DIRECTORY)
    install(FILES ${CMAKE_CURRENT_SOURCE_DIR}/${h} DESTINATION ${HEADER_INCLUDE_DIRECTORY} COMPONENT "dev")
endforeach()

install(TARGETS bonefish DESTINATION ${CMAKE_INSTALL_LIBDIR} EXPORT bonefish-targets )

# Create package build configuration in build directory

export( EXPORT bonefish-targets
  FILE
    "${CMAKE_CURRENT_BINARY_DIR}/bonefish-targets.cmake"
  NAMESPACE
    bonefish::
)

write_basic_package_version_file(
  "${CMAKE_CURRENT_BINARY_DIR}/bonefish-config-version.cmake"
  VERSION
    ${${PROJECT_NAME}_VERSION}
  COMPATIBILITY
    AnyNewerVersion
)

configure_package_config_file(
  "${CMAKE_CURRENT_SOURCE_DIR}/cmake/bonefish-config.cmake.in"
  "bonefish-config.cmake"
  INSTALL_DESTINATION
    "${CMAKE_CURRENT_BINARY_DIR}"
)

# Export build configuration into local cmake repository

export( PACKAGE bonefish )

# Install package configuration for installed targets

set( package_config_install_dir "${CMAKE_INSTALL_LIBDIR}/cmake/bonefish" )

install( EXPORT bonefish-targets
  DESTINATION
    ${package_config_install_dir}
  NAMESPACE
    bonefish::
  COMPONENT
    dev
)

install(
  FILES
    "${CMAKE_CURRENT_BINARY_DIR}/bonefish-config.cmake"
    "${CMAKE_CURRENT_BINARY_DIR}/bonefish-config-version.cmake"
  DESTINATION
    ${package_config_install_dir}
  COMPONENT
    dev
)

add_subdirectory(examples)

add_subdirectory(daemon)

add_subdirectory(test)
