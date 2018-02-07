project('cglm', 'c',
  license : 'MIT',
  version : '0.3.3',
  default_options : [
    'c_std=c99',
    'warning_level=3',
  ]
)

cglm_args = []
cglm_sources = []
cglm_headers = []
cglm_includes = []
cglm_deps = []

# Hiding symbols that are not explicitly marked as exported
# requires a compiler flag on all compilers except VS.
cc = meson.get_compiler('c')
if cc.get_id() != 'msvc'
  cglm_args += ['-fvisibility=hidden']
endif

headers_subdir = meson.project_name()

cglm_deps += cc.find_library('m', required : false)

subdir('src')
subdir('include')

cglm_lib = library('cglm', cglm_sources,
  install : true,
  c_args : cglm_args,
  dependencies: cglm_deps,
)

cglm_lib_dep = declare_dependency(
  include_directories: cglm_includes,
  link_with : cglm_lib
)

cglm_inline_dep = declare_dependency(
  include_directories: cglm_includes,
)

# Make this library usable as a Meson subproject.

if get_option('inline_only')
  cglm_dep = cglm_inline_dep
else
  cglm_dep = cglm_lib_dep
endif

pkg_mod = import('pkgconfig')
pkg_mod.generate(
  name : meson.project_name(),
  filebase : meson.project_name(),
  description : 'Optimized OpenGL/Graphics Math (glm) for C.',
  # subdirs : meson.project_name(),
  libraries : [cglm_lib],
  version : meson.project_version(),
)
