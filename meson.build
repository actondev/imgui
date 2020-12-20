project('imgui', 'cpp',
    version: '1.79',
    license: 'MIT',
)

# forked cause of the way the SDL events are handled
include_dirs = include_directories('.', 'examples')

sources = ['imgui_demo.cpp', 'imgui_draw.cpp', 'imgui_widgets.cpp', 'imgui.cpp']

if get_option('sdl').enabled()
  sources += 'examples/imgui_impl_sdl.cpp'
endif
sources += 'examples/imgui_impl_opengl2.cpp'

cdata = configuration_data()
cdata.set('IMGUI_TLS', get_option('IMGUI_TLS').enabled())
imgui_config_h = configure_file(input : 'imconfig.h.meson',
	       output : 'imconfig.h',
	       configuration : cdata)

imgui = library('imgui',
    sources + [imgui_config_h],
    dependencies: [
        dependency('sdl', required: get_option('sdl').enabled()),
    ],
    include_directories: include_dirs,
)

imgui_dep = declare_dependency(include_directories: include_dirs, link_with: imgui)
