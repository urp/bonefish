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
  bonefish/native/native_connector.hpp
  bonefish/native/native_endpoint.hpp
  bonefish/native/native_message_queue.hpp
  bonefish/native/native_server.hpp
  bonefish/rawsocket/rawsocket_listener.hpp
  bonefish/rawsocket/rawsocket_server.hpp
  bonefish/rawsocket/tcp_listener.hpp
  bonefish/rawsocket/uds_listener.hpp
  bonefish/router/wamp_router.hpp
  bonefish/router/wamp_routers.hpp
  bonefish/serialization/expandable_buffer.hpp
  bonefish/serialization/json_serializer.hpp
  bonefish/serialization/msgpack_serializer.hpp
  bonefish/serialization/wamp_serializer_type.hpp
  bonefish/serialization/wamp_serializer.hpp
  bonefish/serialization/wamp_serializers.hpp
  bonefish/trace/trace.hpp
  bonefish/websocket/websocket_server.hpp
)

set(PRIVATE_HEADERS
  bonefish/broker/wamp_broker.hpp
  bonefish/broker/wamp_broker_subscriptions.hpp
  bonefish/broker/wamp_broker_topic.hpp
  bonefish/common/wamp_connection_base.hpp
  bonefish/common/wamp_message_processor.hpp
  bonefish/dealer/wamp_dealer.hpp
  bonefish/dealer/wamp_dealer_invocation.hpp
  bonefish/dealer/wamp_dealer_registration.hpp
  bonefish/identifiers/wamp_publication_id.hpp
  bonefish/identifiers/wamp_publication_id_generator.hpp
  bonefish/identifiers/wamp_random_id_generator.hpp
  bonefish/identifiers/wamp_random_id.hpp
  bonefish/identifiers/wamp_registration_id.hpp
  bonefish/identifiers/wamp_registration_id_generator.hpp
  bonefish/identifiers/wamp_request_id.hpp
  bonefish/identifiers/wamp_request_id_generator.hpp
  bonefish/identifiers/wamp_sequential_id.hpp
  bonefish/identifiers/wamp_sequential_id_generator.hpp
  bonefish/identifiers/wamp_session_id.hpp
  bonefish/identifiers/wamp_session_id_factory.hpp
  bonefish/identifiers/wamp_session_id_generator.hpp
  bonefish/identifiers/wamp_subscription_id.hpp
  bonefish/identifiers/wamp_subscription_id_generator.hpp
  bonefish/messages/wamp_abort_message.hpp
  bonefish/messages/wamp_call_message.hpp
  bonefish/messages/wamp_call_options.hpp
  bonefish/messages/wamp_error_message.hpp
  bonefish/messages/wamp_event_message.hpp
  bonefish/messages/wamp_goodbye_message.hpp
  bonefish/messages/wamp_hello_details.hpp
  bonefish/messages/wamp_hello_message.hpp
  bonefish/messages/wamp_invocation_details.hpp
  bonefish/messages/wamp_invocation_message.hpp
  bonefish/messages/wamp_message.hpp
  bonefish/messages/wamp_message_defaults.hpp
  bonefish/messages/wamp_message_factory.hpp
  bonefish/messages/wamp_message_type.hpp
  bonefish/messages/wamp_publish_message.hpp
  bonefish/messages/wamp_published_message.hpp
  bonefish/messages/wamp_registered_message.hpp
  bonefish/messages/wamp_register_message.hpp
  bonefish/messages/wamp_result_details.hpp
  bonefish/messages/wamp_result_message.hpp
  bonefish/messages/wamp_subscribed_message.hpp
  bonefish/messages/wamp_subscribe_message.hpp
  bonefish/messages/wamp_unregistered_message.hpp
  bonefish/messages/wamp_unregister_message.hpp
  bonefish/messages/wamp_unsubscribed_message.hpp
  bonefish/messages/wamp_unsubscribe_message.hpp
  bonefish/messages/wamp_welcome_details.hpp
  bonefish/messages/wamp_welcome_message.hpp
  bonefish/messages/wamp_yield_message.hpp
  bonefish/messages/wamp_yield_options.hpp
  bonefish/native/native_connection.hpp
  bonefish/native/native_server_impl.hpp
  bonefish/native/native_transport.hpp
  bonefish/rawsocket/rawsocket_connection.hpp
  bonefish/rawsocket/rawsocket_server_impl.hpp
  bonefish/rawsocket/rawsocket_transport.hpp
  bonefish/rawsocket/tcp_connection.hpp
  bonefish/rawsocket/tcp_listener.hpp
  bonefish/rawsocket/uds_connection.hpp
  bonefish/rawsocket/uds_listener.hpp
  bonefish/roles/wamp_role.hpp
  bonefish/roles/wamp_role_features.hpp
  bonefish/roles/wamp_role_type.hpp
  bonefish/router/wamp_router_impl.hpp
  bonefish/session/wamp_session.hpp
  bonefish/session/wamp_session_state.hpp
  bonefish/transport/wamp_transport.hpp
  bonefish/utility/wamp_uri.hpp
  bonefish/websocket/websocket_config.hpp
  bonefish/websocket/websocket_protocol.hpp
  bonefish/websocket/websocket_server_impl.hpp
  bonefish/websocket/websocket_transport.hpp
)

add_library( bonefish ${SOURCES} )

add_library( bonefish::bonefish ALIAS bonefish )

set_target_properties( bonefish PROPERTIES VERSION ${PROJECT_VERSION} )

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
    install( FILES ${CMAKE_CURRENT_SOURCE_DIR}/src/${h} 
      DESTINATION ${HEADER_INCLUDE_DIRECTORY} 
      COMPONENT dev
    )
endforeach()

install(TARGETS bonefish EXPORT bonefish-targets 
  LIBRARY
    DESTINATION ${CMAKE_INSTALL_LIBDIR} 
    COMPONENT lib
  ARCHIVE 
    DESTINATION ${CMAKE_INSTALL_LIBDIR} 
    COMPONENT lib
  RUNTIME 
    DESTINATION ${CMAKE_INSTALL_LIBDIR} 
    COMPONENT lib
)

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
