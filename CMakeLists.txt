cmake_minimum_required(VERSION 2.6)

project(libxml2)

option(BUILD_SHARED_LIBS "build a shared library" OFF)

if(NOT DEFINED CMAKE_DEBUG_POSTFIX)
	set(CMAKE_DEBUG_POSTFIX "d")
endif()

set(VERSION "2.9.2")
set(LIBXML_VERSION_NUMBER 20902)
set(LIBXML_VERSION_EXTRA)
set(WITH_TRIO 0)
set(WITH_THREADS 1)
set(WITH_THREAD_ALLOC 1)
set(WITH_TREE 1)
set(WITH_OUTPUT 1)
set(WITH_PUSH 1)
set(WITH_READER 1)
set(WITH_PATTERN 0)
set(WITH_WRITER 0)
set(WITH_SAX1 1)
set(WITH_FTP 0)
set(WITH_HTTP 0)
set(WITH_VALID 0)
set(WITH_HTML 0)
set(WITH_LEGACY 0)
set(WITH_C14N 0)
set(WITH_CATALOG 0)
set(WITH_DOCB 0)
set(WITH_XPATH 0)
set(WITH_XPTR 0)
set(WITH_XINCLUDE 0)
set(WITH_ICONV 0)
set(WITH_ICU 0)
set(WITH_ISO8859X 0)
set(WITH_DEBUG 0)
set(WITH_MEM_DEBUG 0)
set(WITH_RUN_DEBUG 0)
set(WITH_REGEXPS 0)
set(WITH_SCHEMAS 0)
set(WITH_SCHEMATRON 0)
set(WITH_MODULES 0)
set(WITH_ZLIB 0)
set(WITH_LZMA 0)

add_definitions(-DUNICODE -D_UNICODE)

set(LIBXML2_SOURCES
	buf.c
	c14n.c
	catalog.c
	chvalid.c
	debugXML.c
	dict.c
	DOCBparser.c
	encoding.c
	entities.c
	error.c
	globals.c
	hash.c
	HTMLparser.c
	HTMLtree.c
	legacy.c
	list.c
	nanoftp.c
	nanohttp.c
	parser.c
	parserInternals.c
	pattern.c
	relaxng.c
	SAX.c
	SAX2.c
	schematron.c
	threads.c
	tree.c
	uri.c
	valid.c
	xinclude.c
	xlink.c
	xmlcatalog.c
	xmlIO.c
	xmlmemory.c
	xmlmodule.c
	xmlreader.c
	xmlregexp.c
	xmlsave.c
	xmlschemas.c
	xmlschemastypes.c
	xmlstring.c
	xmlunicode.c
	xmlwriter.c
	xpath.c
	xpointer.c
)

set(LIBXML2_PUBLIC_HEADERS
	include/libxml/c14n.h
	include/libxml/catalog.h
	include/libxml/chvalid.h
	include/libxml/debugXML.h
	include/libxml/dict.h
	include/libxml/DOCBparser.h
	include/libxml/encoding.h
	include/libxml/entities.h
	include/libxml/globals.h
	include/libxml/hash.h
	include/libxml/HTMLparser.h
	include/libxml/HTMLtree.h
	include/libxml/list.h
	include/libxml/Makefile.am
	include/libxml/nanoftp.h
	include/libxml/nanohttp.h
	include/libxml/parser.h
	include/libxml/parserInternals.h
	include/libxml/pattern.h
	include/libxml/relaxng.h
	include/libxml/SAX.h
	include/libxml/SAX2.h
	include/libxml/schemasInternals.h
	include/libxml/schematron.h
	include/libxml/threads.h
	include/libxml/tree.h
	include/libxml/uri.h
	include/libxml/valid.h
	include/libxml/xinclude.h
	include/libxml/xlink.h
	include/libxml/xmlautomata.h
	include/libxml/xmlerror.h
	include/libxml/xmlexports.h
	include/libxml/xmlIO.h
	include/libxml/xmlmemory.h
	include/libxml/xmlmodule.h
	include/libxml/xmlreader.h
	include/libxml/xmlregexp.h
	include/libxml/xmlsave.h
	include/libxml/xmlschemas.h
	include/libxml/xmlschemastypes.h
	include/libxml/xmlstring.h
	include/libxml/xmlunicode.h
	include/libxml/xmlwriter.h
	include/libxml/xpath.h
	include/libxml/xpathInternals.h
	include/libxml/xpointer.h
	include/win32config.h
	include/wsockcompat.h
)
include_directories(include)

set(LIBXML2_PRIVATE_HEADERS
	buf.h
	elfgcchack.h
	enc.h
	libxml.h
	save.h
	testOOMlib.h
	timsort.h
	trio.h
	triodef.h
	trionan.h
	triop.h
	triostr.h
	xzlib.h
)

configure_file(include/libxml/xmlversion.h.in libxml/xmlversion.h @ONLY)
include_directories(${CMAKE_CURRENT_BINARY_DIR})

if(MSVC)
	list(APPEND LIBXML2_PRIVATE_HEADERS win32/VC10/config.h)
	include_directories(win32/VC10)
endif()

add_library(xml2 ${LIBXML2_SOURCES} ${LIBXML2_PRIVATE_HEADERS} ${LIBXML2_PUBLIC_HEADERS} "${CMAKE_CURRENT_BINARY_DIR}/libxml/xmlversion.h")

install(TARGETS xml2
	ARCHIVE DESTINATION lib
	LIBRARY DESTINATION lib
	RUNTIME DESTINATION bin
)

if(MSVC)
	if(BUILD_SHARED_LIBS)
		install(FILES "$<TARGET_PDB_FILE:xml2>" DESTINATION bin)
	endif()
endif()

install(FILES "${CMAKE_CURRENT_BINARY_DIR}/libxml/xmlversion.h" DESTINATION "${CMAKE_INSTALL_PREFIX}/include/libxml")

foreach(header ${LIBXML2_PUBLIC_HEADERS})
	string(REGEX REPLACE "include/?(.*)/[^/]+" "\\1" dir "${header}")
	install(FILES "${header}" DESTINATION "${CMAKE_INSTALL_PREFIX}/include/${dir}")
endforeach()
