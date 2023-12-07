set(package_name PahoMqttCpp)
configure_file(${package_name}Config.cmake.in ${package_name}Config.cmake @ONLY)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
  "${CMAKE_CURRENT_BINARY_DIR}/${package_name}ConfigVersion.cmake"
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY SameMajorVersion) # TODO

if(${PAHO_BUILD_SHARED})
  list(APPEND targets paho-mqttpp3)
endif()

# static version doesn't work and idk why

# if(${PAHO_BUILD_STATIC}) list(APPEND targets paho-mqttpp3-static) endif()

# if(${targets})
#   install(
#     TARGETS ${targets}
#     EXPORT ${PROJECT_NAME}Targets
#     LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
#     RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
#     ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
#     INCLUDES
#     DESTINATION include
#     PUBLIC_HEADER DESTINATION include)
#   #
#   install(
#     EXPORT ${PROJECT_NAME}Targets
#     FILE ${PROJECT_NAME}Targets.cmake
#     NAMESPACE ${PROJECT_NAME}::
#     DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME})
# endif()

# PackageProject.cmake will be used to make our target installable
CPMAddPackage("gh:TheLartians/PackageProject.cmake@1.8.0")
packageProject(
  NAME ${targets}
  VERSION ${PROJECT_VERSION}
  NAMESPACE ${PROJECT_NAME}
  BINARY_DIR ${PROJECT_BINARY_DIR}
  INCLUDE_DIR ${PROJECT_SOURCE_DIR}/include
  INCLUDE_DESTINATION include/${PROJECT_NAME}-${PROJECT_VERSION}
  VERSION_HEADER "${VERSION_HEADER_LOCATION}"
  COMPATIBILITY SameMajorVersion
  DEPENDENCIES "paho-mqtt 1.3.13"
)
