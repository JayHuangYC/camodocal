if(VCHARGE_D3D)
include_directories(../D3D)
endif(VCHARGE_D3D)

if(VCHARGE_VIZ)
if(GLUT_FOUND)
include_directories(
  ${GLUT_INCLUDE_DIR}
)

endif(GLUT_FOUND)
endif(VCHARGE_VIZ)

include_directories(
  ../ceres-solver/include
  ../D3D
  ../dbow2/DBoW2
  ../dbow2/DUtils
  ../dbow2/DUtilsCV
  ../dbow2/DVision
)

set(SRCS
  CameraCalibration.cc
  CameraRigBA.cc
  CamOdoCalibration.cc
  CamOdoThread.cc
  CamOdoWatchdogThread.cc
  CamRigOdoCalibration.cc
  HandEyeCalibration.cc
  PlanarHandEyeCalibration.cc
  StereoCameraCalibration.cc
  utils.cc
)

camodocal_library(camodocal_calib SHARED ${SRCS})

camodocal_link_libraries(camodocal_calib
  ${CAMODOCAL_PLATFORM_UNIX_LIBRARIES}
  ${Boost_FILESYSTEM_LIBRARY}
  ${Boost_SYSTEM_LIBRARY}
  ${Boost_THREAD_LIBRARY}
  ${OPENCV_CORE_LIBRARY}
  ${OPENCV_IMGPROC_LIBRARY}
  ${OPENCV_CALIB3D_LIBRARY}
  camodocal_camera_models
  camodocal_camera_systems
  camodocal_gpl
  camodocal_pose_graph
  camodocal_sparse_graph
  camodocal_visual_odometry
  ceres
  ${CERES_LIBRARIES}
)

if(VCHARGE_D3D)
vcharge_link_libraries(camodocal_calib
  D3D
)
endif(VCHARGE_D3D)

if(VCHARGE_VIZ)
if(GLUT_FOUND)
camodocal_link_libraries(camodocal_calib
  ${CAMODOCAL_PLATFORM_UNIX_LIBRARIES}
  ${GLUT_glut_LIBRARY}
  ${GLUT_Xmu_LIBRARY}
  ${GLUT_Xi_LIBRARY} 
  ${OPENGL_gl_LIBRARY}
  ${OPENGL_glu_LIBRARY}
  overlay
)
endif(GLUT_FOUND)
endif(VCHARGE_VIZ)

camodocal_test(CamOdoCalibration)
camodocal_link_libraries(CamOdoCalibration_test 
  ${CAMODOCAL_PLATFORM_UNIX_LIBRARIES}
  camodocal_calib
)

camodocal_test(HandEyeCalibration)
camodocal_link_libraries(HandEyeCalibration_test 
  ${CAMODOCAL_PLATFORM_UNIX_LIBRARIES}
  camodocal_calib
)

camodocal_test(PlanarHandEyeCalibration)
camodocal_link_libraries(PlanarHandEyeCalibration_test
  ${CAMODOCAL_PLATFORM_UNIX_LIBRARIES}
  camodocal_calib
)
