# Note: The buildbots evaluate this file with CWD set to the parent
# directory and assume that the root of the checkout is in ./v8/, so
# all paths in here must match this assumption.

vars = {
  'checkout_instrumented_libraries': False,
  'chromium_url': 'https://chromium.googlesource.com',
  'build_for_node': False,
}

deps = {
  'v8/build':
    Var('chromium_url') + '/chromium/src/build.git' + '@' + '1ce2f5d198826a4fb8f4b1c6b91c50163bb9127a',
  'v8/tools/gyp':
    Var('chromium_url') + '/external/gyp.git' + '@' + 'd61a9397e668fa9843c4aa7da9e79460fe590bfb',
  'v8/third_party/icu':
    Var('chromium_url') + '/chromium/deps/icu.git' + '@' + 'f3d25bcc2e542a10248d5a272219ed7695fe3252',
  'v8/third_party/instrumented_libraries':
    Var('chromium_url') + '/chromium/src/third_party/instrumented_libraries.git' + '@' + 'b7578b4132cf73ca3265e2ee0b7bd0a422a54ebf',
  'v8/buildtools':
    Var('chromium_url') + '/chromium/buildtools.git' + '@' + '1be57dc4c2c68fa564ee258a3af0cf8003007edf',
  'v8/base/trace_event/common':
    Var('chromium_url') + '/chromium/src/base/trace_event/common.git' + '@' + '0e9a47d74970bee1bbfc063c47215406f8918699',
  'v8/third_party/android_tools': {
    'url': Var('chromium_url') + '/android_tools.git' + '@' + '7d781b3544ef67dfa8c7c0c1a347b818c49c42bc',
    'condition': 'checkout_android',
  },
  'v8/third_party/catapult': {
    'url': Var('chromium_url') + '/catapult.git' + '@' + 'e1f9b2c0b5761fd51e665b82f50ae96d9ded2cf3',
    'condition': 'checkout_android',
  },
  'v8/third_party/colorama/src': {
    'url': Var('chromium_url') + '/external/colorama.git' + '@' + '799604a1041e9b3bc5d2789ecbd7e8db2e18e6b8',
    'condition': 'checkout_android',
  },
  'v8/third_party/jinja2':
    Var('chromium_url') + '/chromium/src/third_party/jinja2.git' + '@' + 'd34383206fa42d52faa10bb9931d6d538f3a57e0',
  'v8/third_party/markupsafe':
    Var('chromium_url') + '/chromium/src/third_party/markupsafe.git' + '@' + '8f45f5cfa0009d2a70589bcda0349b8cb2b72783',
  'v8/tools/swarming_client':
    Var('chromium_url') + '/infra/luci/client-py.git' + '@' + '4bd9152f8a975d57c972c071dfb4ddf668e02200',
  'v8/testing/gtest':
    Var('chromium_url') + '/external/github.com/google/googletest.git' + '@' + '6f8a66431cb592dad629028a50b3dd418a408c87',
  'v8/testing/gmock':
    Var('chromium_url') + '/external/googlemock.git' + '@' + '0421b6f358139f02e102c9c332ce19a33faf75be',
  'v8/test/benchmarks/data':
    Var('chromium_url') + '/v8/deps/third_party/benchmarks.git' + '@' + '05d7188267b4560491ff9155c5ee13e207ecd65f',
  'v8/test/mozilla/data':
    Var('chromium_url') + '/v8/deps/third_party/mozilla-tests.git' + '@' + 'f6c578a10ea707b1a8ab0b88943fe5115ce2b9be',
  'v8/test/test262/data':
    Var('chromium_url') + '/external/github.com/tc39/test262.git' + '@' + '8311965251953d4745aeb68c98fb71fab2eac1d0',
  'v8/test/test262/harness':
    Var('chromium_url') + '/external/github.com/test262-utils/test262-harness-py.git' + '@' + '0f2acdd882c84cff43b9d60df7574a1901e2cdcd',
  'v8/tools/clang':
    Var('chromium_url') + '/chromium/src/tools/clang.git' + '@' + 'ac1e5f78c39dc2a262af149404dd36c140eecdc1',
  'v8/tools/luci-go':
    Var('chromium_url') + '/chromium/src/tools/luci-go.git' + '@' + 'd882048313f6f51df29856406fa03b620c1d0205',
  'v8/test/wasm-js':
    Var('chromium_url') + '/external/github.com/WebAssembly/spec.git' + '@' + '5afc0fdde1eda1c0f1bb7aac761ca8aadf5b0420',
}

recursedeps = [
  'v8/buildtools',
  'v8/third_party/android_tools',
]

include_rules = [
  # Everybody can use some things.
  '+include',
  '+unicode',
  '+third_party/fdlibm',
]

# checkdeps.py shouldn't check for includes in these directories:
skip_child_includes = [
  'build',
  'gypfiles',
  'third_party',
]

hooks = [
  {
    # This clobbers when necessary (based on get_landmines.py). It must be the
    # first hook so that other things that get/generate into the output
    # directory will not subsequently be clobbered.
    'name': 'landmines',
    'pattern': '.',
    'action': [
        'python',
        'v8/gypfiles/landmines.py',
    ],
  },
  # Pull clang-format binaries using checked-in hashes.
  {
    'name': 'clang_format_win',
    'pattern': '.',
    'condition': 'host_os == "win" and build_for_node != True',
    'action': [ 'download_from_google_storage',
                '--no_resume',
                '--platform=win32',
                '--no_auth',
                '--bucket', 'chromium-clang-format',
                '-s', 'v8/buildtools/win/clang-format.exe.sha1',
    ],
  },
  {
    'name': 'clang_format_mac',
    'pattern': '.',
    'condition': 'host_os == "mac" and build_for_node != True',
    'action': [ 'download_from_google_storage',
                '--no_resume',
                '--platform=darwin',
                '--no_auth',
                '--bucket', 'chromium-clang-format',
                '-s', 'v8/buildtools/mac/clang-format.sha1',
    ],
  },
  {
    'name': 'clang_format_linux',
    'pattern': '.',
    'condition': 'host_os == "linux" and build_for_node != True',
    'action': [ 'download_from_google_storage',
                '--no_resume',
                '--platform=linux*',
                '--no_auth',
                '--bucket', 'chromium-clang-format',
                '-s', 'v8/buildtools/linux64/clang-format.sha1',
    ],
  },
  {
    'name': 'gcmole',
    'pattern': '.',
    'condition': 'build_for_node != True',
    # TODO(machenbach): Insert condition and remove GYP_DEFINES dependency.
    'action': [
        'python',
        'v8/tools/gcmole/download_gcmole_tools.py',
    ],
  },
  {
    'name': 'jsfunfuzz',
    'pattern': '.',
    'condition': 'build_for_node != True',
    # TODO(machenbach): Insert condition and remove GYP_DEFINES dependency.
    'action': [
        'python',
        'v8/tools/jsfunfuzz/download_jsfunfuzz.py',
    ],
  },
  # Pull luci-go binaries (isolate, swarming) using checked-in hashes.
  {
    'name': 'luci-go_win',
    'pattern': '.',
    'condition': 'host_os == "win" and build_for_node != True',
    'action': [ 'download_from_google_storage',
                '--no_resume',
                '--platform=win32',
                '--no_auth',
                '--bucket', 'chromium-luci',
                '-d', 'v8/tools/luci-go/win64',
    ],
  },
  {
    'name': 'luci-go_mac',
    'pattern': '.',
    'condition': 'host_os == "mac" and build_for_node != True',
    'action': [ 'download_from_google_storage',
                '--no_resume',
                '--platform=darwin',
                '--no_auth',
                '--bucket', 'chromium-luci',
                '-d', 'v8/tools/luci-go/mac64',
    ],
  },
  {
    'name': 'luci-go_linux',
    'pattern': '.',
    'condition': 'host_os == "linux" and build_for_node != True',
    'action': [ 'download_from_google_storage',
                '--no_resume',
                '--platform=linux*',
                '--no_auth',
                '--bucket', 'chromium-luci',
                '-d', 'v8/tools/luci-go/linux64',
    ],
  },
  # Pull GN using checked-in hashes.
  {
    'name': 'gn_win',
    'pattern': '.',
    'condition': 'host_os == "win"',
    'action': [ 'download_from_google_storage',
                '--no_resume',
                '--platform=win32',
                '--no_auth',
                '--bucket', 'chromium-gn',
                '-s', 'v8/buildtools/win/gn.exe.sha1',
    ],
  },
  {
    'name': 'gn_mac',
    'pattern': '.',
    'condition': 'host_os == "mac"',
    'action': [ 'download_from_google_storage',
                '--no_resume',
                '--platform=darwin',
                '--no_auth',
                '--bucket', 'chromium-gn',
                '-s', 'v8/buildtools/mac/gn.sha1',
    ],
  },
  {
    'name': 'gn_linux',
    'pattern': '.',
    'condition': 'host_os == "linux"',
    'action': [ 'download_from_google_storage',
                '--no_resume',
                '--platform=linux*',
                '--no_auth',
                '--bucket', 'chromium-gn',
                '-s', 'v8/buildtools/linux64/gn.sha1',
    ],
  },
  {
    'name': 'wasm_spec_tests',
    'pattern': '.',
    'condition': 'build_for_node != True',
    'action': [ 'download_from_google_storage',
                '--no_resume',
                '--no_auth',
                '-u',
                '--bucket', 'v8-wasm-spec-tests',
                '-s', 'v8/test/wasm-spec-tests/tests.tar.gz.sha1',
    ],
  },
  {
    'name': 'closure_compiler',
    'pattern': '.',
    'condition': 'build_for_node != True',
    'action': [ 'download_from_google_storage',
                '--no_resume',
                '--no_auth',
                '-u',
                '--bucket', 'chromium-v8-closure-compiler',
                '-s', 'v8/src/inspector/build/closure-compiler.tar.gz.sha1',
    ],
  },
  {
    # Downloads the current stable linux sysroot to build/linux/ if needed.
    # This sysroot updates at about the same rate that the chrome build deps
    # change.
    'name': 'sysroot',
    'pattern': '.',
    'condition': 'build_for_node != True',
    'action': [
        'python',
        'v8/build/linux/sysroot_scripts/install-sysroot.py',
        '--running-as-hook',
    ],
  },
  {
    'name': 'msan_chained_origins',
    'pattern': '.',
    'condition': 'checkout_instrumented_libraries',
    'action': [ 'download_from_google_storage',
                '--no_resume',
                '--no_auth',
                '--bucket', 'chromium-instrumented-libraries',
                '-s', 'v8/third_party/instrumented_libraries/binaries/msan-chained-origins-trusty.tgz.sha1',
              ],
  },
  {
    'name': 'msan_no_origins',
    'pattern': '.',
    'condition': 'checkout_instrumented_libraries',
    'action': [ 'download_from_google_storage',
                '--no_resume',
                '--no_auth',
                '--bucket', 'chromium-instrumented-libraries',
                '-s', 'v8/third_party/instrumented_libraries/binaries/msan-no-origins-trusty.tgz.sha1',
              ],
  },
  {
    # Update the Windows toolchain if necessary.
    'name': 'win_toolchain',
    'pattern': '.',
    'condition': 'checkout_win',
    'action': ['python', 'v8/build/vs_toolchain.py', 'update'],
  },
  # Pull binutils for linux, enabled debug fission for faster linking /
  # debugging when used with clang on Ubuntu Precise.
  # https://code.google.com/p/chromium/issues/detail?id=352046
  {
    'name': 'binutils',
    'pattern': 'v8/third_party/binutils',
    'condition': 'host_os == "linux" and build_for_node != True',
    'action': [
        'python',
        'v8/third_party/binutils/download.py',
    ],
  },
  {
    # Note: On Win, this should run after win_toolchain, as it may use it.
    'name': 'clang',
    'pattern': '.',
    'action': ['python', 'v8/tools/clang/scripts/update.py'],
  },
  {
    'name': 'fuchsia_sdk',
    'pattern': '.',
    'condition': 'checkout_fuchsia',
    'action': [
      'python',
      'v8/build/fuchsia/update_sdk.py',
      '226f6dd0cad1d6be63a353ce2649423470729ae9',
    ],
  },
  {
    # A change to a .gyp, .gypi, or to GYP itself should run the generator.
    'name': 'regyp_if_needed',
    'pattern': '.',
    'condition': 'build_for_node != True',
    'action': ['python', 'v8/gypfiles/gyp_v8', '--running-as-hook'],
  },
  # Download and initialize "vpython" VirtualEnv environment packages.
  {
    'name': 'vpython_common',
    'pattern': '.',
    'condition': 'checkout_android',
    'action': [ 'vpython',
                '-vpython-spec', 'v8/.vpython',
                '-vpython-tool', 'install',
    ],
  },
]
