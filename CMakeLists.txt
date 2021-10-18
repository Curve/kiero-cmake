cmake_minimum_required(VERSION 3.5)
project(kiero LANGUAGES CXX VERSION 1.2.12)

# --------------------------------------------------------------------------------------------------------
# Library options
# --------------------------------------------------------------------------------------------------------

option(USE_MINHOOK "Whether or not Kiero should use MinHook" OFF)

set(kiero_backend_type D3D9 D3D10 D3D11 D3D12 OpenGL Vulkan) #TODO: Remove None
set(kiero_backend None CACHE STRING "The backend kiero should use.")
set_property(CACHE kiero_backend PROPERTY STRINGS ${kiero_backend_type})

if (NOT kiero_backend IN_LIST kiero_backend_type)
    message(FATAL_ERROR "Kiero backend must be one of ${kiero_backend_type}")
endif()

# --------------------------------------------------------------------------------------------------------
# Fix up Kiero
# --------------------------------------------------------------------------------------------------------

file(READ "kiero/kiero.h" kiero_data)
string(REGEX REPLACE "(#define (KIERO_INCLUDE|KIERO_USE_MINHOOK)[^\n]+)" "" kiero_data "${kiero_data}")
file(WRITE "kiero/kiero.h" "${kiero_data}")

# --------------------------------------------------------------------------------------------------------
# Fix up Kiero
# --------------------------------------------------------------------------------------------------------

add_library(${PROJECT_NAME} STATIC "kiero/kiero.cpp")
target_include_directories(${PROJECT_NAME} PUBLIC "kiero/kiero.h")

# --------------------------------------------------------------------------------------------------------
# Add defines depending on options
# --------------------------------------------------------------------------------------------------------

if (kiero_backend EQUAL D3D9)
    target_compile_definitions(${PROJECT_NAME} PUBLIC KIERO_INCLUDE_D3D9=1)
elseif(kiero_backend EQUAL D3D10)
    target_compile_definitions(${PROJECT_NAME} PUBLIC KIERO_INCLUDE_D3D10=1)
elseif(kiero_backend EQUAL D3D11)
    target_compile_definitions(${PROJECT_NAME} PUBLIC KIERO_INCLUDE_D3D11=1)
elseif(kiero_backend EQUAL D3D12)
    target_compile_definitions(${PROJECT_NAME} PUBLIC KIERO_INCLUDE_D3D12=1)
elseif(kiero_backend EQUAL OpenGL)
    target_compile_definitions(${PROJECT_NAME} PUBLIC KIERO_INCLUDE_OPENGL=1)
elseif(kiero_backend EQUAL Vulkan)
    target_compile_definitions(${PROJECT_NAME} PUBLIC KIERO_INCLUDE_VULKAN=1)
endif()
    
if (USE_MINHOOK)
    find_package(minhook QUIET)

    if (TARGET minhook::minhook)
        message(STATUS "Linking MinHook")
        target_link_libraries(${PROJECT_NAME} PRIVATE minhook::minhook)
    endif()
    
    target_compile_definitions(${PROJECT_NAME} PUBLIC KIERO_USE_MINHOOK=1)
endif()