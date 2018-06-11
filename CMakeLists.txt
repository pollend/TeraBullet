cmake_minimum_required(VERSION 2.8)
SET(BULLET_VERSION 1.0)

find_package(SWIG REQUIRED)
find_package(Java REQUIRED)
find_package(JNI REQUIRED)
include(UseJava)
include(UseSWIG)
include_directories(${JNI_INCLUDE_DIRS})


SET(SWIG_ROOT ${CMAKE_CURRENT_BINARY_DIR}/swig-src )
SET(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} -fPIC" )
SET(CMAKE_JNI_TARGET TRUE)
SET(CMAKE_JAVA_INCLUDE_PATH lib/guava-25.1-jre.jar lib/slf4j-api-1.7.12.jar lib/TeraMath-1.4.0.jar lib/trove4j-3.0.3.jar)

set_source_files_properties(${CMAKE_CURRENT_BINARY_DIR}/swig-src PROPERTIES GENERATED TRUE)

include_directories(jni/src/bullet)
include_directories(jni/src/custom)
include_directories(jni/src/extras)
include_directories(jni/jni-headers)
include_directories(jni/src/extras/Serialize)

add_subdirectory(jni/src/bullet)
add_subdirectory(jni/src/extras/Serialize)
add_subdirectory(jni/src/custom)


if(WIN32)
    include_directories(jni/jni-headers/win32)
elseif(APPLE)
    include_directories(jni/jni-headers/mac)
elseif(UNIX AND NOT APPLE)
    include_directories(jni/jni-headers/linux)
endif()

SET(UseSWIG_MODULE_VERSION 2)

SET_SOURCE_FILES_PROPERTIES(jni/swig/collision/collision.i  PROPERTIES CPLUSPLUS ON )
SET_SOURCE_FILES_PROPERTIES(jni/swig/linearmath/linearmath.i SWIG_FLAGS  PROPERTIES CPLUSPLUS ON )
SET_SOURCE_FILES_PROPERTIES(jni/swig/dynamics/dynamics.i SWIG_FLAGS  PROPERTIES CPLUSPLUS ON)
SET_SOURCE_FILES_PROPERTIES(jni/swig/softbody/softbody.i SWIG_FLAGS PROPERTIES CPLUSPLUS ON)
SET_SOURCE_FILES_PROPERTIES(jni/swig/extras/extras.i SWIG_FLAGS PROPERTIES CPLUSPLUS ON)

foreach(SWIG_MODULE collision;linearmath;dynamics;softbody;extras)
    SET(CMAKE_SWIG_FLAGS -c++ -package org.terasology.bullet.${SWIG_MODULE} -fvirtual -DBT_NO_PROFILE  )
    SET(SWIG_OUTFILE_DIR ${SWIG_ROOT}/${SWIG_MODULE}/)
    SET(CMAKE_SWIG_OUTDIR ${SWIG_ROOT}/${SWIG_MODULE}/org/terasology/physics/${SWIG_MODULE})
    SWIG_ADD_LIBRARY(BulletSwig${SWIG_MODULE} LANGUAGE java SOURCES jni/swig/${SWIG_MODULE}/${SWIG_MODULE}.i )
    SWIG_LINK_LIBRARIES(BulletSwig${SWIG_MODULE} GdxCustom BulletWorldImporter BulletCollision BulletDynamics BulletSoftBody LinearMath  ${JAVA_LIBRARY})
endforeach()


file(GLOB_RECURSE JAVA_SWIG_SOURCE "${SWIG_ROOT}/*.java")
file(GLOB_RECURSE JAVA_SOURCE "src/*.java")

add_jar(
        TerraBullet
        SOURCES
        ${JAVA_SOURCE}
        ${JAVA_SWIG_SOURCE}

)
add_dependencies(TerraBullet BulletSwigcollision
        BulletSwigdynamics
        BulletSwigsoftbody
        BulletSwigextras)
#ADD_DEPENDENCIES(TerraBullet )


file(GLOB_RECURSE SWIG_HDRS "${SWIG_ROOT}/*.h")
file(GLOB_RECURSE SWIG_SRCS "${SWIG_ROOT}/*.cxx")
add_library(combined SHARED  ${SWIG_HDRS} ${SWIG_SRCS})
set_target_properties(combined PROPERTIES OUTPUT_NAME "TerraBullet")
target_link_libraries(combined GdxCustom LinearMath BulletWorldImporter BulletCollision BulletDynamics BulletSoftBody )