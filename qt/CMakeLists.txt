project(desktop)

set(CMAKE_AUTOMOC ON)

QT5_ADD_RESOURCES(RES_SOURCES res/resources.qrc)

set(RESOURCES_FOLDER ${CMAKE_BINARY_DIR}/${PROJECT_NAME}.app/Contents/Resources)
set(DATA_DIR ${OMIM_ROOT}/data)

execute_process(
    COMMAND mkdir -p ${RESOURCES_FOLDER}
)

include_directories(
  ${OMIM_ROOT}/3party/glm
)

set(
  SRC
  about.cpp
  about.hpp
  create_feature_dialog.cpp
  create_feature_dialog.hpp
  draw_widget.cpp
  draw_widget.hpp
  editor_dialog.cpp
  editor_dialog.hpp
  info_dialog.cpp
  info_dialog.hpp
  main.cpp
  mainwindow.cpp
  mainwindow.hpp
  osm_auth_dialog.cpp
  osm_auth_dialog.hpp
  place_page_dialog.cpp
  place_page_dialog.hpp
  preferences_dialog.cpp
  preferences_dialog.hpp
  proxystyle.cpp
  proxystyle.hpp
  qtoglcontext.cpp
  qtoglcontext.hpp
  qtoglcontextfactory.cpp
  qtoglcontextfactory.hpp
  search_panel.cpp
  search_panel.hpp
  slider_ctrl.cpp
  slider_ctrl.hpp
  update_dialog.cpp
  update_dialog.hpp
)

add_executable(${PROJECT_NAME} MACOSX_BUNDLE ${RES_SOURCES} ${SRC})

omim_link_libraries(
  ${PROJECT_NAME}
  map
  drape_frontend
  routing
  search
  storage
  tracking
  traffic
  indexer
  drape
  partners_api
  platform
  editor geometry
  coding
  base
  freetype
  expat
  fribidi
  jansson
  protobuf
  osrm
  stats_client
  minizip
  succinct
  pugixml
  oauthcpp
  opening_hours
  ${Qt5Gui_LIBRARIES}
  ${Qt5Network_LIBRARIES}
  ${Qt5Widgets_LIBRARIES}
  ${LIBZ}
)

link_qt5_core(${PROJECT_NAME})

if (PLATFORM_MAC)
  target_link_libraries(
    ${PROJECT_NAME}
    "-framework Foundation"
    "-framework CoreLocation"
    "-framework CoreWLAN"
    "-framework SystemConfiguration"
    "-framework CFNetwork"
  )

  set_target_properties(
    ${PROJECT_NAME}
    PROPERTIES
    MACOSX_BUNDLE_INFO_PLIST ${PROJECT_SOURCE_DIR}/res/Info.plist
  )
endif()

link_opengl(${PROJECT_NAME})
link_qt5_core(${PROJECT_NAME})

function(copy_resources)
  set(files "")
  foreach(file ${ARGN})
    execute_process(
      COMMAND cp -r ${DATA_DIR}/${file} ${RESOURCES_FOLDER}
    )
  endforeach()
endfunction()

copy_resources(
  countries-strings
  resources-default
  resources-mdpi_clear
  cuisine-strings

  banners.txt
  categories.txt
  classificator.txt
  colors.txt
  copyright.html
  countries.txt
  countries_obsolete.txt
  drules_proto_clear.bin
  drules_proto_dark.bin
  editor.config
  fonts_blacklist.txt
  fonts_whitelist.txt
  packed_polygons.bin
  packed_polygons_obsolete.bin
  patterns.txt
  types.txt
  unicode_blocks.txt
  World.mwm
  WorldCoasts.mwm
  WorldCoasts_obsolete.mwm

  01_dejavusans.ttf
  02_droidsans-fallback.ttf
  03_jomolhari-id-a3d.ttf
  04_padauk.ttf
  05_khmeros.ttf
  06_code2000.ttf
  07_roboto_medium.ttf
)