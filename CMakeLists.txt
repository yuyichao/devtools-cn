project(devtools-cn)

## Setup
cmake_minimum_required(VERSION 3.0)
set(CMAKE_MODULE_PATH "${CMAKE_MODULE_PATH}"
  "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

include(GNUInstallDirs)

function(install_build_script cmd)
  add_custom_command(OUTPUT "${CMAKE_CURRENT_BINARY_DIR}/cmds/${cmd}"
    COMMAND "${CMAKE_COMMAND}" -E make_directory
    "${CMAKE_CURRENT_BINARY_DIR}/cmds"
    COMMAND "${CMAKE_COMMAND}" -E create_symlink archbuild
    "${CMAKE_CURRENT_BINARY_DIR}/cmds/${cmd}")
  add_custom_target(${cmd}.target ALL
    DEPENDS "${CMAKE_CURRENT_BINARY_DIR}/cmds/${cmd}")
endfunction()

install_build_script(archlinuxcn-i686-build)
install_build_script(archlinuxcn-x86_64-build)
install_build_script(archlinuxcn-testing-x86_64-build)
install_build_script(archlinuxcn-staging-x86_64-build)
install_build_script(archlinuxcn-kde-unstable-x86_64-build)
install_build_script(multilib-archlinuxcn-build)

foreach(ARM_ARCH arm armv6h armv7h aarch64)
  install_build_script("archlinuxcn-${ARM_ARCH}-build")
endforeach()

install(DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/cmds/"
  DESTINATION "${CMAKE_INSTALL_BINDIR}")
install(FILES
  "pacman-archlinuxcn.conf"
  "pacman-archlinuxcn-testing.conf"
  "pacman-archlinuxcn-staging.conf"
  "pacman-archlinuxcn-kde-unstable.conf"
  "pacman-multilib-archlinuxcn.conf"
  DESTINATION "${CMAKE_INSTALL_DATAROOTDIR}/devtools/")
