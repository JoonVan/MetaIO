PROJECT(ITKZLIB)
INCLUDE_REGULAR_EXPRESSION("^(itk|deflate|inf|trees|zconf|zlib|zutil).*$")

INCLUDE_DIRECTORIES(BEFORE ${ITKZLIB_SOURCE_DIR})

# source files for zlib
SET(ZLIB_SRCS
  adler32.c  compress.c  crc32.c  deflate.c  gzio.c   inffast.c
  inflate.c  inftrees.c  trees.c  uncompr.c  zutil.c
  )

# for windows add the .def and .rc files to the source list
# if building shared libs
IF(WIN32)
  IF(BUILD_SHARED_LIBS)
    SET(ZLIB_DLL 1)
    IF(NOT UNIX)
      IF(NOT BORLAND)
        IF(NOT MINGW)
          SET(ZLIB_SRCS ${ZLIB_SRCS} zlib.def zlib.rc  )
        ENDIF(NOT MINGW)
      ENDIF(NOT BORLAND)
    ENDIF(NOT UNIX)
  ENDIF(BUILD_SHARED_LIBS)
ENDIF(WIN32)

CONFIGURE_FILE(${ITKZLIB_SOURCE_DIR}/.NoDartCoverage
               ${ITKZLIB_BINARY_DIR}/.NoDartCoverage)
CONFIGURE_FILE(${ITKZLIB_SOURCE_DIR}/zlibDllConfig.h.in
               ${ITKZLIB_BINARY_DIR}/zlibDllConfig.h)


ADD_LIBRARY(itkzlib ${ZLIB_SRCS})

# Apply user-defined properties to the library target.
IF(ITK_LIBRARY_PROPERTIES)
  SET_TARGET_PROPERTIES(itkzlib PROPERTIES ${ITK_LIBRARY_PROPERTIES})
ENDIF(ITK_LIBRARY_PROPERTIES)

INSTALL(TARGETS itkzlib
  RUNTIME DESTINATION ${ITK_INSTALL_BIN_DIR_CM24} COMPONENT RuntimeLibraries
  LIBRARY DESTINATION ${ITK_INSTALL_LIB_DIR_CM24} COMPONENT RuntimeLibraries
  ARCHIVE DESTINATION ${ITK_INSTALL_LIB_DIR_CM24} COMPONENT Development)

INSTALL(FILES
  ${ITKZLIB_SOURCE_DIR}/zlib.h
  ${ITKZLIB_SOURCE_DIR}/zconf.h
  ${ITKZLIB_SOURCE_DIR}/itk_zlib_mangle.h
  ${ITKZLIB_BINARY_DIR}/zlibDllConfig.h
  DESTINATION ${ITK_INSTALL_INCLUDE_DIR_CM24}/itkzlib
  COMPONENT Development)
