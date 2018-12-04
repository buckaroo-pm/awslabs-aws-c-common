load('//:subdir_glob.bzl', 'subdir_glob')

genrule(
  name = 'config-h', 
  out = 'config.h', 
  srcs = glob([
    'cmake/**/*.c', 
    'cmake/**/*.cmake', 
    'cmake/**/*.txt', 
    'include/**/*.in', 
    'source/**/*.c', 
    'source/**/*.in', 
    'source/**/*.txt', 
    'CMakeLists.txt', 
  ]), 
  cmd = ' && '.join([
    'mkdir -p $SRCDIR/tests', 
    'touch $SRCDIR/tests/CMakeLists.txt', 
    'cd $TMP', 
    'cmake $SRCDIR -G "Unix Makefiles"', 
    'cp $TMP/generated/include/aws/common/config.h $OUT', 
  ]), 
)

linux_srcs = glob([
  'source/posix/**/*.c', 
])

macos_srcs = glob([
  'source/posix/**/*.c', 
])

windows_srcs = glob([
  'source/windows/**/*.c', 
])

cxx_library(
  name = 'aws-c-common', 
  header_namespace = '', 
  exported_headers = dict(
    subdir_glob([
      ('include', '**/*.c'), 
      ('include', '**/*.h'), 
      ('include', '**/*.inl'), 
    ]).items() + [
      ('aws/common/config.h', ':config-h'), 
    ]
  ), 
  srcs = glob([
    'source/*.c', 
  ]), 
  platform_srcs = [
    ('linux.*', linux_srcs), 
    ('macos.*', macos_srcs), 
    ('windows.*', windows_srcs), 
  ], 
  exported_platform_linker_flags = [
    ('linux.*', [ '-lpthread' ]), 
  ], 
  visibility = [
    'PUBLIC', 
  ], 
)
