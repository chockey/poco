set(LIBNAME "PocoUtil")

set(BASE_SRCS
    src/AbstractConfiguration.cpp
    src/Application.cpp
    src/ConfigurationMapper.cpp
    src/ConfigurationView.cpp
    src/FilesystemConfiguration.cpp
    src/HelpFormatter.cpp
    src/IniFileConfiguration.cpp
    src/IntValidator.cpp
    src/JSONConfiguration.cpp
    src/LayeredConfiguration.cpp
    src/LoggingConfigurator.cpp
    src/LoggingSubsystem.cpp
    src/MapConfiguration.cpp
    src/OptionCallback.cpp
    src/Option.cpp
    src/OptionException.cpp
    src/OptionProcessor.cpp
    src/OptionSet.cpp
    src/PropertyFileConfiguration.cpp
    src/RegExpValidator.cpp
    src/ServerApplication.cpp
    src/Subsystem.cpp
    src/SystemConfiguration.cpp
    src/Timer.cpp
    src/TimerTask.cpp
    src/Validator.cpp
    src/XMLConfiguration.cpp
)

set( WIN_SRCS
    src/WinRegistryConfiguration.cpp
    src/WinRegistryKey.cpp
    src/WinService.cpp
)

include_directories( "include" )

if(CMAKE_SYSTEM MATCHES "Windows")
    set(SRCS ${BASE_SRCS} ${WIN_SRCS})
    add_definitions(-DUtil_EXPORTS)
else (CMAKE_SYSTEM MATCHES "Windows")
    set(SRCS ${BASE_SRCS})
endif(CMAKE_SYSTEM MATCHES "Windows")

add_library( ${LIBNAME} ${LIB_MODE} ${SRCS} )
set_target_properties( ${LIBNAME}
    PROPERTIES
    VERSION ${SHARED_LIBRARY_VERSION} SOVERSION ${SHARED_LIBRARY_VERSION} )
target_link_libraries( ${LIBNAME} PocoJSON PocoXML PocoFoundation)

install(
    DIRECTORY include/Poco
    DESTINATION include
    PATTERN ".svn" EXCLUDE
    )

install(
    TARGETS ${LIBNAME}
    LIBRARY DESTINATION lib${LIB_SUFFIX}
    ARCHIVE DESTINATION lib${LIB_SUFFIX}
    RUNTIME DESTINATION bin
    )

if (ENABLE_TESTS)
    add_subdirectory(samples)
    add_subdirectory(testsuite)
endif ()
