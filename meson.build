project('imgui', 'cpp',
    version: '1.85',
    license: 'MIT',
)

# forked cause of the way the SDL events are handled
include_dirs = include_directories('.', 'examples')

sources = files(
  'imgui_demo.cpp',
  'imgui_draw.cpp',
  'imgui_widgets.cpp',
  'imgui.cpp',
  'imgui_tables.cpp',
)
deps = []
extra_includes = []

if get_option('sdl')
  sources += 'backends/imgui_impl_sdl.cpp'
endif

if get_option('glfw')
  message('adding gflw support')
  sources += 'backends/imgui_impl_glfw.cpp'
endif

if get_option('sfml')
  message('adding sfml support')
  subdir('imgui-sfml')
  deps += sfml_imgui_dep
endif

# that's the default value
# but allegro needs int
ImDrawIdx = 'unsigned short'

if get_option('allegro')
  message('adding allegro support')
  ImDrawIdx = 'int'
  sources += 'backends/imgui_impl_allegro5.cpp'
  cmake = import('cmake')
  allegro_proj = cmake.subproject('allegro')
  # WIP trying to get allegro to work.
  # could not find allegro_primitives.h, and saddly the
  # allegro_proj.include_directories('allegro_primitives') did NOT do the trick

  # Gotta clean this up
  extra_includes += allegro_proj.include_directories('allegro')
  deps += allegro_proj.dependency('allegro_primitives')
endif

sources += get_option('opengl') == 2 ? 'backends/imgui_impl_opengl2.cpp' : 'backends/imgui_impl_opengl3.cpp'

# TODO haven't found a way of forcing to use this generated header
# instead of the imconfig.h file present in imgui repo
# so, ended up renaming to imconfig.h.example
cdata = configuration_data()
cdata.set('IMGUI_TLS', get_option('IMGUI_TLS'))
cdata.set('IMGUI_DISABLE_OBSOLETE_FUNCTIONS', get_option('disable_obsolete_functions'))
cdata.set('ImDrawIdx', ImDrawIdx)
imgui_config_h = configure_file(input : 'imconfig.h.meson',
	       output : 'imconfig.h',
	       configuration : cdata)

imgui = library('imgui',
    sources,
    dependencies: [
      # the dependency here just addes the necessary include_directories
      dependency('sdl', required: get_option('sdl')),
      dependency('glfw', required: get_option('glfw')),
      # sfml
      dependency('sfml-window', required: get_option('sfml')),
      dependency('sfml-system', required: get_option('sfml')),
      deps,
    ],
    include_directories: [
      include_dirs,
      extra_includes,
      ]
)

imgui_dep = declare_dependency(
  include_directories: include_dirs,
  link_with: imgui)
