licenses(['notice'])

if repository_name() == '@':
    gendir = '$(GENDIR)/%s' % package_name()
else:
    gendir = '$(GENDIR)/external/%s' % repository_name.lstrip('@')

# See NON-AUTOTOOLS-BUILD to get inspirations on how to build pcre with Bazel.

PCRE_MAJOR = 8
PCRE_MINOR = 41
CONFIG_COPTS = [
    '-DHAVE_BCOPY=1',
    '-DHAVE_DIRENT_H=1',
    '-DHAVE_DLFCN_H=1',
    '-DHAVE_INTTYPES_H=1',
    '-DHAVE_LIMITS_H=1',
    '-DHAVE_LONG_LONG=1',
    '-DHAVE_MEMMOVE=1',
    '-DHAVE_MEMORY_H=1',
    '-DHAVE_STDINT_H=1',
    '-DHAVE_STDLIB_H=1',
    '-DHAVE_STRERROR=1',
    '-DHAVE_STRING=1',
    '-DHAVE_STRINGS_H=1',
    '-DHAVE_STRING_H=1',
    '-DHAVE_STRTOQ=1',
    '-DHAVE_SYS_STAT_H=1',
    '-DHAVE_SYS_TYPES_H=1',
    '-DHAVE_UNISTD_H=1',
    '-DHAVE_UNSIGNED_LONG_LONG=1',
    '-DHAVE_VISIBILITY=1',
    '-DHAVE_ZLIB_H=1',
    '-DLINK_SIZE=2',
    '-DLT_OBJDIR=".libs/"',
    '-DMATCH_LIMIT=10000000',
    '-DMATCH_LIMIT_RECURSION=MATCH_LIMIT',
    '-DMAX_NAME_COUNT=10000',
    '-DMAX_NAME_SIZE=32',
    '-DNEWLINE=10',
    '-DPACKAGE="pcre"',
    '-DPACKAGE_BUGREPORT=""',
    '-DPACKAGE_NAME="PCRE"',
    '-DPACKAGE_STRING="PCRE=%d.%d"' % (PCRE_MAJOR, PCRE_MINOR),
    '-DPACKAGE_TARNAME="pcre"',
    '-DPACKAGE_URL=""',
    '-DPACKAGE_VERSION="%d.%d"' % (PCRE_MAJOR, PCRE_MINOR),
    '-DPARENS_NEST_LIMIT=250',
    '-DPCREGREP_BUFSIZE=20480',
    '-DPOSIX_MALLOC_THRESHOLD=10',
    '-DSTDC_HEADERS=1',
    '-DSUPPORT_PCRE%d' % PCRE_MAJOR,
    '-DVERSION="%d.%d"' % (PCRE_MAJOR, PCRE_MINOR),
    r'-DPCRECPP_EXP_DECL="extern __attribute__ ((visibility (\"default\")))"',
    r'-DPCRECPP_EXP_DEFN="__attribute__ ((visibility (\"default\")))"',
    r'-DPCREPOSIX_EXP_DECL="extern __attribute__ ((visibility (\"default\")))"',
    r'-DPCREPOSIX_EXP_DEFN="extern __attribute__ ((visibility (\"default\")))"',
    r'-DPCRE_EXP_DATA_DEFN="__attribute__ ((visibility (\"default\")))"',
    r'-DPCRE_EXP_DECL="extern __attribute__ ((visibility (\"default\")))"',
    r'-DPCRE_EXP_DEFN="__attribute__ ((visibility (\"default\")))"',
]

cc_library(
    name = 'pcre',
    visibility = [
        '//visibility:public',
    ],
    srcs = [
        ':pcre_chartables_c',
        ':pcre_stringpiece_h',
        'pcre_byte_order.c',
        'pcre_compile.c',
        'pcre_config.c',
        'pcre_dfa_exec.c',
        'pcre_exec.c',
        'pcre_fullinfo.c',
        'pcre_get.c',
        'pcre_globals.c',
        'pcre_internal.h',
        'pcre_jit_compile.c',
        'pcre_maketables.c',
        'pcre_newline.c',
        'pcre_ord2utf8.c',
        'pcre_refcount.c',
        'pcre_string_utils.c',
        'pcre_study.c',
        'pcre_tables.c',
        'pcre_ucd.c',
        'pcre_valid_utf8.c',
        'pcre_version.c',
        'pcre_xclass.c',
        'pcreposix.c',
        'pcreposix.h',
        'ucp.h',
    ],
    textual_hdrs = [
        'sljit/sljitExecAllocator.c',
        'sljit/sljitLir.c',
        'sljit/sljitNativeARM_32.c',
        'sljit/sljitNativeARM_64.c',
        'sljit/sljitNativeARM_T2_32.c',
        'sljit/sljitNativeMIPS_32.c',
        'sljit/sljitNativeMIPS_64.c',
        'sljit/sljitNativeMIPS_common.c',
        'sljit/sljitNativePPC_32.c',
        'sljit/sljitNativePPC_64.c',
        'sljit/sljitNativePPC_common.c',
        'sljit/sljitNativeSPARC_32.c',
        'sljit/sljitNativeSPARC_common.c',
        'sljit/sljitNativeTILEGX-encoder.c',
        'sljit/sljitNativeTILEGX_64.c',
        'sljit/sljitNativeX86_32.c',
        'sljit/sljitNativeX86_64.c',
        'sljit/sljitNativeX86_common.c',
        'sljit/sljitProtExecAllocator.c',
        'sljit/sljitUtils.c',
    ],
    hdrs = [
        ':pcre_h',
    ],
    includes = [
        'pcre_internal',
    ],
    copts = CONFIG_COPTS + [
        '-I%s' % '/'.join([gendir]),
        '-I%s' % '/'.join([gendir, 'pcre_internal']),
        '-I%s' % '/'.join(['.', package_name()]),

        # Suppress known warnings:
        '-Wno-self-assign',
        '-Wno-unused-const-variable',
    ],
)

genrule(
    name = 'pcre_chartables_c',
    outs = [
        'pcre_chartables.c',
    ],
    srcs = [
        'pcre_chartables.c.dist',
    ],
    cmd = 'cp $< $@',
)

genrule(
    name = 'pcre_h',
    outs = [
        'pcre_internal/pcre.h',
    ],
    srcs = [
        'pcre.h.in',
    ],
    cmd = r'''sed $< \
            -e 's|@PCRE_MAJOR@|%d|g' \
            -e 's|@PCRE_MINOR@|%d|g' \
            -e 's|@PCRE_PRERELEASE@|pcre_prerelease|g' \
            -e 's|@PCRE_DATE@|REDACTED|g' \
            > $@''' % (PCRE_MAJOR, PCRE_MINOR),
)

genrule(
    name = 'pcre_stringpiece_h',
    outs = [
        'pcre_stringpiece.h',
    ],
    srcs = [
        'pcre_stringpiece.h.in',
    ],
    cmd = r'''sed $< \
            -e 's|@pcre_have_type_traits@|1|g' \
            -e 's|@pcre_have_bits_type_traits@|1|g' \
            > $@''',
)

