cmake_minimum_required(VERSION 3.2)

project(echoesphere)

if (NO_X11 OR FORCE_GBM)
    add_compile_options(-std=c++14 -DGLM_FORCE_CXX14 -fpermissive -Wno-psabi -lpthread)
endif()

# The compiled ADA 
add_subdirectory(deps)

file(GLOB ROOT_SOURCE "${PROJECT_SOURCE_DIR}/src/*.cpp")
add_executable(echoesphere
    ${ROOT_SOURCE}
)

set_target_properties(echoesphere PROPERTIES
    CXX_STANDARD 11
    CXX_STANDARD_REQUIRED ON
)

if (EMSCRIPTEN)
    set_target_properties(echoesphere PROPERTIES SUFFIX ".html")

    set(LFLAGS "${LFLAGS} -s USE_GLFW=3")
    set(LFLAGS "${LFLAGS} -s NO_DYNAMIC_EXECUTION=1")
    
    set_target_properties(echoesphere PROPERTIES LINK_FLAGS "${LFLAGS}")
    target_link_libraries(echoesphere PRIVATE ada glfw pthread)
    
else()

    target_link_libraries(echoesphere PRIVATE ada)
endif()


