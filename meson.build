project('imgui', 'cpp',
    version: '1.77',
    license: 'MIT',
)

# forked cause of the way the SDL events are handled
include_dirs = include_directories('.', 'examples')

sources = ['imgui_demo.cpp', 'imgui_draw.cpp', 'imgui_widgets.cpp', 'imgui.cpp']

sources += 'examples/imgui_impl_sdl.cpp'
sources += 'examples/imgui_impl_opengl2.cpp'

imgui = library('imgui',
    sources,
    dependencies: [
        dependency('sdl', required: true),
    ],
    include_directories: include_dirs,
)

imgui_dep = declare_dependency(include_directories: include_dirs, link_with: imgui)
