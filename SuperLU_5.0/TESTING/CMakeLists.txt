ADD_SUBDIRECTORY(MATGEN)


ADD_EXECUTABLE(${ARITH_PREFIX}drive
				${ARITH_PREFIX}drive
				sp_${ARITH_PREFIX}convert
				${ARITH_PREFIX}gst01
				${ARITH_PREFIX}gst02
				${ARITH_PREFIX}gst04
				${ARITH_PREFIX}gst07
			)
			
TARGET_LINK_LIBRARIES(${ARITH_PREFIX}drive
            libblas
            libsuperlu
			libtmglib
			)