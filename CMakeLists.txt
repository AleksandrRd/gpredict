cmake_minimum_required(VERSION 3.10)

project(gpredict C)

add_compile_definitions(
    VERSION=\"2.2.1\"
    PACKAGE_DATA_DIR=\"${CMAKE_INSTALL_FULL_DATAROOTDIR}/gpredict\"
#    PACKAGE_PIXMAPS_DIR=\"${CMAKE_INSTALL_FULL_DATAROOTDIR}/pixmaps\"
    PACKAGE_PIXMAPS_DIR=\"pixmaps\"
    GETTEXT_PACKAGE=\"gpredict\"
)



set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)

# Включить дополнительные предупреждения
if(CMAKE_C_COMPILER_ID MATCHES "GNU|Clang")
    add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# Найти зависимости через pkg-config
find_package(PkgConfig REQUIRED)

pkg_check_modules(GTK3 REQUIRED gtk+-3.0)
# pkg_check_modules(GTK2 REQUIRED gtk+-2.0)
pkg_check_modules(GLIB REQUIRED glib-2.0 gobject-2.0 gmodule-2.0)
pkg_check_modules(GTHREAD REQUIRED gthread-2.0)
pkg_check_modules(GIO REQUIRED gio-2.0)
pkg_check_modules(CURL REQUIRED libcurl)
pkg_check_modules(LIBXML2 REQUIRED libxml-2.0)
pkg_check_modules(LIBCONFIG REQUIRED libconfig)

pkg_check_modules(GOO REQUIRED goocanvas-2.0)

# При необходимости можете добавить сюда другие зависимости, которые используются в исходниках:
# pkg_check_modules(GSL REQUIRED gsl)

include_directories(
    ${GTK3_INCLUDE_DIRS}
    ${GLIB_INCLUDE_DIRS}
    ${GTHREAD_INCLUDE_DIRS}
    ${GIO_INCLUDE_DIRS}
    ${CURL_INCLUDE_DIRS}
    ${LIBXML2_INCLUDE_DIRS}
    ${LIBCONFIG_INCLUDE_DIRS}
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${GOO_INCLUDE_DIRS}/src
)

link_directories(
    ${GTK3_LIBRARY_DIRS}
    ${GLIB_LIBRARY_DIRS}
    ${GTHREAD_LIBRARY_DIRS}
    ${GIO_LIBRARY_DIRS}
    ${CURL_LIBRARY_DIRS}
    ${LIBXML2_LIBRARY_DIRS}
    ${LIBCONFIG_LIBRARY_DIRS}
    ${GOO_LIBRARY_DIRS}
)

add_definitions(
    ${GTK3_CFLAGS_OTHER}
    ${GLIB_CFLAGS_OTHER}
    ${GTHREAD_CFLAGS_OTHER}
    ${GIO_CFLAGS_OTHER}
    ${CURL_CFLAGS_OTHER}
    ${LIBXML2_CFLAGS_OTHER}
    ${LIBCONFIG_CFLAGS_OTHER}
    ${GOO_CFLAGS_OTHER}
)

# Список всех .c файлов проекта.
# Удобнее всего сгенерировать его скриптом или временно использовать
# aux_source_directory(), а потом зафиксировать вручную.

set(GPREDICT_SOURCES
    src/about.c
    src/compat.c
    src/first-time.c
    src/gpredict-help.c
    src/gpredict-utils.c
    src/gtk-azel-plot.c
    src/gtk-event-list-popup.c
    src/gtk-event-list.c
    src/gtk-freq-knob.c
    src/gtk-polar-plot.c
    src/gtk-polar-view-popup.c
    src/gtk-polar-view.c
    src/gtk-rig-ctrl.c
    src/gtk-rot-ctrl.c
    src/gtk-rot-knob.c
    src/gtk-sat-data.c
    src/gtk-sat-list-popup.c
    src/gtk-sat-list.c
    src/gtk-sat-map-ground-track.c
    src/gtk-sat-map-popup.c
    src/gtk-sat-map.c
    src/gtk-sat-module-popup.c
    src/gtk-sat-module-tmg.c
    src/gtk-sat-module.c
    src/gtk-sat-popup-common.c
    src/gtk-sat-selector.c
    src/gtk-sat-tree.c
    src/gtk-single-sat.c
    src/gtk-sky-glance.c
    src/gui.c
    src/loc-tree.c
    src/locator.c
    src/main.c
    src/map-selector.c
    src/map-tools.c
    src/menubar.c
    src/mod-cfg-get-param.c
    src/mod-cfg.c
    src/mod-mgr.c
    src/nxjson/nxjson.c
    src/orbit-tools.c
    src/pass-popup-menu.c
    src/pass-to-txt.c
    src/predict-tools.c
    src/print-pass.c
    src/qth-data.c
    src/qth-editor.c
    src/radio-conf.c
    src/rotor-conf.c
    src/sat-cfg.c
    src/sat-info.c
    src/sat-log-browser.c
    src/sat-log.c
    src/sat-monitor.c
    src/sat-pass-dialogs.c
    src/sat-pref-conditions.c
    src/sat-pref-debug.c
    src/sat-pref-formats.c
    src/sat-pref-general.c
    src/sat-pref-interfaces.c
    src/sat-pref-layout.c
    src/sat-pref-list-view.c
    src/sat-pref-map-view.c
    src/sat-pref-modules.c
    src/sat-pref-multi-pass.c
    src/sat-pref-polar-view.c
    src/sat-pref-predict.c
    src/sat-pref-qth-editor.c
    src/sat-pref-qth.c
    src/sat-pref-refresh.c
    src/sat-pref-rig-editor.c
    src/sat-pref-rig.c
    src/sat-pref-rot-editor.c
    src/sat-pref-rot.c
    src/sat-pref-single-pass.c
    src/sat-pref-single-sat.c
    src/sat-pref-sky-at-glance.c
    src/sat-pref-tle.c
    src/sat-pref.c
    src/sat-vis.c
    src/save-pass.c
    src/sgpsdp/sgp4sdp4.c
    src/sgpsdp/sgp_in.c
    src/sgpsdp/sgp_math.c
    src/sgpsdp/sgp_obs.c
    src/sgpsdp/sgp_time.c
    src/sgpsdp/solar.c
#    src/sgpsdp/test-001.c
#    src/sgpsdp/test-002.c
    src/strnatcmp.c
    src/time-tools.c
    src/tle-tools.c
    src/tle-update.c
    src/trsp-conf.c
    src/trsp-update.c
)

add_executable(gpredict ${GPREDICT_SOURCES})

target_link_libraries(gpredict
    ${GTK3_LIBRARIES}
    ${GLIB_LIBRARIES}
    ${GTHREAD_LIBRARIES}
    ${GIO_LIBRARIES}
    ${CURL_LIBRARIES}
    ${LIBXML2_LIBRARIES}
    ${LIBCONFIG_LIBRARIES}
    ${GOO_LIBRARIES}
    m
)

# Установочные пути (пример для /usr/local)
include(GNUInstallDirs)

install(TARGETS gpredict
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

# Данные, иконки, desktop-файл и т.п. при необходимости добавляются так:
# install(FILES data/gpredict.desktop DESTINATION ${CMAKE_INSTALL_DATADIR}/applications)
# install(DIRECTORY share/icons DESTINATION ${CMAKE_INSTALL_DATADIR})
