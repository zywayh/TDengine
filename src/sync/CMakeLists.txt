CMAKE_MINIMUM_REQUIRED(VERSION 2.8)
PROJECT(TDengine)

IF ((TD_LINUX_64) OR (TD_LINUX_32 AND TD_ARM))
  INCLUDE_DIRECTORIES(${TD_COMMUNITY_DIR}/src/inc)
  INCLUDE_DIRECTORIES(${TD_COMMUNITY_DIR}/src/os/inc)
  INCLUDE_DIRECTORIES(${TD_COMMUNITY_DIR}/src/util/inc)
  INCLUDE_DIRECTORIES(${TD_COMMUNITY_DIR}/src/common/inc)
  INCLUDE_DIRECTORIES(${TD_ENTERPRISE_DIR}/src/inc)
  INCLUDE_DIRECTORIES(inc)
  AUX_SOURCE_DIRECTORY(src SRC)
  LIST(REMOVE_ITEM SRC ./src/tarbitrator.c)

  ADD_LIBRARY(sync ${SRC})
  TARGET_LINK_LIBRARIES(sync tutil pthread common)

  LIST(APPEND BIN_SRC ./src/tarbitrator.c)
  LIST(APPEND BIN_SRC ./src/taosTcpPool.c)
  ADD_EXECUTABLE(tarbitrator ${BIN_SRC})
  TARGET_LINK_LIBRARIES(tarbitrator sync common osdetail tutil)

  ADD_SUBDIRECTORY(test)
ENDIF ()
