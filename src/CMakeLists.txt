aux_source_directory( . SRC_DIRS )

add_library( myvo SHARED ${SRC_DIRS} )

target_link_libraries( myvo Sophus::Sophus )
target_link_libraries( myvo ${THIRD_PARTY_LIBS} )