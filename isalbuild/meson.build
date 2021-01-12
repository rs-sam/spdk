# SPDX-License-Identifier: BSD-3-Clause
# Copyright (c) 2020, MayaData Inc. All rights reserved.
# Copyright (c) 2020, DataCore Software Corporation. All rights reserved.

project('isa-l', 'C',
	version: '2.29',
	license: 'BSD',
	default_options: [
		'buildtype=release',
		'default_library=static',
		'b_vscrt=none',
		'warning_level=3'
		],
	meson_version: '>= 0.55.0'
)

cc = meson.get_compiler('c')

# MS linker requires special treatment.
is_ms_linker = (cc.get_id() == 'clang')

warnings = [
	'-Wdeprecated',
	'-Wformat',
	'-Wformat-security',
	'-Wnested-externs',
	'-Wpointer-arith',
	'-Wno-unused-parameter',
	'-Wno-sign-compare',
	'-Wundef',
]

foreach warning : warnings
	if cc.has_argument(warning)
		add_project_arguments(warning, language: 'c')
	endif
endforeach

cflags = [ '-D_USE_MATH_DEFINES', '-D__WORDSIZE=64' ]

c_includes = include_directories('.', 'erasure_code', 'raid', 'crc', 'igzip', 'include', 'mem')

yasm = find_program('yasm')

asm_includes = [ ]

foreach i : [ 'include' ]
	asm_includes = '-I' + join_paths(meson.source_root(), i)
endforeach

sources = [
	'erasure_code/ec_base.c',
	'erasure_code/ec_highlevel_func.c',
	'raid/raid_base.c',
	'crc/crc64_base.c',
	'crc/crc_base.c',
	'igzip/adler32_base.c',
	'igzip/encode_df.c',
	'igzip/flatten_ll.c',
	'igzip/hufftables_c.c',
	'igzip/huff_codes.c',
	'igzip/igzip.c',
	'igzip/igzip_base.c',
	'igzip/igzip_icf_base.c',
	'igzip/igzip_icf_body.c',
	'igzip/igzip_inflate.c',
	'mem/mem_zero_detect_base.c',
]

asm_sources = [
	'erasure_code/ec_multibinary.asm',
	'erasure_code/gf_2vect_dot_prod_avx.asm',
	'erasure_code/gf_2vect_dot_prod_avx2.asm',
	'erasure_code/gf_2vect_dot_prod_avx512.asm',
	'erasure_code/gf_2vect_dot_prod_sse.asm',
	'erasure_code/gf_2vect_mad_avx.asm',
	'erasure_code/gf_2vect_mad_avx2.asm',
	'erasure_code/gf_2vect_mad_avx512.asm',
	'erasure_code/gf_2vect_mad_sse.asm',
	'erasure_code/gf_3vect_dot_prod_avx.asm',
	'erasure_code/gf_3vect_dot_prod_avx2.asm',
	'erasure_code/gf_3vect_dot_prod_avx512.asm',
	'erasure_code/gf_3vect_dot_prod_sse.asm',
	'erasure_code/gf_3vect_mad_avx.asm',
	'erasure_code/gf_3vect_mad_avx2.asm',
	'erasure_code/gf_3vect_mad_avx512.asm',
	'erasure_code/gf_3vect_mad_sse.asm',
	'erasure_code/gf_4vect_dot_prod_avx.asm',
	'erasure_code/gf_4vect_dot_prod_avx2.asm',
	'erasure_code/gf_4vect_dot_prod_avx512.asm',
	'erasure_code/gf_4vect_dot_prod_sse.asm',
	'erasure_code/gf_4vect_mad_avx.asm',
	'erasure_code/gf_4vect_mad_avx2.asm',
	'erasure_code/gf_4vect_mad_avx512.asm',
	'erasure_code/gf_4vect_mad_sse.asm',
	'erasure_code/gf_5vect_dot_prod_avx.asm',
	'erasure_code/gf_5vect_dot_prod_avx2.asm',
	'erasure_code/gf_5vect_dot_prod_avx512.asm',
	'erasure_code/gf_5vect_dot_prod_sse.asm',
	'erasure_code/gf_5vect_mad_avx.asm',
	'erasure_code/gf_5vect_mad_avx2.asm',
	'erasure_code/gf_5vect_mad_avx512.asm',
	'erasure_code/gf_5vect_mad_sse.asm',
	'erasure_code/gf_6vect_dot_prod_avx.asm',
	'erasure_code/gf_6vect_dot_prod_avx2.asm',
	'erasure_code/gf_6vect_dot_prod_avx512.asm',
	'erasure_code/gf_6vect_dot_prod_sse.asm',
	'erasure_code/gf_6vect_mad_avx.asm',
	'erasure_code/gf_6vect_mad_avx2.asm',
	'erasure_code/gf_6vect_mad_avx512.asm',
	'erasure_code/gf_6vect_mad_sse.asm',
	'erasure_code/gf_vect_dot_prod_avx.asm',
	'erasure_code/gf_vect_dot_prod_avx2.asm',
	'erasure_code/gf_vect_dot_prod_avx512.asm',
	'erasure_code/gf_vect_dot_prod_sse.asm',
	'erasure_code/gf_vect_mad_avx.asm',
	'erasure_code/gf_vect_mad_avx2.asm',
	'erasure_code/gf_vect_mad_avx512.asm',
	'erasure_code/gf_vect_mad_sse.asm',
	'erasure_code/gf_vect_mul_avx.asm',
	'erasure_code/gf_vect_mul_sse.asm',
	'raid/pq_check_sse.asm',
	'raid/pq_gen_avx.asm',
	'raid/pq_gen_avx2.asm',
	'raid/pq_gen_avx512.asm',
	'raid/pq_gen_sse.asm',
	'raid/raid_multibinary.asm',
	'raid/xor_check_sse.asm',
	'raid/xor_gen_avx.asm',
	'raid/xor_gen_avx512.asm',
	'raid/xor_gen_sse.asm',
	'crc/crc16_t10dif_01.asm',
	'crc/crc16_t10dif_02.asm',
	'crc/crc16_t10dif_by16_10.asm',
	'crc/crc16_t10dif_by4.asm',
	'crc/crc16_t10dif_copy_by4.asm',
	'crc/crc16_t10dif_copy_by4_02.asm',
	'crc/crc32_gzip_refl_by16_10.asm',
	'crc/crc32_gzip_refl_by8.asm',
	'crc/crc32_gzip_refl_by8_02.asm',
	'crc/crc32_ieee_01.asm',
	'crc/crc32_ieee_02.asm',
	'crc/crc32_ieee_by16_10.asm',
	'crc/crc32_ieee_by4.asm',
	'crc/crc32_iscsi_00.asm',
	'crc/crc32_iscsi_01.asm',
	'crc/crc64_ecma_norm_by16_10.asm',
	'crc/crc64_ecma_norm_by8.asm',
	'crc/crc64_ecma_refl_by16_10.asm',
	'crc/crc64_ecma_refl_by8.asm',
	'crc/crc64_iso_norm_by16_10.asm',
	'crc/crc64_iso_norm_by8.asm',
	'crc/crc64_iso_refl_by16_10.asm',
	'crc/crc64_iso_refl_by8.asm',
	'crc/crc64_jones_norm_by16_10.asm',
	'crc/crc64_jones_norm_by8.asm',
	'crc/crc64_jones_refl_by16_10.asm',
	'crc/crc64_jones_refl_by8.asm',
	'crc/crc64_multibinary.asm',
	'crc/crc_multibinary.asm',
	'igzip/adler32_avx2_4.asm',
	'igzip/adler32_sse.asm',
	'igzip/encode_df_04.asm',
	'igzip/igzip_body.asm',
	'igzip/igzip_decode_block_stateless_01.asm',
	'igzip/igzip_decode_block_stateless_04.asm',
	'igzip/igzip_deflate_hash.asm',
	'igzip/igzip_finish.asm',
	'igzip/igzip_gen_icf_map_lh1_04.asm',
	'igzip/igzip_gen_icf_map_lh1_06.asm',
	'igzip/igzip_icf_body_h1_gr_bt.asm',
	'igzip/igzip_icf_finish.asm',
	'igzip/igzip_inflate_multibinary.asm',
	'igzip/igzip_multibinary.asm',
	'igzip/igzip_set_long_icf_fg_04.asm',
	'igzip/igzip_set_long_icf_fg_06.asm',
	'igzip/igzip_update_histogram_01.asm',
	'igzip/igzip_update_histogram_04.asm',
	'igzip/proc_heap.asm',
	'igzip/rfc1951_lookup.asm',
	'mem/mem_multibinary.asm',
	'mem/mem_zero_detect_avx.asm',
	'mem/mem_zero_detect_sse.asm',
]

install_headers(
	'include/crc.h',
	subdir: '../isa-l'
)

gen = generator(yasm,
	output : '@BASENAME@.obj',
	arguments : [ '-f', 'win64', '@INPUT@', '-o', '@OUTPUT@', asm_includes ])

asm_objects = gen.process(asm_sources)

# build static lib
static_lib = static_library('isal',
		sources, asm_objects,
		c_args: cflags,
		include_directories: [ c_includes ],
		install: true,
		install_dir: '.libs')

# use pre-build objects to build shared lib
sources = []
objs = static_lib.extract_all_objects(recursive: false)

def_file = join_paths(meson.source_root(), 'isa-l.def')
def_arg = []

if cc.get_id() == 'msvc'
	def_arg = ['/def:' + def_file]
elif is_ms_linker
	def_arg = ['-Wl,/def:' + def_file]
endif

# build shared lib
shared_lib = shared_library('isa-l',
		sources,
		objects: objs,
		c_args: cflags,
		include_directories: [ c_includes ],
		link_depends: [ def_file ],
		link_args: [ def_arg ],
		install: true,
		install_dir: '.libs')
