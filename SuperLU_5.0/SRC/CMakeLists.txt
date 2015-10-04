
## SuperLU source files
SET(${ARITH_PREFIX}LU_SOURCES
		${ARITH_PREFIX}gssv
		${ARITH_PREFIX}gssvx
		${ARITH_PREFIX}sp_blas2
		${ARITH_PREFIX}sp_blas3
		${ARITH_PREFIX}gscon
		${ARITH_PREFIX}langs
		${ARITH_PREFIX}gsequ
		${ARITH_PREFIX}laqgs
		${ARITH_PREFIX}pivotgrowth
		${ARITH_PREFIX}gsrfs
		${ARITH_PREFIX}gstrf
		${ARITH_PREFIX}gstrs
		${ARITH_PREFIX}copy_to_ucol
		${ARITH_PREFIX}snode_dfs
		${ARITH_PREFIX}snode_bmod
		${ARITH_PREFIX}panel_dfs
		${ARITH_PREFIX}panel_bmod
		${ARITH_PREFIX}readhb
		${ARITH_PREFIX}readrb
		${ARITH_PREFIX}readtriple
		${ARITH_PREFIX}column_dfs
		${ARITH_PREFIX}column_bmod
		${ARITH_PREFIX}pivotL
		${ARITH_PREFIX}pruneL
		${ARITH_PREFIX}memory
		${ARITH_PREFIX}util
		${ARITH_PREFIX}myblas2
		${ARITH_PREFIX}gsisx
		${ARITH_PREFIX}gsitrf
		${ARITH_PREFIX}ldperm
		${ARITH_PREFIX}diagonal
		ilu_${ARITH_PREFIX}drop_row
	    ilu_${ARITH_PREFIX}snode_dfs
        ilu_${ARITH_PREFIX}column_dfs
        ilu_${ARITH_PREFIX}panel_dfs
        ilu_${ARITH_PREFIX}copy_to_ucol
        ilu_${ARITH_PREFIX}pivotL
	)
	

IF (ARITH_PREFIX MATCHES "d")
	LIST(APPEND ${ARITH_PREFIX}LU_SOURCES
		dgstrs)
	MESSAGE(STATUS "${${ARITH_PREFIX}LU_SOURCES}")
ELSEIF (ARITH_PREFIX MATCHES "c")
	LIST(APPEND ${ARITH_PREFIX}LU_SOURCES
		scomplex)
ELSEIF (ARITH_PREFIX MATCHES "z")
	LIST(APPEND ${ARITH_PREFIX}LU_SOURCES
		dcomplex)
ENDIF ()


SET(ALLAUX_SOURCES
		superlu_timer.c
		util.c
		memory.c
		get_perm_c.c
		mmd.c
		sp_coletree.c
		sp_preorder.c
		sp_ienv.c
		relax_snode.c
		heap_relax_snode.c
		colamd.c
		ilu_relax_snode.c
		ilu_heap_relax_snode.c
		mark_relax.c
		mc64ad.c
		qselect.c
		input_error.c
	)
	
SET(SUPERLU_HEADERS
	colamd.h
	html_mainpage.h
	slu_Cnames.h
	slu_${ARITH_PREFIX}defs.h
	slu_util.h
	superlu_enum_consts.h
	supermatrix.h
)

IF (ARITH_PREFIX MATCHES "c")
	SET(SUPERLU_HEADERS ${SUPERLU_HEADERS}
			slu_scomplex.h)
ELSEIF (ARITH_PREFIX MATCHES "z")
	SET(SUPERLU_HEADERS ${SUPERLU_HEADERS}
			slu_dcomplex.h)
ENDIF ()
	
# Lapack sources
SET(${ARITH_PREFIX}LAPACK_SOURCES 
		${ARITH_PREFIX}lacon2
	)
	
IF (ARITH_PREFIX MATCHES "s")
	LIST(APPEND ${ARITH_PREFIX}LAPACK_SOURCES
		# slamch
		smach
		)
ELSEIF (ARITH_PREFIX MATCHES "d")
	LIST(APPEND ${ARITH_PREFIX}LAPACK_SOURCES
		# dlamch
		dmach
		)
ELSEIF (ARITH_PREFIX MATCHES "c")
	LIST(APPEND ${ARITH_PREFIX}LAPACK_SOURCES
		scsum1
		icmax1
		# slamch
		smach
		)
ELSEIF (ARITH_PREFIX MATCHES "z")
	LIST(APPEND ${ARITH_PREFIX}LAPACK_SOURCES
		dzsum1
		izmax1
		# dlamch
		dmach
		)
ENDIF ()

LIST(APPEND ${ARITH_PREFIX}LAPACK_SOURCES
		lsame
		xerbla
	)
	
##

ADD_LIBRARY(libsuperlu STATIC
			${${ARITH_PREFIX}LU_SOURCES}
			${ALLAUX_SOURCES}
			${SUPERLU_HEADERS}
			${${ARITH_PREFIX}LAPACK_SOURCES})
			
SET_TARGET_PROPERTIES(libsuperlu PROPERTIES
						COMPILE_DEFINITIONS "${BLASDEF}"
						LINKER_LANGUAGE C)
						
						
SET_PROPERTY(TARGET libsuperlu PROPERTY ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/lib")
