set(Sources
	POSIX.cpp
	Windows.cpp)
set(PublicHeaders
	${WAVM_INCLUDE_DIR}/Platform/Platform.h)
include_directories(${WAVM_INCLUDE_DIR}/Platform)

add_definitions(-DPLATFORM_API=DLL_EXPORT)

IF(WIN32)
  add_library(Platform SHARED ${Sources} ${PublicHeaders})
ELSE()
  add_library(Platform STATIC ${Sources} ${PublicHeaders})
ENDIF()

# Link with dl on Linux for dladdr.
if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
	target_link_libraries(Platform dl pthread rt)
endif()

install(TARGETS Platform 
   LIBRARY DESTINATION ${CMAKE_INSTALL_FULL_LIBDIR}
   ARCHIVE DESTINATION ${CMAKE_INSTALL_FULL_LIBDIR})

