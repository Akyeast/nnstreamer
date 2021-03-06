if get_option('enable-nnfw-runtime')
  filter_sub_nnfw_sources = ['tensor_filter_nnfw.c']

  nnstreamer_filter_nnfw_sources = []
  foreach s : filter_sub_nnfw_sources
    nnstreamer_filter_nnfw_sources += join_paths(meson.current_source_dir(), s)
  endforeach

  nnfw_dep = dependency('nnfw', required: false)
  if not nnfw_dep.found()
    # Until nnfw supports pkg-config, we need to do this primitively.
    nnfw_dep = cc.find_library('nnfw-dev')
  endif
  json_glib_dep = dependency('json-glib-1.0', required: true)
  nnstreamer_filter_nnfw_deps = [glib_dep, gst_dep, nnstreamer_dep, json_glib_dep, nnfw_dep]

  nnfw_plugin_lib = shared_library('nnstreamer_filter_nnfw',
    nnstreamer_filter_nnfw_sources,
    dependencies: nnstreamer_filter_nnfw_deps,
    install: true,
    install_dir: filter_subplugin_install_dir
  )
  static_library('nnstreamer_filter_nnfw',
    nnstreamer_filter_nnfw_sources,
    dependencies: nnstreamer_filter_nnfw_deps,
    install: true,
    install_dir: nnstreamer_libdir
  )

  nnfw_plugin_dep = declare_dependency(link_with: nnfw_plugin_lib,
    dependencies: nnstreamer_filter_nnfw_deps)
endif

if get_option('enable-armnn')
  filter_sub_armnn_sources = [
    'tensor_filter_armnn.cc'
  ]

  nnstreamer_filter_armnn_sources = []
  foreach s : filter_sub_armnn_sources
    nnstreamer_filter_armnn_sources += join_paths(meson.current_source_dir(), s)
  endforeach

  nnstreamer_filter_armnn_deps = [glib_dep, gst_dep, nnstreamer_dep, armnn_dep]

  armnn_plugin_lib = shared_library('nnstreamer_filter_armnn',
    nnstreamer_filter_armnn_sources,
    dependencies: nnstreamer_filter_armnn_deps,
    install: true,
    install_dir: filter_subplugin_install_dir
  )
  static_library('nnstreamer_filter_armnn',
    nnstreamer_filter_armnn_sources,
    dependencies: nnstreamer_filter_armnn_deps,
    install: true,
    install_dir: nnstreamer_libdir
  )

  armnn_plugin_dep = declare_dependency(link_with: armnn_plugin_lib,
    dependencies: nnstreamer_filter_armnn_deps)
endif

if get_option('enable-tensorflow')
  filter_sub_tf_sources = ['tensor_filter_tensorflow.cc']

  nnstreamer_filter_tf_sources = []
  foreach s : filter_sub_tf_sources
    nnstreamer_filter_tf_sources += join_paths(meson.current_source_dir(), s)
  endforeach

  nnstreamer_filter_tf_deps = [tf_dep, protobuf_dep, glib_dep, gst_dep, nnstreamer_dep]

  shared_library('nnstreamer_filter_tensorflow',
    nnstreamer_filter_tf_sources,
    dependencies: nnstreamer_filter_tf_deps,
    install: true,
    install_dir: filter_subplugin_install_dir
  )

  static_library('nnstreamer_filter_tensorflow',
    nnstreamer_filter_tf_sources,
    dependencies: nnstreamer_filter_tf_deps,
    install: true,
    install_dir: nnstreamer_libdir
  )
endif

if get_option('enable-tensorflow-lite')
  filter_sub_tflite_sources = ['tensor_filter_tensorflow_lite.cc']

  nnstreamer_filter_tflite_sources = []
  foreach s : filter_sub_tflite_sources
    nnstreamer_filter_tflite_sources += join_paths(meson.current_source_dir(), s)
  endforeach

  nnstreamer_filter_tflite_deps = [tflite_dep, thread_dep, libdl_dep, glib_dep, gst_dep, nnstreamer_dep]

  if get_option('enable-tflite-nnapi-delegation')
    nnfw_nnapi_lib = dependency('nnfw', required: true)
    if nnfw_nnapi_lib.found()
      nnstreamer_filter_tflite_deps += nnfw_nnapi_lib
    else
      error('Cannot find nnfw')
    endif
  endif

  tflite_compile_args = []

  if cxx.has_type('kTfLiteInt8', prefix : '#include <tensorflow/contrib/lite/model.h>')
    tflite_compile_args += '-DTFLITE_INT8=1'
  endif
  if cxx.has_type('kTfLiteInt16', prefix : '#include <tensorflow/contrib/lite/model.h>')
    tflite_compile_args += '-DTFLITE_INT16=1'
  endif
  if cxx.has_type('kTfLiteFloat16', prefix : '#include <tensorflow/contrib/lite/model.h>')
    tflite_compile_args += '-DTFLITE_FLOAT16=1'
  endif
  if cxx.has_type('kTfLiteComplex64', prefix : '#include <tensorflow/contrib/lite/model.h>')
    tflite_compile_args += '-DTFLITE_COMPLEX64=1'
  endif

  tflite_extra_dep = declare_dependency(
    compile_args : tflite_compile_args,
  )

  nnstreamer_filter_tflite_deps += tflite_extra_dep

  shared_library('nnstreamer_filter_tensorflow-lite',
    nnstreamer_filter_tflite_sources,
    dependencies: nnstreamer_filter_tflite_deps,
    install: true,
    install_dir: filter_subplugin_install_dir
  )

  static_library('nnstreamer_filter_tensorflow-lite',
    nnstreamer_filter_tflite_sources,
    dependencies: nnstreamer_filter_tflite_deps,
    install: true,
    install_dir: nnstreamer_libdir
  )
endif

if get_option('enable-pytorch')
  filter_sub_torch_sources = ['tensor_filter_pytorch.cc']

  nnstreamer_filter_torch_sources = []
  foreach s : filter_sub_torch_sources
    nnstreamer_filter_torch_sources += join_paths(meson.current_source_dir(), s)
  endforeach

  nnstreamer_filter_torch_deps = [torch_dep, glib_dep, gst_dep, nnstreamer_dep]

  shared_library('nnstreamer_filter_pytorch',
    nnstreamer_filter_torch_sources,
    dependencies: nnstreamer_filter_torch_deps,
    install: true,
    install_dir: filter_subplugin_install_dir
  )

  static_library('nnstreamer_filter_pytorch',
    nnstreamer_filter_torch_sources,
    dependencies: nnstreamer_filter_torch_deps,
    install: true,
    install_dir: nnstreamer_libdir
  )
endif

if get_option('enable-caffe2')
  filter_sub_caffe2_sources = ['tensor_filter_caffe2.cc']

  nnstreamer_filter_caffe2_sources = []
  foreach s : filter_sub_caffe2_sources
    nnstreamer_filter_caffe2_sources += join_paths(meson.current_source_dir(), s)
  endforeach

  nnstreamer_filter_caffe2_deps = [caffe2_dep, protobuf_dep, glib_dep, gst_dep, nnstreamer_dep]

  shared_library('nnstreamer_filter_caffe2',
    nnstreamer_filter_caffe2_sources,
    dependencies: nnstreamer_filter_caffe2_deps,
    install: true,
    install_dir: filter_subplugin_install_dir
  )

  static_library('nnstreamer_filter_caffe2',
    nnstreamer_filter_caffe2_sources,
    dependencies: nnstreamer_filter_caffe2_deps,
    install: true,
    install_dir: nnstreamer_libdir
  )
endif

filter_sub_python_sources = ['tensor_filter_python.cc']

nnstreamer_filter_python_sources = []
foreach s : filter_sub_python_sources
  nnstreamer_filter_python_sources += join_paths(meson.current_source_dir(), s)
endforeach

if have_python2
  nnstreamer_filter_python2_deps = [python2_deps, libdl_dep, glib_dep, gst_dep, nnstreamer_dep]

  shared_library('nnstreamer_filter_python2',
    nnstreamer_filter_python_sources,
    dependencies: nnstreamer_filter_python2_deps,
    install: true,
    install_dir: filter_subplugin_install_dir
  )

  static_library('nnstreamer_filter_python2',
    nnstreamer_filter_python_sources,
    dependencies: nnstreamer_filter_python2_deps,
    install: true,
    install_dir: nnstreamer_libdir
  )

  shared_library('nnstreamer_python2',
    'tensor_filter_python_api.c',
    name_prefix: '',
    dependencies: nnstreamer_filter_python2_deps,
    install: true,
    install_dir: filter_subplugin_install_dir
  )
endif

if have_python3
  nnstreamer_filter_python3_deps = [python3_deps, libdl_dep, glib_dep, gst_dep, nnstreamer_dep]

  shared_library('nnstreamer_filter_python3',
    nnstreamer_filter_python_sources,
    dependencies: nnstreamer_filter_python3_deps,
    install: true,
    install_dir: filter_subplugin_install_dir
  )

  static_library('nnstreamer_filter_python3',
    nnstreamer_filter_python_sources,
    dependencies: nnstreamer_filter_python3_deps,
    install: true,
    install_dir: nnstreamer_libdir
  )

  shared_library('nnstreamer_python3',
    'tensor_filter_python_api.c',
    name_prefix: '',
    dependencies: nnstreamer_filter_python3_deps,
    install: true,
    install_dir: filter_subplugin_install_dir
  )
endif

if have_movidius_ncsdk2
  filter_sub_mvncsdk2_sources = [
    'tensor_filter_movidius_ncsdk2.c'
  ]

  nnstreamer_filter_mvncsdk2_sources = []
  foreach s : filter_sub_mvncsdk2_sources
    nnstreamer_filter_mvncsdk2_sources += join_paths(meson.current_source_dir(), s)
  endforeach

  nnstreamer_filter_mvncsdk2_deps = [glib_dep, gst_dep, nnstreamer_dep, nns_mvncsdk2_dep]

  shared_library('nnstreamer_filter_movidius-ncsdk2',
    nnstreamer_filter_mvncsdk2_sources,
    dependencies: nnstreamer_filter_mvncsdk2_deps,
    install: true,
    install_dir: filter_subplugin_install_dir
  )

  static_library('nnstreamer_filter_movidius-ncsdk2',
    nnstreamer_filter_mvncsdk2_sources,
    dependencies: nnstreamer_filter_mvncsdk2_deps,
    install: true,
    install_dir: nnstreamer_libdir
  )
endif

if get_option('enable-cppfilter')
  filter_sub_cpp_sources = ['tensor_filter_cpp.cc']

  nnstreamer_filter_cpp_sources = []
  foreach s : filter_sub_cpp_sources
    nnstreamer_filter_cpp_sources += join_paths(meson.current_source_dir(), s)
  endforeach

  nnstreamer_cpp_lib = shared_library('nnstreamer_filter_cpp',
    nnstreamer_filter_cpp_sources,
    dependencies: [glib_dep, gst_dep, nnstreamer_dep],
    install: true,
    install_dir: filter_subplugin_install_dir
  )
  install_headers(['tensor_filter_cpp.hh'],
    subdir: 'nnstreamer'
  )
  configure_file(input: 'nnstreamer-cpp.pc.in', output: 'nnstreamer-cpp.pc',
    install_dir: join_paths(nnstreamer_libdir, 'pkgconfig'),
    configuration: nnstreamer_install_conf
  )
  nnstreamer_cpp_dep = declare_dependency(link_with: nnstreamer_cpp_lib,
    dependencies: [glib_dep, gst_dep, nnstreamer_dep],
    include_directories: include_directories('.')
  )
endif

if get_option('enable-edgetpu')
  if get_option('enable-tensorflow-lite')
    edgetpu_dep = dependency('edgetpu', required: false)
    if not edgetpu_dep.found()
      # Ubuntu edgetpu package does not have pkgconfig file
      edgetpu_dep = cc.find_library('edgetpu')
    endif
    filter_sub_edgetpu_sources = ['tensor_filter_edgetpu.cc']

    nnstreamer_filter_edgetpu_sources = []
    foreach s : filter_sub_edgetpu_sources
      nnstreamer_filter_edgetpu_sources += join_paths(meson.current_source_dir(), s)
    endforeach

    rpath_edgetpu_test_helper = ''
    if get_option('enable-test')
      '''
      In order to use the test helper library, add build_rpath.
      This temporarily overrides the runtime dependency on edge tpu and
      it is removed after installation.
      '''
      rpath_edgetpu_test_helper = join_paths(meson.build_root(), 'tests/nnstreamer_filter_edgetpu')

    endif
    shared_library('nnstreamer_filter_edgetpu',
      nnstreamer_filter_edgetpu_sources,
      dependencies: [glib_dep, gst_dep, nnstreamer_dep, edgetpu_dep, tflite_dep, libdl_dep],
      install: true,
      install_dir: filter_subplugin_install_dir,
      build_rpath: rpath_edgetpu_test_helper
    )
  else
    error ('enable-tensorflow-lite should be set as \'true\' to build the tensor filter for Edge TPU.')
  endif
endif

if get_option('enable-openvino')
  openvino_deps = []
  openvino_cpp_args = []
  openvino_deps += dependency('openvino', required: true)
  if cxx.has_header('ext_list.hpp')
    openvino_cpuext_lib = cxx.find_library('cpu_extension', required: true)
    if openvino_cpuext_lib.found()
      openvino_cpp_args += '-D__OPENVINO_CPU_EXT__=1'
      openvino_deps += openvino_cpuext_lib
    else
      error('Cannot find the shared object, libcpu_extension.')
    endif
  endif
  filter_sub_openvino_sources = ['tensor_filter_openvino.cc']

  nnstreamer_filter_openvino_sources = []
  foreach s : filter_sub_openvino_sources
    nnstreamer_filter_openvino_sources += join_paths(meson.current_source_dir(), s)
  endforeach

  nnstreamer_filter_openvino_deps = [glib_dep, gst_dep, nnstreamer_dep, openvino_deps]

  nnstreamer_filter_openvino_so_lib = shared_library('nnstreamer_filter_openvino',
    nnstreamer_filter_openvino_sources,
    cpp_args: openvino_cpp_args,
    dependencies: nnstreamer_filter_openvino_deps,
    install: false,
    install_dir: filter_subplugin_install_dir
  )

  static_library('nnstreamer_filter_openvino',
    nnstreamer_filter_openvino_sources,
    cpp_args: openvino_cpp_args,
    dependencies: nnstreamer_filter_openvino_deps,
    install: false,
    install_dir: nnstreamer_libdir
  )

  nnstreamer_filter_openvino_dep = declare_dependency(
    link_with: nnstreamer_filter_openvino_so_lib,
    include_directories: include_directories('.')
  )
endif

if get_option('enable-mediapipe')
  if host_machine.system() == 'linux' and host_machine.cpu_family() == 'x86_64'
    opencv_path = 'bazel-bin/_solib_k8/_U@linux_Uopencv_S_S_Copencv___Uexternal_Slinux_Uopencv_Slib_Sx86_U64-linux-gnu'
  else
    error('Not Supported System & Architecture. Linux x86_64 is required')
  endif
  
  cmd = run_command('sh', '-c', 'echo $MEDIAPIPE_HOME')
  MEDIAPIPE_HOME = cmd.stdout().strip()

  mediapipe_env_exist = run_command('[', '-z', MEDIAPIPE_HOME, ']').returncode()
  mediapipe_dir_exist = run_command('[', '-d', MEDIAPIPE_HOME, ']').returncode()

  if mediapipe_env_exist != 1 and mediapipe_dir_exist != 0
    error('Cannot find MEDIAPIPE_HOME: ' + MEDIAPIPE_HOME)
  endif

  message('MEDIAPIPE_HOME: ' + MEDIAPIPE_HOME)
  mediapipe_incdir = include_directories(
    MEDIAPIPE_HOME,
    join_paths(MEDIAPIPE_HOME, 'bazel-bin'),
    join_paths(MEDIAPIPE_HOME, 'bazel-mediapipe/external/eigen_archive'),
    join_paths(MEDIAPIPE_HOME, 'bazel-mediapipe/external/com_google_absl'),
  )

  cmd = run_command('sh', '-c', meson.source_root() + '/tools/development/gen_mediapipe_libs.sh ' + nnstreamer_libdir)
  if cmd.returncode() != 0
    error(cmd.stderr().strip())
  endif

  mediapipe_internal_dep = cxx.find_library('mediapipe_internal', dirs: nnstreamer_libdir)
  mediapipe_external_dep = cxx.find_library('mediapipe_external', dirs: nnstreamer_libdir)

  opencv_core_dep = cc.find_library('opencv_core', dirs: join_paths(MEDIAPIPE_HOME, opencv_path))
  opencv_video_dep = cc.find_library('opencv_video', dirs: join_paths(MEDIAPIPE_HOME, opencv_path))
  opencv_imgcodecs_dep = cc.find_library('opencv_imgcodecs', dirs: join_paths(MEDIAPIPE_HOME, opencv_path))
  opencv_imgproc_dep = cc.find_library('opencv_imgproc', dirs: join_paths(MEDIAPIPE_HOME, opencv_path))
  opencv_calib3d_dep = cc.find_library('opencv_calib3d', dirs: join_paths(MEDIAPIPE_HOME, opencv_path))
  opencv_highgui_dep = cc.find_library('opencv_highgui', dirs: join_paths(MEDIAPIPE_HOME, opencv_path))
  opencv_videoio_dep = cc.find_library('opencv_videoio', dirs: join_paths(MEDIAPIPE_HOME, opencv_path))
  opencv_features2d_dep = cc.find_library('opencv_features2d', dirs: join_paths(MEDIAPIPE_HOME, opencv_path))
  
  nnstreamer_filter_mediapipe_deps = [
    glib_dep,
    gst_dep,
    nnstreamer_dep,
    opencv_core_dep,
    opencv_video_dep,
    opencv_imgcodecs_dep,
    opencv_imgproc_dep,
    opencv_calib3d_dep,
    opencv_highgui_dep,
    opencv_videoio_dep,
    opencv_features2d_dep,
    mediapipe_external_dep,
    mediapipe_internal_dep
  ]

  filter_sub_mp_sources = ['tensor_filter_mediapipe.cc']

  nnstreamer_filter_mediapipe_sources = []
  foreach s : filter_sub_mp_sources
    nnstreamer_filter_mediapipe_sources += join_paths(meson.current_source_dir(), s)
  endforeach
  
  shared_library('nnstreamer_filter_mediapipe',
    nnstreamer_filter_mediapipe_sources,
    dependencies: nnstreamer_filter_mediapipe_deps,
    include_directories: mediapipe_incdir,
    install: true,
    install_dir: filter_subplugin_install_dir
  )
endif
