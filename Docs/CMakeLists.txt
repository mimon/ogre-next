#-------------------------------------------------------------------
# This file is part of the CMake build system for OGRE
#     (Object-oriented Graphics Rendering Engine)
# For the latest info, see http://www.ogre3d.org/
#
# The contents of this file are placed in the public domain. Feel
# free to make use of it in any way you like.
#-------------------------------------------------------------------

###############################################################
# Install documentation
# Build API docs, if doxygen is available
###############################################################

# Set docs target directory
if (WIN32 OR APPLE)
  set(OGRE_DOCS_PATH "Docs")
elseif (UNIX)
  set(OGRE_DOCS_PATH "share/OGRE/docs")
endif ()

# Generate the 'Doxyfile' regardless whether doxygen is available or not.
# This is useful in CI when doxygen might not be available yet in the build step  

if (OGRE_INSTALL_DOCS)
  install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/
  DESTINATION ${OGRE_DOCS_PATH}
  PATTERN ".hg" EXCLUDE
  PATTERN "src" EXCLUDE
  )
endif ()

# RELATIVE will determine relative path from directory to the given file.
file(GLOB MANUAL RELATIVE ${OGRE_SOURCE_DIR}
		${OGRE_SOURCE_DIR}/Docs/src/manual/*.md
		${OGRE_SOURCE_DIR}/Docs/src/SettingUpOgre/*.md
		${OGRE_SOURCE_DIR}/Docs/src/UsingOgre/*.md
    ${OGRE_SOURCE_DIR}/Components
    ${OGRE_SOURCE_DIR}/Plugins
    ${OGRE_SOURCE_DIR}/RenderSystems
    ${OGRE_SOURCE_DIR}/Docs/src/apimainpage.md
    ${OGRE_SOURCE_DIR}/OgreMain
	)

file(RELATIVE_PATH CMAKE_DOXYGEN_OUTPUT_DIRECTORY ${OGRE_SOURCE_DIR} ${OGRE_SOURCE_DIR}/api)
file(RELATIVE_PATH CMAKE_DOXYGEN_DOCS_DIR ${OGRE_SOURCE_DIR} ${OGRE_SOURCE_DIR}/Docs)
  
set(CMAKE_DOXYGEN_INPUT_LIST 
    \"${MANUAL}\")
string(REPLACE ";" "\" \\\n\"" CMAKE_DOXYGEN_INPUT_LIST "${CMAKE_DOXYGEN_INPUT_LIST}")
configure_file(${OGRE_TEMPLATES_DIR}/Doxyfile.in ${OGRE_BINARY_DIR}/Doxyfile)
# Build and install API documentation if doxygen is available
if(DOXYGEN_FOUND)
  add_custom_target(OgreDoc COMMAND 
      ${DOXYGEN_EXECUTABLE} ${OGRE_BINARY_DIR}/Doxyfile
      DEPENDS ${OGRE_BINARY_DIR}/Doxyfile
      WORKING_DIRECTORY ${OGRE_SOURCE_DIR}/Docs/src/ 
      COMMENT "Building documentation" VERBATIM)
file(MAKE_DIRECTORY ${OGRE_BINARY_DIR}/api/)
	if (OGRE_INSTALL_DOCS)
		install(DIRECTORY ${OGRE_BINARY_DIR}/api DESTINATION ${OGRE_DOCS_PATH})
	endif()
endif()
