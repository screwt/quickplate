#-------------------------------------------------------------------
# This file is part of the CMake build system for OGRE
#     (Object-oriented Graphics Rendering Engine)
# For the latest info, see http://www.ogre3d.org/
#
# The contents of this file are placed in the public domain. Feel
# free to make use of it in any way you like.
#-------------------------------------------------------------------

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

