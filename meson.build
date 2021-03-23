project('imgui', 'cpp',
    version: '1.79',
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

if get_option('sdl').enabled()
  sources += 'examples/imgui_impl_sdl.cpp'
endif

message('gflw options is' + get_option('glfw').enabled().to_string())
if get_option('glfw').enabled()
  # hm I wonder where the glfw header comes from since this builds
  message('adding gflw support')
  sources += 'backends/imgui_impl_glfw.cpp'
endif

if get_option('example_glfw_opengl2').enabled()
  sources += 'examples/example_glfw_opengl2/main.cpp'
endif

if get_option('sfml').enabled()
  message('adding sfml support')
  subdir('sfml')
  deps += sfml_imgui_dep
endif

# that's the default value
# but allegro needs int
ImDrawIdx = 'unsigned short'

if get_option('allegro').enabled()
  message('adding allegro support')
  ImDrawIdx = 'int'
  sources += 'examples/imgui_impl_allegro5.cpp'
  cmake = import('cmake')
  allegro_proj = cmake.subproject('allegro')
  # WIP trying to get allegro to work.
  # could not find allegro_primitives.h, and saddly the
  # allegro_proj.include_directories('allegro_primitives') did NOT do the trick

  # Gotta clean this up
  extra_includes += allegro_proj.include_directories('allegro')
  deps += allegro_proj.dependency('allegro_primitives')
endif

sources += 'backends/imgui_impl_opengl2.cpp'

cdata = configuration_data()
cdata.set('IMGUI_TLS', get_option('IMGUI_TLS').enabled())
cdata.set('ImDrawIdx', ImDrawIdx)
imgui_config_h = configure_file(input : 'imconfig.h.meson',
	       output : 'imconfig.h',
	       configuration : cdata)

imgui = library('imgui',
    sources + [imgui_config_h],
    dependencies: [
      # the dependency here just addes the necessary include_directories
      dependency('sdl', required: get_option('sdl').enabled()),
      dependency('glfw', required: get_option('glfw').enabled()),
      dependency('sfml', required: get_option('sfml').enabled()),
      deps,
    ],
    include_directories: [
      include_dirs,
      extra_includes,
      ]
)

imgui_dep = declare_dependency(include_directories: include_dirs, link_with: imgui)
