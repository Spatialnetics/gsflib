
cmake_minimum_required(VERSION 3.15)

project(gsflib LANGUAGES C)

# Build a shared library by default, for ease of compatibility with the LGPL.
option(BUILD_SHARED_LIBS "Build using shared libraries" ON)

add_library(gsflib)
target_sources(gsflib PRIVATE
    source/gsf/gsf.c
    source/gsf/gsf.h
    source/gsf/gsf_compress.c
    source/gsf/gsf_dec.c
    source/gsf/gsf_dec.h
    source/gsf/gsf_enc.c
    source/gsf/gsf_enc.h
    source/gsf/gsf_ft.h
    source/gsf/gsf_geo.c
    source/gsf/gsf_indx.c
    source/gsf/gsf_indx.h
    source/gsf/gsf_info.c
)

if(CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
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

# We don't yet have an install step, but for those using this repo as a submodule,
# we can add the source directory to the include path for anyone linking against
# the lib.
target_include_directories(gsflib SYSTEM INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/source/)

# Add a modern scoped library alias
add_library(Gsf::Gsf ALIAS gsflib)
