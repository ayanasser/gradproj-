set(EXAMPLE_FILES 
    1_user_asynchronous_output.cpp
    2_user_synchronous.cpp
    3_user_asynchronous.cpp)
  
include(${CMAKE_SOURCE_DIR}/cmake/Utils.cmake)

foreach(EXAMPLE_FILE ${EXAMPLE_FILES})

  get_filename_component(SOURCE_NAME ${EXAMPLE_FILE} NAME_WE)
  
  if (UNIX AND NOT APPLE)
    set(EXE_NAME "${SOURCE_NAME}.bin")
  elseif (WIN32)
    set(EXE_NAME "${SOURCE_NAME}")
  endif ()

  message(STATUS "Adding Example ${EXE_NAME}")
  add_executable(${EXE_NAME} ${EXAMPLE_FILE})
  target_link_libraries( ${EXE_NAME} openpose ${GLOG_LIBRARY} ${GFLAGS_LIBRARY} ${Caffe_LIBS} ${MKL_LIBS} ${GLUT_LIBRARY} ${SPINNAKER_LIB} ${CMAKE_THREAD_LIBS_INIT})
  
  if (WIN32)
    set_property(TARGET ${EXE_NAME} PROPERTY FOLDER "Examples/Tutorial/Wrapper")
    configure_file(${CMAKE_SOURCE_DIR}/cmake/OpenPose${CL}.vcxproj.user
        ${CMAKE_CURRENT_BINARY_DIR}/${EXE_NAME}.vcxproj.user @ONLY)
    # Properties->General->Output Directory
    set_property(TARGET ${EXE_NAME} PROPERTY RUNTIME_OUTPUT_DIRECTORY_RELEASE ${PROJECT_BINARY_DIR}/$(Platform)/$(Configuration))
    set_property(TARGET ${EXE_NAME} PROPERTY RUNTIME_OUTPUT_DIRECTORY_DEBUG ${PROJECT_BINARY_DIR}/$(Platform)/$(Configuration))
  endif (WIN32)

endforeach()
