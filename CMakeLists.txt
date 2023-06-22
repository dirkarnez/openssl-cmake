cmake_minimum_required(VERSION 3.0)

set(GLM_VERSION 3.0.5)

project(openssl VERSION 3.0.5 LANGUAGES CXX)

# add_library(openssl INTERFACE)

# add_library(protobuf::libprotobuf ALIAS libprotobuf)
# add_library(protobuf::libprotobuf-lite ALIAS libprotobuf-lite)


include(GNUInstallDirs)



# # Set the target properties to include header files and link to the prebuilt library file
# set_target_properties(openssl_ssl_static PROPERTIES
#     IMPORTED_LOCATION 
#     IMPORTED_LINK_INTERFACE_LANGUAGES "C"
# )

# target_link_libraries(openssl_static PUBLIC 
#     "lib64/libssl.a"
# )

# add_library(openssl::SSL ALIAS openssl_ssl_static)


add_library(OpenSSL::Crypto STATIC IMPORTED)
target_include_directories(OpenSSL::Crypto INTERFACE
	$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}>
	$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

set_target_properties(OpenSSL::Crypto PROPERTIES
                    INTERFACE_INCLUDE_DIRECTORIES include/)       
set_target_properties(OpenSSL::Crypto PROPERTIES
                    IMPORTED_LINK_INTERFACE_LANGUAGES "C"
                    IMPORTED_LOCATION ${CMAKE_CURRENT_SOURCE_DIR}/lib64/libcrypto.a)
                    
set_property( TARGET OpenSSL::Crypto APPEND PROPERTY INTERFACE_LINK_LIBRARIES ws2_32 )
set_property( TARGET OpenSSL::Crypto APPEND PROPERTY INTERFACE_LINK_LIBRARIES crypt32 )


install(TARGETS OpenSSL::Crypto  EXPORT OpenSSL::Crypto)

include(CPack)
install(DIRECTORY include/ DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
install(DIRECTORY lib64/ DESTINATION ${CMAKE_INSTALL_LIBDIR})
install(EXPORT openssl FILE opensslConfig.cmake DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/openssl NAMESPACE openssl::)
include(CMakePackageConfigHelpers)
write_basic_package_version_file("opensslConfigVersion.cmake" COMPATIBILITY AnyNewerVersion)
install(FILES ${CMAKE_CURRENT_BINARY_DIR}/opensslConfigVersion.cmake DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/openssl)










# # Create a library target for the prebuilt static library
# add_library(openssl STATIC IMPORTED)

# include(GNUInstallDirs)




# # install(EXPORT ${PROJECT_NAME} FILE ${PROJECT_NAME}Config.cmake DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME} NAMESPACE ${PROJECT_NAME}::)

# # install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION "${CMAKE_INSTALL_INCLUDEDIR}")




