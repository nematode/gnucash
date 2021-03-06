
SET (gnome_search_SOURCES
  gncmod-gnome-search.c
  gnc-general-search.c
  dialog-search.c
  search-account.c
  search-boolean.c
  search-core-type.c
  search-core-utils.c
  search-date.c
  search-double.c
  search-int64.c
  search-numeric.c
  search-reconciled.c
  search-string.c
)

SET (gnome_search_HEADERS
  dialog-search.h
  gnc-general-search.h
)

SET (gnome_search_noinst_HEADERS
  search-account.h
  search-boolean.h
  search-core-type.h
  search-core-utils.h
  search-date.h
  search-double.h
  search-int64.h
  search-numeric.h
  search-reconciled.h
  search-string.h
)  

# Add dependency on config.h
SET_SOURCE_FILES_PROPERTIES (${gnome_search_SOURCES} PROPERTIES OBJECT_DEPENDS ${CONFIG_H})

SET_LOCAL_DIST(gnome_search_DIST_local CMakeLists.txt Makefile.am  dialog-search.glade ${gnome_search_SOURCES} ${gnome_search_HEADERS}
        ${gnome_search_noinst_HEADERS})
SET(gnome_search_DIST ${gnome_search_DIST_local} PARENT_SCOPE)

ADD_LIBRARY (gncmod-gnome-search
    ${gnome_search_SOURCES}
    ${gnome_search_HEADERS}
    ${gnome_search_noinst_HEADERS}
)

TARGET_LINK_LIBRARIES(gncmod-gnome-search gncmod-gnome-utils ${GTK3_LDFLAGS})

TARGET_COMPILE_DEFINITIONS(gncmod-gnome-search PRIVATE -DG_LOG_DOMAIN=\"gnc.gui.search\")

TARGET_INCLUDE_DIRECTORIES(gncmod-gnome-search PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

INSTALL(TARGETS gncmod-gnome-search
  LIBRARY DESTINATION lib/gnucash
  ARCHIVE DESTINATION lib/gnucash
  RUNTIME DESTINATION bin)

INSTALL(FILES ${gnome_search_HEADERS} DESTINATION include/gnucash)

SET(gnome_search_GLADE dialog-search.glade)

INSTALL(FILES ${gnome_search_GLADE} DESTINATION share/gnucash/gtkbuilder)

FILE(COPY .
    DESTINATION ${DATADIR_BUILD}/gnucash/gtkbuilder
    PATTERN Makefile.* EXCLUDE
    PATTERN CMakeLists.txt EXCLUDE)

