#-------------------------------------------------------------------
# This file is part of the CMake build system for OGRE
#     (Object-oriented Graphics Rendering Engine)
# For the latest info, see http://www.ogre3d.org/
#
# The contents of this file are placed in the public domain. Feel
# free to make use of it in any way you like.
#-------------------------------------------------------------------

# Configure Ogre HLMS User Interface (2D) for OpenGL ES 2.0 build

PROJECT(OgreHlmsUnlitMobile)

file(GLOB HEADER_FILES "${CMAKE_CURRENT_SOURCE_DIR}/include/*.h")
file(GLOB SOURCE_FILES "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp")

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)

ogre_add_library(OgreHlmsUnlitMobile ${OGRE_COMP_LIB_TYPE} ${HEADER_FILES} ${SOURCE_FILES})
set_target_properties(OgreHlmsUnlitMobile PROPERTIES VERSION ${OGRE_SOVERSION} SOVERSION ${OGRE_SOVERSION})
target_link_libraries(OgreHlmsUnlitMobile OgreMain)

ogre_config_framework(OgreHlmsUnlitMobile)
ogre_config_component(OgreHlmsUnlitMobile)

install(FILES ${HEADER_FILES}
  DESTINATION include/OGRE/Hlms/UnlitMobile
)
