SET(GDX_CUSTOM_SRCS
        gdx/collision/ClosestNotMeConvexResultCallback.cpp
        gdx/collision/ClosestNotMeRayResultCallback.cpp
        gdx/collision/ContactCache.cpp
        gdx/collision/ContactListener.cpp
        gdx/collision/GdxCollisionObjectBridge.cpp
        gdx/common/jniHelpers.cpp
        gdx/dynamics/FilterableVehicleRaycaster.cpp
        gdx/dynamics/InternalTickCallback.cpp
        gdx/linearmath/mathtypes.cpp
)

SET(GDX_CUSTOM_HDRS
        gdx/collision/ClosestNotMeConvexResultCallback.h
        gdx/collision/ClosestNotMeRayResultCallback.h
        gdx/collision/CollisionObjectWrapper.h
        gdx/collision/ContactCache.h
        gdx/collision/ContactListener.h
        gdx/collision/CustomCollisionDispatcher.h
        gdx/collision/GdxCollisionObjectBridge.h
        gdx/common/jniHelpers.h
        gdx/dynamics/InternalTickCallback.h
        gdx/dynamics/FilterableVehicleRaycaster.h
        gdx/dynamics/CustomActionInterface.h
        gdx/linearmath/mathtypes.h
)


ADD_LIBRARY(GdxCustom ${GDX_CUSTOM_SRCS} ${GDX_CUSTOM_HDRS})