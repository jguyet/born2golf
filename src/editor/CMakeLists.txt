set(EDITOR_LIBRARIES
    common
    cimgui
    fast_obj
    glad
    lightmapper
    mattiasgustavsson_libs
    miniz
    parson
    sokol
    stb
    xatlas)

if(CMAKE_SYSTEM_NAME STREQUAL Linux)
  set(EDITOR_LIBRARIES 
      ${EDITOR_LIBRARIES}
      -lX11
      -lXcursor
      -lXi
      -lGL
      -ldl
      -lpthread
      -lm
      -lasound)
endif()

if(CMAKE_SYSTEM_NAME STREQUAL Darwin)
  set(CMAKE_EXE_LINKER_FLAGS
    "\
    -framework AudioToolbox\
    -framework VideoToolbox\
    -framework CoreGraphics\
    -framework AppKit\
    -framework AppIntents\
    -framework CoreFoundation\
    -framework CoreServices\
    -framework CoreMedia\
    -framework CoreVideo\
    -framework IOKit\
    -I/opt/X11/include\
    -L/opt/X11/lib\
    -L/usr/local/lib\
    -I/usr/local/include\
    -lglfw\
    -framework OpenGL\
    "
  )
endif()

add_executable(editor
    draw.c
    editor.c
    gi.c
    gizmo.c
    main.c)
target_link_libraries(editor PRIVATE ${EDITOR_LIBRARIES})

if(CMAKE_SYSTEM_NAME STREQUAL Windows)
  target_compile_options(editor PRIVATE /W3)
else()
  target_compile_options(editor PRIVATE -L/usr/local/Cellar/glfw/3.3.8/lib -I/usr/local/Cellar/glfw/3.3.8/include -lglfw)
  #  -Wall -Wextra -Wpedantic)
endif()
