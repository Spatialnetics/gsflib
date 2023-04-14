
cmake_minimum_required(VERSION 3.15)

project(gsflib LANGUAGES C)

# Build a shared library by default, for ease of compatibility with the LGPL.
option(BUILD_SHARED_LIBS "Build using shared libraries" ON)

add_library(gsflib)

# Use a glob to find all header and implementation files. Explicitly listing the files
# is typically preferred, but given that the number of files rarely changes upstream,
# this is probably good enough.
file(GLOB sources LIST_DIRECTORIES false source/gsf/*.c source/gsf/*.h)
target_sources(gsflib PRIVATE ${sources})

if(CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    # GSFLib uses ancient, explicitly 64-bit file APIs and needs this define to work
    # on modern GCC build systems.
    target_compile_definitions(gsflib PRIVATE _LARGEFILE64_SOURCE)
endif()

# We don't yet have an install step, but for those using this repo as a submodule,
# we can add the source directory to the include path for anyone linking against
# the lib.
target_include_directories(gsflib SYSTEM INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/source/)

# Add a modern scoped library alias
add_library(Gsf::Gsf ALIAS gsflib)
