###########################################################################
#   Copyright (C) 2014 by Jerry			                                  #
#                                                                         #
#   This file is part of IBrowser.                                        #
#                                                                         #
#   IBrowser is private software.                                         #
#   program.  If not, see <https://github.com/jerrykk/IBrowser>.          #
#                                                                         #
#   IBrowser website: https://github.com/jerrykk/IBrowser                 #
###########################################################################


#############################################################################
#
# IBrowser binary
#
#############################################################################
set(IBRMP_SRCS
	subprocess.cpp
   )
   
include_directories(${IBROWSER_SOURCE_DIR}/include
					${IBROWSER_SOURCE_DIR}/include/cef
					${IBROWSER_SOURCE_DIR}/include/utils
					${IBROWSER_SOURCE_DIR}/src
					${IBROWSER_SOURCE_DIR}/src/ibrowser)

add_definitions(-DCURL_STATICLIB)

IF (WIN32)
add_executable(subprocess 	WIN32 	${IBRMP_SRCS})
TARGET_LINK_LIBRARIES(subprocess
	general
	#utils
	kernel32.lib
	user32.lib
	shell32.lib
	uuid.lib
	ole32.lib
	advapi32.lib
	ws2_32.lib
	gdi32.lib
	comdlg32.lib
	oleaut32.lib
	imm32.lib
	winmm.lib
	winspool.lib
	qtmaind.lib
	QtCored.lib
	QtGuid.lib
	${IBROWSER_SOURCE_DIR}/lib/libcef.lib
	debug
	${IBROWSER_SOURCE_DIR}/lib/libcef_dll_wrapper_debug.lib
	optimized
	${IBROWSER_SOURCE_DIR}/lib/libcef_dll_wrapper_release.lib
)

ADD_CUSTOM_COMMAND(
    TARGET subprocess
    POST_BUILD
    COMMAND cmake -E copy_directory ${IBROWSER_SOURCE_DIR}/Resources/
$<TARGET_FILE_DIR:subprocess>
	COMMAND cmake -E copy_directory ${IBROWSER_SOURCE_DIR}/dll/$<CONFIGURATION>/
$<TARGET_FILE_DIR:subprocess>
)

#set_target_properties(ibrowser PROPERTIES LINK_FLAGS "/SUBSYSTEM:WINDOWS")
SET(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} /subsystem:windows") 
ELSE(WIN32)
	add_executable(subprocess 	${IBRMP_SRCS})
#Message(STATUS "OPENCL_LIBRARIES : ${OPENCL_INCLUDE_DIR}")
	TARGET_LINK_LIBRARIES(subprocess)
ENDIF(WIN32)
