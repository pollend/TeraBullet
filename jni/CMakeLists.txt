include_directories(src/bullet)
include_directories(src/custom)
include_directories(src/extras)
include_directories(jni-headers)
include_directories(src/extras/Serialize)

add_subdirectory(src/bullet)
add_subdirectory(src/extras/Serialize)


if(WIN32)
    include_directories(jni-headers/win32)
elseif(APPLE)
    include_directories(jni-headers/mac)
elseif(UNIX AND NOT APPLE)
    include_directories(jni-headers/linux)
endif()

SET(UseSWIG_MODULE_VERSION 2)

SET_SOURCE_FILES_PROPERTIES(swig/collision/collision.i  PROPERTIES CPLUSPLUS ON )
SET_SOURCE_FILES_PROPERTIES(swig/linearmath/linearmath.i SWIG_FLAGS  PROPERTIES CPLUSPLUS ON )
SET_SOURCE_FILES_PROPERTIES(swig/dynamics/dynamics.i SWIG_FLAGS  PROPERTIES CPLUSPLUS ON)
SET_SOURCE_FILES_PROPERTIES(swig/softbody/softbody.i SWIG_FLAGS PROPERTIES CPLUSPLUS ON)
SET_SOURCE_FILES_PROPERTIES(swig/extras/extras.i SWIG_FLAGS PROPERTIES CPLUSPLUS ON)

foreach(SWIG_MODULE collision;linearmath;dynamics;softbody;extras)
SET(CMAKE_SWIG_FLAGS -c++ -package org.terasology.bullet.${SWIG_MODULE} -fvirtual -DBT_NO_PROFILE  )
SET(SWIG_OUTFILE_DIR ${SWIG_ROOT}/${SWIG_MODULE}/)
SET(CMAKE_SWIG_OUTDIR ${SWIG_ROOT}/${SWIG_MODULE}/org/terasology/physics/${SWIG_MODULE})
SWIG_ADD_LIBRARY(BulletSwig${SWIG_MODULE} LANGUAGE java SOURCES swig/${SWIG_MODULE}/${SWIG_MODULE}.i )
SWIG_LINK_LIBRARIES(BulletSwig${SWIG_MODULE} BulletWorldImporter BulletCollision BulletDynamics BulletSoftBody LinearMath  ${JAVA_LIBRARY})
endforeach()
#
#SET(CMAKE_SWIG_FLAGS -c++ -package org.terasology.bullet.linearmath )
#SET(SWIG_OUTFILE_DIR ${SWIG_ROOT}/collision/)
#SET(CMAKE_SWIG_OUTDIR ${SWIG_ROOT}/collision/org/terasology/physics/collision)
#SWIG_ADD_LIBRARY(BulletSwigLinearmath LANGUAGE java OUTFILE_DIR linearmath/org/terasology/bullet/linearmath SOURCES swig/linearmath/linearmath.i)
#
#SET(CMAKE_SWIG_FLAGS -c++ -package org.terasology.bullet.dynamics )
#SET(SWIG_OUTFILE_DIR ${SWIG_ROOT}/collision/)
#SET(CMAKE_SWIG_OUTDIR ${SWIG_ROOT}/collision/org/terasology/physics/collision)
#SWIG_ADD_LIBRARY(BulletSwigDynamics LANGUAGE java OUTFILE_DIR dynamics/org/terasology/bullet/dynamics SOURCES swig/dynamics/dynamics.i )
#
#SET(CMAKE_SWIG_FLAGS -c++ -package org.terasology.bullet.softbody )
#SET(SWIG_OUTFILE_DIR ${SWIG_ROOT}/collision/)
#SET(CMAKE_SWIG_OUTDIR ${SWIG_ROOT}/collision/org/terasology/physics/collision)
#SWIG_ADD_LIBRARY(BulletSwigSoftbody LANGUAGE java OUTFILE_DIR softbody/org/terasology/bullet/softbody SOURCES swig/softbody/softbody.i )
#
#SET(CMAKE_SWIG_FLAGS -c++ -package org.terasology.bullet.extras )
#SET(SWIG_OUTFILE_DIR ${SWIG_ROOT}/collision/)
#SET(CMAKE_SWIG_OUTDIR ${SWIG_ROOT}/collision/org/terasology/physics/collision)
#SWIG_ADD_LIBRARY(BulletSwigExtras LANGUAGE java OUTFILE_DIR extras/org/terasology/bullet/extras SOURCES swig/extras/extras.i)



#SWIG_LINK_LIBRARIES(BulletSwigLinearmath BulletWorldImporter BulletCollision BulletDynamics BulletSoftBody LinearMath ${JAVA_LIBRARY})
#SWIG_LINK_LIBRARIES(BulletSwigDynamics BulletWorldImporter BulletCollision BulletDynamics BulletSoftBody LinearMath ${JAVA_LIBRARY})
#SWIG_LINK_LIBRARIES(BulletSwigSoftbody BulletWorldImporter BulletCollision BulletDynamics BulletSoftBody LinearMath ${JAVA_LIBRARY})
#SWIG_LINK_LIBRARIES(BulletSwigExtras BulletWorldImporter BulletCollision BulletDynamics BulletSoftBody LinearMath ${JAVA_LIBRARY})
#
#


