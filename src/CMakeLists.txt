#-------------------------------------------------------------------
# This file is part of the CMake build system for OGRE
#     (Object-oriented Graphics Rendering Engine)
# For the latest info, see http://www.ogre3d.org/
#
# The contents of this file are placed in the public domain. Feel
# free to make use of it in any way you like.
#-------------------------------------------------------------------
SET(CMAKE_INSTALL_RPATH "$ORIGIN")

macro( add_recursive dir retVal )
	file( GLOB_RECURSE ${retVal} ${dir}/*.h ${dir}/*.cpp ${dir}/*.c )
endmacro()

find_package(OGRE REQUIRED)
find_package(SDL2 REQUIRED)

include_directories(
    ${SDL2_INCLUDE_DIR}        
    ${OGRE_INCLUDE_DIRS}/OGRE/
    ${OGRE_INCLUDE_DIRS}/OGRE/Overlay
	${OGRE_INCLUDE_DIR}/Hlms/Unlit
	${OGRE_INCLUDE_DIR}/Hlms/Common
	${OGRE_INCLUDE_DIR}/Hlms/Pbs
)

add_recursive( ./ SOURCE_FILES )

ogre_add_executable(main WIN32 ${SOURCE_FILES})



target_link_libraries(
    main 
    ${OGRE_LIBRARIES} 
    ${OGRE_Overlay_LIBRARIES}
    ${OGRE_HlmsPbs_LIBRARIES}
    ${OGRE_HlmsUnlit_LIBRARIES}
    ${SDL2_LIBRARY}
)

#set(ExternalSharedLibraries )


#message('${OGRE_HlmsPbsMobile_LIBRARY_DBG}')
#message('${OGRE_HlmsPbs_LIBRARY_DBG}')
#message('${OGRE_HlmsUnlitMobile_LIBRARY_DBG}')
#message('${OGRE_HlmsUnlit_LIBRARY_DBG}')
#message('${OGRE_LIBRARY_DBG}')
#message('${OGRE_Overlay_LIBRARY_DBG}')
#message('${OGRE_RenderSystem_GL3Plus_LIBRARY_DBG}')
#message('${SDL2_LIBRARY}')
#message('OGRE_LIB_SUFFIX: ${OGRE_LIBRARY_NAMES} ${OGRE_VERSION}')
#message('SOURCE_DIR: ${CMAKE_SOURCE_DIR}')

message('OGRE_LIBRARIES ${OGRE_LIBRARIES}')

install(TARGETS main DESTINATION "bin/${CMAKE_BUILD_TYPE}")

IF(CMAKE_BUILD_TYPE MATCHES Release)
    install(CODE "
    execute_process(COMMAND 
       cp -L ${OGRE_HlmsPbs_LIBRARY_REL}.${OGRE_VERSION}
             ${CMAKE_INSTALL_PREFIX}/bin/${CMAKE_BUILD_TYPE}/)
    execute_process(COMMAND 
       cp -L ${OGRE_LIBRARY_REL}.${OGRE_VERSION} 
             ${CMAKE_INSTALL_PREFIX}/bin/${CMAKE_BUILD_TYPE}/)
    execute_process(COMMAND 
       cp -L ${OGRE_Overlay_LIBRARY_REL}.${OGRE_VERSION} 
             ${CMAKE_INSTALL_PREFIX}/bin/${CMAKE_BUILD_TYPE}/)
    execute_process(COMMAND 
       cp -L ${OGRE_HlmsUnlit_LIBRARY_REL}.${OGRE_VERSION} 
             ${CMAKE_INSTALL_PREFIX}/bin/${CMAKE_BUILD_TYPE}/)
    execute_process(COMMAND 
       cp -L ${OGRE_RenderSystem_GL3Plus_LIBRARY_REL}.${OGRE_VERSION} 
             ${CMAKE_INSTALL_PREFIX}/bin/${CMAKE_BUILD_TYPE}/)
    execute_process(COMMAND 
       cp  ${CMAKE_SOURCE_DIR}/ogre_utils/plugins.cfg
           ${CMAKE_INSTALL_PREFIX}/bin/${CMAKE_BUILD_TYPE}/)
    execute_process(COMMAND 
       cp  ${CMAKE_SOURCE_DIR}/ogre_utils/resources2.cfg
           ${CMAKE_INSTALL_PREFIX}/bin/${CMAKE_BUILD_TYPE}/)
    ")
elseif(CMAKE_BUILD_TYPE MATCHES Debug)
    install(CODE "
    execute_process(COMMAND 
       cp -L ${OGRE_HlmsPbs_LIBRARY_DBG}.${OGRE_VERSION}
             ${CMAKE_INSTALL_PREFIX}/bin/${CMAKE_BUILD_TYPE}/)
    execute_process(COMMAND 
       cp -L ${OGRE_LIBRARY_DBG}.${OGRE_VERSION} 
             ${CMAKE_INSTALL_PREFIX}/bin/${CMAKE_BUILD_TYPE}/)
    execute_process(COMMAND 
       cp -L ${OGRE_Overlay_LIBRARY_DBG}.${OGRE_VERSION} 
             ${CMAKE_INSTALL_PREFIX}/bin/${CMAKE_BUILD_TYPE}/)
    execute_process(COMMAND 
       cp -L ${OGRE_HlmsUnlit_LIBRARY_DBG}.${OGRE_VERSION} 
             ${CMAKE_INSTALL_PREFIX}/bin/${CMAKE_BUILD_TYPE}/)
    execute_process(COMMAND 
       cp -L ${OGRE_RenderSystem_GL3Plus_LIBRARY_DBG}.${OGRE_VERSION} 
             ${CMAKE_INSTALL_PREFIX}/bin/${CMAKE_BUILD_TYPE}/)
    execute_process(COMMAND 
       cp  ${CMAKE_SOURCE_DIR}/ogre_utils/plugins_d.cfg
           ${CMAKE_INSTALL_PREFIX}/bin/${CMAKE_BUILD_TYPE}/)
    execute_process(COMMAND 
       cp  ${CMAKE_SOURCE_DIR}/ogre_utils/resources2.cfg
           ${CMAKE_INSTALL_PREFIX}/bin/${CMAKE_BUILD_TYPE}/)
    ")
endif()
SET(CMAKE_INSTALL_RPATH "$ORIGIN")
