#BLAS1 sources

SET(SUPERLU_DEFAULT_BLAS TRUE CACHE BOOL "Use SuperLU default BLAS library")

IF (SUPERLU_DEFAULT_BLAS)

SET(${ARITH_PREFIX}BLAS1
		i${ARITH_PREFIX}amax
		${ARITH_PREFIX}axpy
		${ARITH_PREFIX}copy
		${ARITH_PREFIX}scal
		${ARITH_PREFIX}swap
	)
	
IF (${ARITH_PREFIX} MATCHES "s" OR ${ARITH_PREFIX} MATCHES "d")	
	SET(${ARITH_PREFIX}BLAS1 ${${ARITH_PREFIX}BLAS1}
			${ARITH_PREFIX}dot
			${ARITH_PREFIX}rot
			${ARITH_PREFIX}asum
			${ARITH_PREFIX}nrm2
		)
ENDIF ()

IF (${ARITH_PREFIX} MATCHES "c")	
	SET(${ARITH_PREFIX}BLAS1 ${${ARITH_PREFIX}BLAS1}
			scasum
			scnrm2
		)
ENDIF ()

IF (${ARITH_PREFIX} MATCHES "z")	
	SET(${ARITH_PREFIX}BLAS1 ${${ARITH_PREFIX}BLAS1}
			dzasum
			dznrm2
			dcabs1
		)
ENDIF ()

# BLAS2 sources

IF (${ARITH_PREFIX} MATCHES "s" OR ${ARITH_PREFIX} MATCHES "d")	
	SET(${ARITH_PREFIX}BLAS2
			${ARITH_PREFIX}gemv
			${ARITH_PREFIX}symv
			${ARITH_PREFIX}trsv
			${ARITH_PREFIX}ger
			${ARITH_PREFIX}syr2
		)
ENDIF ()

IF (${ARITH_PREFIX} MATCHES "c" OR ${ARITH_PREFIX} MATCHES "z")	
	SET(${ARITH_PREFIX}BLAS2
			${ARITH_PREFIX}gemv
			${ARITH_PREFIX}hemv
			${ARITH_PREFIX}trsv
			${ARITH_PREFIX}gerc
			${ARITH_PREFIX}her2
		)
ENDIF()



# BLAS_HEADERS
SET (BLAS_HEADERS
			f2c
			slu_Cnames
			superlu_f2c)


ADD_LIBRARY(libblas STATIC
			${${ARITH_PREFIX}BLAS2}
			${${ARITH_PREFIX}BLAS1}
			${BLAS_HEADERS})
			
SET_TARGET_PROPERTIES(libblas PROPERTIES
						LINKER_LANGUAGE C)

TARGET_INCLUDE_DIRECTORIES(libblas PUBLIC
						"${CMAKE_CURRENT_SOURCE_DIR}/../src"
					)
						
ELSE (SUPERLU_DEFAULT_BLAS)
	SET(BLASDEF "USE_VENDOR_BLAS" PARENT_SCOPE)
	SET(USE_MKL_BLAS TRUE CACHE BOOL "Select if BLAS libraries should be used" FORCE)
	IF (USE_MKL_BLAS)
	
		SET(BLAS_INCLUDE
		"C:/Program Files (x86)/Intel/Composer XE 2015/mkl/include"
		"C:/Program Files (x86)/Intel/Composer XE 2015/compiler/include"
		)
		SET(libblas
				"C:/Program Files (x86)/Intel/Composer XE 2015/mkl/lib/intel64/mkl_intel_lp64.lib"
				"C:/Program Files (x86)/Intel/Composer XE 2015/mkl/lib/intel64/mkl_intel_thread.lib"
				"C:/Program Files (x86)/Intel/Composer XE 2015/mkl/lib/intel64/mkl_core.lib"
				"C:/Program Files (x86)/Intel/Composer XE 2015/compiler/lib/intel64/libiomp5md.lib")
	ENDIF(USE_MKL_BLAS)

ENDIF(SUPERLU_DEFAULT_BLAS)