set(SOURCES integration.cpp)

add_executable(integration ${SOURCES})

add_dependencies(integration bonefish)

target_link_libraries(integration
    bonefish::bonefish
)
