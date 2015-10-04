CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

PROJECT(SuperLuBuild C Fortran)

#################### Mumps configuration ####################
# Arithmetic configuration
SET(ARITHMETIC "double" CACHE STRING "SuperLU arithmetic")
SET_PROPERTY(CACHE ARITHMETIC PROPERTY STRINGS float double complex complex-double)


IF (ARITHMETIC MATCHES "float")
	SET(ARITH_PREFIX "s")
ELSEIF (ARITHMETIC MATCHES "double")
	SET(ARITH_PREFIX "d")
ELSEIF (ARITHMETIC MATCHES "complex")
	SET(ARITH_PREFIX "c")
ELSEIF (ARITHMETIC MATCHES "complex-double")
	SET(ARITH_PREFIX "z")
ENDIF ()

MESSAGE(STATUS "Arithmetic set to '${ARITHMETIC}'. Added '${ARITH_PREFIX}' prefix...")


SET(SUPERLU_USE_MATLAB FALSE CACHE BOOL "Build Matlab Integration")

SET(CDEFS "Add_")
############

add_subdirectory(CBLAS)
# add_subdirectory(EXAMPLE)
# add_subdirectory(FORTRAN)
add_subdirectory(SRC)
add_subdirectory(TESTING/MATGEN)
# add_subdirectory(TESTING)