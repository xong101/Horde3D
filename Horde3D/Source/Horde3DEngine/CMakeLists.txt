
# CMAKE_BINARY_DIR is used for inclusion of automatically generated files
include_directories(. ../Shared ../../Bindings/C++ ../../../ ${CMAKE_BINARY_DIR} ${HORDE3D_EXTENSION_INCLUDE_DIRS})


set(HORDE3D_SOURCES 
	egAnimatables.cpp
	egAnimation.cpp
	egCamera.cpp
	egCom.cpp
	egComputeNode.cpp
	egComputeBuffer.cpp
	egExtensions.cpp
	egGeometry.cpp
	egLight.cpp
	egMain.cpp
	egMaterial.cpp
	egModel.cpp
	egModules.cpp
	egParticle.cpp
	egPipeline.cpp
	egPrimitives.cpp
	egRendererBaseGL2.cpp
	egRendererBaseGL4.cpp
	egRenderer.cpp
	egResource.cpp
	egScene.cpp
	egSceneGraphRes.cpp
	egShader.cpp
	egTexture.cpp
	utImage.cpp
	utOpenGL.cpp
	config.h
	egAnimatables.h
	egAnimation.h
	egCamera.h
	egCom.h
	egComputeNode.h
	egComputeBuffer.h
	egExtensions.h
	egGeometry.h
	egLight.h
	egMaterial.h
	egModel.h
	egModules.h
	egParticle.h
	egPipeline.h
	egPrerequisites.h
	egPrimitives.h
	egRenderer.h
	egRendererBase.h
	egRendererBaseGL2.h
	egRendererBaseGL4.h
	egResource.h
	egScene.h
	egSceneGraphRes.h
	egShader.h
	egTexture.h
	utImage.h
	utTimer.h
	utOpenGL.h
        ../Shared/utPlatform.h
	../../Bindings/C++/Horde3D.h

	${HORDE3D_EXTENSION_SOURCES}
	)

add_definitions(-DCMAKE )

add_library(Horde3D SHARED
	${HORDE3D_SOURCES}
	)


if(${CMAKE_SYSTEM_NAME} MATCHES "Windows")
	IF(MSVC)
		set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -DSECURE_SCL=0")		
	ENDIF(MSVC)
	FIND_PACKAGE(OpenGL REQUIRED)
	target_link_libraries(Horde3D ${OPENGL_gl_LIBRARY} ${HORDE3D_EXTENSION_LIBS})
endif(${CMAKE_SYSTEM_NAME} MATCHES "Windows")

if(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
	FIND_PACKAGE(OpenGL REQUIRED)
        FIND_PACKAGE(EGL )
        if( EGL_FOUND )
            add_definitions( -DHAVE_EGL ${EGL_DEFINITIONS} )
        endif( EGL_FOUND )
        target_link_libraries(Horde3D ${OPENGL_gl_LIBRARY} ${EGL_LIBRARIES} ${HORDE3D_EXTENSION_LIBS})
	install(TARGETS Horde3D
		RUNTIME DESTINATION bin
		LIBRARY DESTINATION lib
		ARCHIVE DESTINATION lib
		)
endif(${CMAKE_SYSTEM_NAME} MATCHES "Linux")

if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
	set_target_properties(Horde3D PROPERTIES
		FRAMEWORK TRUE
		PRIVATE_HEADER "egAnimatables.h;egAnimation.h;egCamera.h;egCom.h;egExtensions.h;egGeometry.h;egLight.h;egMaterial.h;egModel.h;egModules.h;egParticle.h;egPipeline.h;egPrerequisites.h;egPrimitives.h;egRenderer.h;egRendererBase.h;egRendererBaseGL2.h;egRendererBaseGL4.h;egResource.h;egScene.h;egSceneGraphRes.h;egShader.h;egTexture.h;utImage.h;utTimer.h;utOpenGL.h;"
		PUBLIC_HEADER "../../Bindings/C++/Horde3D.h")
	
	FIND_LIBRARY(OPENGL_LIBRARY OpenGL)
	FIND_LIBRARY(CORE_FOUNDATION_LIBRARY CoreFoundation)
	target_link_libraries(Horde3D ${OPENGL_LIBRARY} ${HORDE3D_EXTENSION_LIBS} ${CORE_FOUNDATION_LIBRARY})
endif(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
