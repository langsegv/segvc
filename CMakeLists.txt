cmake_minimum_required(VERSION 3.16)

project(segvc
    VERSION 1.0.0
    LANGUAGES C CXX
)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(IS_SHARED "Build shared libraries" ON)

# ----------------------------
# Include Parsr Submodule
# ----------------------------
add_subdirectory(segvp)

# ----------------------------
# Main Executable
# ----------------------------
add_executable(segvc)

# ----------------------------
# Add Source Files
# ----------------------------
file(GLOB_RECURSE SEGVC_SOURCES
    CONFIGURE_DEPENDS
    src/*.c
    src/*.cpp
    src/*.cxx
)

# ----------------------------
# Main Executable
# ----------------------------
target_sources(segvc
    PRIVATE
        ${SEGVC_SOURCES}
)

# Include headers (no need to include parser submodule since it's PUBLIC)
target_include_directories(segvc
    PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}/include
)

# ----------------------------
# Link
# ----------------------------
target_link_libraries(segvc
    PRIVATE
        segvc-parser
)

# ----------------------------
# Warning policy ( Compability between Unix-Like and DOS-like systems )
# ----------------------------
if (MSVC)
    target_compile_options(segvc PRIVATE /W4)
else()
    target_compile_options(segvc PRIVATE -Wall -Wextra -Wpedantic)
endif()
