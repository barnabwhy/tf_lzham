set(PROJECT_NAME liblzham)

################################################################################
# Source groups
################################################################################
set(no_group_source_files
    "../thirdparty/lzham/lzham_api.cpp"
    "../thirdparty/lzham/lzham_assert.cpp"
    "../thirdparty/lzham/lzham_checksum.cpp"
    "../thirdparty/lzham/lzham_huffman_codes.cpp"
    "../thirdparty/lzham/lzham_lzbase.cpp"
    "../thirdparty/lzham/lzham_match_accel.cpp"
    "../thirdparty/lzham/lzham_mem.cpp"
    "../thirdparty/lzham/lzham_platform.cpp"
    "../thirdparty/lzham/lzham_polar_codes.cpp"
    "../thirdparty/lzham/lzham_prefix_coding.cpp"
    "../thirdparty/lzham/lzham_pthreads_threading.cpp"
    "../thirdparty/lzham/lzham_symbol_codec.cpp"
    "../thirdparty/lzham/lzham_timer.cpp"
    "../thirdparty/lzham/lzham_vector.cpp"
    "../thirdparty/lzham/lzham_win32_threading.cpp"
)
source_group("" FILES ${no_group_source_files})

set(include
    "../thirdparty/lzham/include/lzham.h"
    "../thirdparty/lzham/include/lzham_assert.h"
    "../thirdparty/lzham/include/lzham_checksum.h"
    "../thirdparty/lzham/include/lzham_config.h"
    "../thirdparty/lzham/include/lzham_core.h"
    "../thirdparty/lzham/include/lzham_helpers.h"
    "../thirdparty/lzham/include/lzham_huffman_codes.h"
    "../thirdparty/lzham/include/lzham_lzbase.h"
    "../thirdparty/lzham/include/lzham_match_accel.h"
    "../thirdparty/lzham/include/lzham_math.h"
    "../thirdparty/lzham/include/lzham_mem.h"
    "../thirdparty/lzham/include/lzham_null_threading.h"
    "../thirdparty/lzham/include/lzham_platform.h"
    "../thirdparty/lzham/include/lzham_polar_codes.h"
    "../thirdparty/lzham/include/lzham_prefix_coding.h"
    "../thirdparty/lzham/include/lzham_pthreads_threading.h"
    "../thirdparty/lzham/include/lzham_static_lib.h"
    "../thirdparty/lzham/include/lzham_symbol_codec.h"
    "../thirdparty/lzham/include/lzham_threading.h"
    "../thirdparty/lzham/include/lzham_timer.h"
    "../thirdparty/lzham/include/lzham_traits.h"
    "../thirdparty/lzham/include/lzham_types.h"
    "../thirdparty/lzham/include/lzham_utils.h"
    "../thirdparty/lzham/include/lzham_vector.h"
    "../thirdparty/lzham/include/lzham_win32_threading.h"
)
source_group("include" FILES ${include})

set(lzhamcomp
    "../thirdparty/lzham/lzhamcomp/lzham_lzcomp.cpp"
    "../thirdparty/lzham/lzhamcomp/lzham_lzcomp_internal.cpp"
    "../thirdparty/lzham/lzhamcomp/lzham_lzcomp_state.cpp"
)
source_group("lzhamcomp" FILES ${lzhamcomp})

set(lzhamcomp__include
    "../thirdparty/lzham/lzhamcomp/lzham_comp.h"
    "../thirdparty/lzham/lzhamcomp/lzham_lzcomp_internal.h"
)
source_group("lzhamcomp\\include" FILES ${lzhamcomp__include})

set(lzhamdecomp
    "../thirdparty/lzham/lzhamdecomp/lzham_lzdecomp.cpp"
    "../thirdparty/lzham/lzhamdecomp/lzham_lzdecompbase.cpp"
)
source_group("lzhamdecomp" FILES ${lzhamdecomp})

set(lzhamdecomp__include
    "../thirdparty/lzham/lzhamdecomp/lzham_decomp.h"
    "../thirdparty/lzham/lzhamdecomp/lzham_lzdecompbase.h"
)
source_group("lzhamdecomp\\include" FILES ${lzhamdecomp__include})

set(ALL_FILES
    ${no_group_source_files}
    ${include}
    ${lzhamcomp}
    ${lzhamcomp__include}
    ${lzhamdecomp}
    ${lzhamdecomp__include}
)

################################################################################
# Target
################################################################################
add_library(${PROJECT_NAME} STATIC ${ALL_FILES})

use_props(${PROJECT_NAME} "${CMAKE_CONFIGURATION_TYPES}" "${DEFAULT_CXX_PROPS}")
set_target_properties(${PROJECT_NAME} PROPERTIES
    VS_GLOBAL_KEYWORD "Win32Proj"
)
################################################################################
# Target name
################################################################################
set_target_properties(${PROJECT_NAME} PROPERTIES
    TARGET_NAME_DEBUG   "liblzham_x64"
    TARGET_NAME_RELEASE "liblzham_x64"
)
################################################################################
# Output directory
################################################################################
set_target_properties(${PROJECT_NAME} PROPERTIES
    OUTPUT_DIRECTORY_DEBUG   "D:/Documents/GitHub/tf_lzham/lib/Debug/"
    OUTPUT_DIRECTORY_RELEASE "D:/Documents/GitHub/tf_lzham/lib/Release/"
)
set_target_properties(${PROJECT_NAME} PROPERTIES
    INTERPROCEDURAL_OPTIMIZATION_RELEASE "TRUE"
)
################################################################################
# MSVC runtime library
################################################################################
get_property(MSVC_RUNTIME_LIBRARY_DEFAULT TARGET ${PROJECT_NAME} PROPERTY MSVC_RUNTIME_LIBRARY)
string(CONCAT "MSVC_RUNTIME_LIBRARY_STR"
    $<$<CONFIG:Debug>:
        MultiThreadedDebugDLL
    >
    $<$<CONFIG:Release>:
        MultiThreadedDLL
    >
    $<$<NOT:$<OR:$<CONFIG:Debug>,$<CONFIG:Release>>>:${MSVC_RUNTIME_LIBRARY_DEFAULT}>
)
set_target_properties(${PROJECT_NAME} PROPERTIES MSVC_RUNTIME_LIBRARY ${MSVC_RUNTIME_LIBRARY_STR})

################################################################################
# Compile definitions
################################################################################
target_compile_definitions(${PROJECT_NAME} PRIVATE
    "$<$<CONFIG:Debug>:"
        "_DEBUG;"
        "CMAKE_INTDIR=\"Debug\""
    ">"
    "$<$<CONFIG:Release>:"
        "NDEBUG;"
        "CMAKE_INTDIR=\"Release\""
    ">"
    "_CRT_SECURE_NO_WARNINGS;"
    "_CONSOLE;"
    "UNICODE;"
    "_UNICODE"
)

################################################################################
# Compile and link options
################################################################################
if(MSVC)
    target_compile_options(${PROJECT_NAME} PRIVATE
        $<$<CONFIG:Release>:
            /Ob2;
            /Oi;
            /GT;
            /Oy;
            /Gy;
            /Ot
        >
        /permissive-;
        /std:c++17;
        /sdl;
        /W3;
        /Zi;
        ${DEFAULT_CXX_EXCEPTION_HANDLING};
        /Y-
    )
    string(CONCAT FILE_CL_OPTIONS
        "/Y-"
    )
    source_file_compile_options(../thirdparty/lzham/lzham_api.cpp ${FILE_CL_OPTIONS})
    string(CONCAT FILE_CL_OPTIONS
        "/Y-"
    )
    source_file_compile_options(../thirdparty/lzham/lzham_assert.cpp ${FILE_CL_OPTIONS})
    string(CONCAT FILE_CL_OPTIONS
        "/Y-"
    )
    source_file_compile_options(../thirdparty/lzham/lzham_checksum.cpp ${FILE_CL_OPTIONS})
    string(CONCAT FILE_CL_OPTIONS
        "/Y-"
    )
    source_file_compile_options(../thirdparty/lzham/lzham_huffman_codes.cpp ${FILE_CL_OPTIONS})
    string(CONCAT FILE_CL_OPTIONS
        "/Y-"
    )
    source_file_compile_options(../thirdparty/lzham/lzham_lzbase.cpp ${FILE_CL_OPTIONS})
    string(CONCAT FILE_CL_OPTIONS
        "/Y-"
    )
    source_file_compile_options(../thirdparty/lzham/lzham_match_accel.cpp ${FILE_CL_OPTIONS})
    string(CONCAT FILE_CL_OPTIONS
        "/Y-"
    )
    source_file_compile_options(../thirdparty/lzham/lzham_mem.cpp ${FILE_CL_OPTIONS})
    string(CONCAT FILE_CL_OPTIONS
        "/Y-"
    )
    source_file_compile_options(../thirdparty/lzham/lzham_platform.cpp ${FILE_CL_OPTIONS})
    string(CONCAT FILE_CL_OPTIONS
        "/Y-"
    )
    source_file_compile_options(../thirdparty/lzham/lzham_polar_codes.cpp ${FILE_CL_OPTIONS})
    string(CONCAT FILE_CL_OPTIONS
        "/Y-"
    )
    source_file_compile_options(../thirdparty/lzham/lzham_prefix_coding.cpp ${FILE_CL_OPTIONS})
    string(CONCAT FILE_CL_OPTIONS
        "/Y-"
    )
    source_file_compile_options(../thirdparty/lzham/lzham_pthreads_threading.cpp ${FILE_CL_OPTIONS})
    string(CONCAT FILE_CL_OPTIONS
        "/Y-"
    )
    source_file_compile_options(../thirdparty/lzham/lzham_symbol_codec.cpp ${FILE_CL_OPTIONS})
    string(CONCAT FILE_CL_OPTIONS
        "/Y-"
    )
    source_file_compile_options(../thirdparty/lzham/lzham_timer.cpp ${FILE_CL_OPTIONS})
    string(CONCAT FILE_CL_OPTIONS
        "/Y-"
    )
    source_file_compile_options(../thirdparty/lzham/lzham_vector.cpp ${FILE_CL_OPTIONS})
    string(CONCAT FILE_CL_OPTIONS
        "/Y-"
    )
    source_file_compile_options(../thirdparty/lzham/lzham_win32_threading.cpp ${FILE_CL_OPTIONS})
    string(CONCAT FILE_CL_OPTIONS
        "/Y-"
    )
    source_file_compile_options(../thirdparty/lzham/lzhamcomp/lzham_lzcomp.cpp ${FILE_CL_OPTIONS})
    string(CONCAT FILE_CL_OPTIONS
        "/Y-"
    )
    source_file_compile_options(../thirdparty/lzham/lzhamcomp/lzham_lzcomp_internal.cpp ${FILE_CL_OPTIONS})
    string(CONCAT FILE_CL_OPTIONS
        "/Y-"
    )
    source_file_compile_options(../thirdparty/lzham/lzhamcomp/lzham_lzcomp_state.cpp ${FILE_CL_OPTIONS})
    string(CONCAT FILE_CL_OPTIONS
        "/Y-"
    )
    source_file_compile_options(../thirdparty/lzham/lzhamdecomp/lzham_lzdecomp.cpp ${FILE_CL_OPTIONS})
    string(CONCAT FILE_CL_OPTIONS
        "/Y-"
    )
    source_file_compile_options(../thirdparty/lzham/lzhamdecomp/lzham_lzdecompbase.cpp ${FILE_CL_OPTIONS})
endif()

