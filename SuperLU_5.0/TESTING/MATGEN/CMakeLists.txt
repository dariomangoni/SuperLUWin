
SET(${ARITH_PREFIX}ATGEN
		${ARITH_PREFIX}latm1
        ${ARITH_PREFIX}laran
        ${ARITH_PREFIX}larnd
        ${ARITH_PREFIX}laruv
        ${ARITH_PREFIX}labad
	    ${ARITH_PREFIX}larnv
		lsamen
	)
	
SET(${ARITH_PREFIX}LAPACK_SOURCES
		${ARITH_PREFIX}latb4
        ${ARITH_PREFIX}laset
        ${ARITH_PREFIX}lartg
	)
	
SET(${ARITH_PREFIX}MATGEN
		${ARITH_PREFIX}latms
		${ARITH_PREFIX}latme
		${ARITH_PREFIX}latmr
		${ARITH_PREFIX}lagge
		${ARITH_PREFIX}lagsy
		${ARITH_PREFIX}large
		${ARITH_PREFIX}laror
		${ARITH_PREFIX}larot
		${ARITH_PREFIX}latm2
		${ARITH_PREFIX}latm3
	)	


IF (ARITH_PREFIX MATCHES "c" OR ARITH_PREFIX MATCHES "z")
	LIST(APPEND ${ARITH_PREFIX}LAPACK_SOURCES
		${ARITH_PREFIX}larnv
		${ARITH_PREFIX}lacgv
		${ARITH_PREFIX}symv
	)
	LIST(APPEND ${ARITH_PREFIX}MATGEN
		${ARITH_PREFIX}laghe
		${ARITH_PREFIX}larnd
		${ARITH_PREFIX}dotc
	)
ENDIF ()

IF (ARITH_PREFIX MATCHES "s" OR ARITH_PREFIX MATCHES "c")
	SET(${ARITH_PREFIX}INTRINSIC
		r_lg10
	    r_sign
	    pow_dd
	    pow_ri
	)
ELSEIF (ARITH_PREFIX MATCHES "d" OR ARITH_PREFIX MATCHES "z")
ENDIF ()
	SET(${ARITH_PREFIX}INTRINSIC
		d_lg10
	    d_sign
        pow_dd
        pow_di
	)
	
	
	
ADD_LIBRARY(libtmglib STATIC
				${${ARITH_PREFIX}ATGEN}
				${${ARITH_PREFIX}LAPACK_SOURCES}
				${${ARITH_PREFIX}MATGEN}
				${${ARITH_PREFIX}INTRINSIC}
			)
			
SET_PROPERTY(TARGET libtmglib PROPERTY ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_SOURCE_DIR}/lib")
