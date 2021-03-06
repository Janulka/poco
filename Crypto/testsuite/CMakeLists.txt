set(TESTUNIT "${LIBNAME}-testrunner")

# Sources
file(GLOB SRCS_G "src/*.cpp")
POCO_SOURCES_AUTO( TEST_SRCS ${SRCS_G})

# Headers
file(GLOB_RECURSE HDRS_G "src/*.h" )
POCO_HEADERS_AUTO( TEST_SRCS ${HDRS_G})

POCO_SOURCES_AUTO_PLAT( TEST_SRCS WIN32
    src/WinDriver.cpp
)

POCO_SOURCES_AUTO_PLAT( TEST_SRCS WINCE
    src/WinCEDriver.cpp
)

add_executable( ${TESTUNIT} ${TEST_SRCS} )
add_test(NAME ${LIBNAME} WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR} COMMAND ${TESTUNIT} -all)
target_link_libraries( ${TESTUNIT} PocoCrypto PocoNetSSL PocoXML PocoUtil PocoFoundation CppUnit )
if( WIN32)
    #TODO: Is this flag always required?
    add_definitions("-D_AFXDLL")
    target_link_libraries( ${TESTUNIT} WinTestRunner)
else()
    target_link_libraries( ${TESTUNIT} pthread)
endif(WIN32)

