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


file(GLOB_RECURSE JAVA_SWIG_SOURCE "${SWIG_ROOT}/*.java")
file(GLOB_RECURSE JAVA_SOURCE "src/*.java")
add_subdirectory(jni)


add_jar(
        TerraBullet
        SOURCES
        ${JAVA_SOURCE}
        ${JAVA_SWIG_SOURCE}
)
ADD_DEPENDENCIES(TerraBullet GdxCustom GdxCustom BulletSwigcollision BulletSwigdynamics BulletSwigsoftbody BulletSwigextras LinearMath BulletWorldImporter BulletCollision BulletDynamics BulletSoftBody  ${JAVA_LIBRARY})


add_library(combined SHARED )
set_target_properties(combined PROPERTIES OUTPUT_NAME "TerraBullet")
target_link_libraries(combined GdxCustom LinearMath BulletWorldImporter BulletCollision BulletDynamics BulletSoftBody )