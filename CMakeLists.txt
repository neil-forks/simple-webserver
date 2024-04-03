cmake_minimum_required(VERSION 3.5)
project(lexiserver C)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_C_STANDARD 99)

set(LEXISERVER_VERSION 1.1.0)

# Add OpenSSL package
find_package(OpenSSL REQUIRED)

# Add executable target
add_executable(lexiserver src/lexiserver.c)

set_target_properties(lexiserver PROPERTIES
        OUTPUT_NAME "lexiserver"
)

target_compile_definitions(lexiserver PRIVATE DEFAULT_LEXISERVER=lexiserver-${LEXISERVER_VERSION})

# Link OpenSSL libraries
target_link_libraries(lexiserver OpenSSL::SSL OpenSSL::Crypto)

# Take care of installation
include(GNUInstallDirs)
install(TARGETS lexiserver
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

# spec stuff
configure_file(
  "rpm/lexiserver.spec.in"
  "rpm/lexiserver.spec"
  @ONLY
)
