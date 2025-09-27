project('fmt.wasm',['cpp'],version:'10.x',meson_version:'>=1.3.0',default_options:['buildtype=release','cpp_std=c++17'])

if get_option('simd')
  add_project_arguments('-msimd128', language:'cpp')
endif
# Disable C++20 modules path in headers
add_project_arguments('-DFMT_USE_MODULES=0', language:'cpp')

incs = [ include_directories('include'), include_directories('src') ]
core = static_library('fmt_core', [ 'src/format.cc','src/os.cc' ], include_directories: incs)

if get_option('main_module')
  executable('fmt-main','src/fmt_wasm_module.cpp', link_with: core, include_directories: incs, install:true,
    link_args:['--no-entry','-sMODULARIZE=1','-sEXPORT_ES6=1','-sALLOW_MEMORY_GROWTH=1','-sENVIRONMENT=web,webview,worker','-sNO_FILESYSTEM=1','-sEXPORTED_RUNTIME_METHODS=["cwrap","ccall","UTF8ToString"]','-sEXPORTED_FUNCTIONS=@' + meson.project_source_root()/get_option('exports')])
endif
if get_option('side_module')
  shared_module('fmt-side','src/fmt_wasm_side.cpp', link_whole: core, include_directories: incs, name_prefix:'', install:true, link_args:['-sSIDE_MODULE=2','-fPIC'])
endif
