cmake_minimum_required(VERSION 3.0)
project(xml VERSION 0.1)
# an EXTREMELY pared down version of libxml2, without any of the tests
# without compression, or network crap
# just parsing

add_library(xml
	include/libxml/xmlversion.h
	include/libxml/HTMLtree.h
	config.h
	HTMLtree.c
	globals.c
	SAX2.c
	tree.c # xmlTextConcat
	xmlstring.c # xmlStrncatNew
	buf.c # xmlBufCreate
	parserInternals.c # xmlErrMemory
	parser.c # xmlCtxtReset inputPop
	hash.c # xmlHashFree
	dict.c # xmlDictFree xmlDictSetLimit
	error.c # xmlParserValidityWarning xmlParserValidityError
	threads.c # xmlRMutex*
	xmlIO.c # xmlParserInputBufferCreateFd xmlParserInputBufferCreateIO
	encoding.c # xmlCharEncInput xmlCharEncOutput
	HTMLparser.c # UTF8ToHtml
	chvalid.c # xmlIsPubidChar_tab
	uri.c # xmlURIUnescapeString
	xmlmemory.c # xmlCleanupMemory
	xpath.c # xmlXPathInit
	entities.c # xmlGetPredefinedEntity
	xpointer.c # xmlXPtrWrapLocationSet
	valid.c # xmlGetID xmlFreeDocElementContent
	list.c # xmlListEmpty
	xmlsave.c # xmlNsListDumpOutput
	nanohttp.c # xmlNanoHTTPRedir
	nanoftp.c # etc
	xmlwriter.c)

target_link_libraries(xml PUBLIC m)

target_include_directories(xml PUBLIC
	"${CMAKE_CURRENT_SOURCE_DIR}/include"
	"${CMAKE_CURRENT_BINARY_DIR}"
	"${CMAKE_CURRENT_BINARY_DIR}/include")

add_custom_command(
	OUTPUT
	include/libxml/xmlversion.h
	config.h
	COMMAND ${CMAKE_CURRENT_SOURCE_DIR}/configure
	ARGS
	--without-zlib
	--without-lzma
	--without-modules
	--without-xinclude
	--without-valid
	--without-threads
	--without-schematron
	--without-schemas
	--without-sax1
	--without-regexps
	--without-python
	--without-pattern
	--without-legacy
	--with-ftp
	--with-http
	--without-debug
	--without-catalog
	--disable-ipv6
	--disable-dependency-tracking
	--disable-shared
	--disable-static
	MAIN_DEPENDENCY ${CMAKE_CURRENT_SOURCE_DIR}/configure)

