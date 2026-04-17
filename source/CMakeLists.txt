
add_library(gsflib)
target_sources(gsflib PRIVATE
    gsf/gsf.c
    gsf/gsf.h
    gsf/gsf_compress.c
    gsf/gsf_dec.c
    gsf/gsf_dec.h
    gsf/gsf_enc.c
    gsf/gsf_enc.h
    gsf/gsf_ft.h
    gsf/gsf_geo.c
    gsf/gsf_indx.c
    gsf/gsf_indx.h
    gsf/gsf_info.c
)

if(CMAKE_C_COMPILER_ID STREQUAL "GNU")
    # GSFLib uses ancient, explicitly 64-bit file APIs and needs this define to work
    # on modern GCC build systems.
    target_compile_definitions(gsflib PRIVATE _LARGEFILE64_SOURCE)
endif()

if(WIN32)
    # GSFLib isn't exactly at home on Windows and needs some help adapting to the
    # environment.
    target_compile_definitions(gsflib
        PUBLIC
            HAVE_STRUCT_TIMESPEC
        PRIVATE
            __WINDOWS__
    )

    # Link against Winsock2
    find_library(ws2_32_LIBRARY_PATH ws2_32 REQUIRED)
    target_link_libraries(gsflib PRIVATE ${ws2_32_LIBRARY_PATH})

    # Tell CMake to export all symbols and generate a .lib for the .dll.
    #
    # This is a bit of a hack. The "right" thing to do would be to generate
    # an export header and mark only the public API for export.
    set_target_properties(gsflib PROPERTIES WINDOWS_EXPORT_ALL_SYMBOLS ON)
endif()

# Suppress all warnings.
#
# This is an older codebase that does many things that compilers now discourage.
# Let's suppress the hundreds of resulting warnings until someone is interested in
# modernizing the code.
if (MSVC)
    target_compile_options(gsflib PRIVATE /W0)
else()
    target_compile_options(gsflib PRIVATE -w)
endif()

