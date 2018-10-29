cmake_minimum_required(VERSION 3.0)

project(fonttest)

add_executable(fonttest
    main.cpp
    font_engine.cpp
    freestack_engine.cpp
    freestack_font.cpp
    freestack_line.cpp
    freestack_path.cpp
    test_harness.cpp
    $<IF:${APPLE},coretext_engine.mm,>
    $<IF:${APPLE},coretext_font.mm,>
    $<IF:${APPLE},coretext_line.mm,>
    $<IF:${APPLE},coretext_path.mm,>
)

set_target_properties(fonttest PROPERTIES
    CXX_STANDARD 11
    CXX_STANDARD_REQUIRED YES
    CXX_EXTENSIONS NO
)

target_include_directories(fonttest
    PRIVATE ..
)

target_compile_definitions(fonttest
    PRIVATE $<IF:${APPLE},HAVE_CORETEXT,>
)

if(APPLE)
  find_library(Foundation Foundation)
  find_library(CoreGraphics CoreGraphics)
  find_library(CoreText CoreText)
endif()

target_link_libraries(fonttest
    freetype harfbuzz raqm
    $<IF:${APPLE},${Foundation},>
    $<IF:${APPLE},${CoreGraphics},>
    $<IF:${APPLE},${CoreText},>
)