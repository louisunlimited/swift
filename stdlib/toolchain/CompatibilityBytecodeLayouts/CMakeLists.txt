set(library_name "swiftCompatibilityBytecodeLayouts")

add_swift_target_library("${library_name}" STATIC
  BytecodeLayouts.cpp

  C_COMPILE_FLAGS ${CXX_COMPILE_FLAGS}
  LINK_FLAGS ${CXX_LINK_FLAGS}
  SWIFT_COMPILE_FLAGS ${SWIFT_STANDARD_LIBRARY_SWIFT_FLAGS}
  INSTALL_IN_COMPONENT compiler
  INSTALL_WITH_SHARED)

# FIXME: We need a more flexible mechanism to add lipo targets generated by
# add_swift_target_library to the ALL target. Until then this hack is necessary
# to ensure these libraries build.
foreach(sdk ${SWIFT_SDKS})
  set(target_name "${library_name}-${SWIFT_SDK_${sdk}_LIB_SUBDIR}")
  if(NOT TARGET "${target_name}")
    continue()
  endif()

  set_target_properties("${target_name}"
    PROPERTIES
      EXCLUDE_FROM_ALL FALSE)
endforeach()
