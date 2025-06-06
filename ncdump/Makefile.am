## This is a automake file, part of Unidata's netCDF package.
# Copyright 2018, see the COPYRIGHT file for more information.
# This file builds and runs the ncdump program.

# Ed Hartnett, Dennis Heimbigner, Ward Fisher

#SH_LOG_DRIVER = $(SHELL) $(top_srcdir)/test-driver-verbose
#sh_LOG_DRIVER = $(SHELL) $(top_srcdir)/test-driver-verbose
#LOG_DRIVER = $(SHELL) $(top_srcdir)/test-driver-verbose
#TESTS_ENVIRONMENT = export SETX=1;

# Put together AM_CPPFLAGS and AM_LDFLAGS.
include $(top_srcdir)/lib_flags.am
LDADD = ${top_builddir}/liblib/libnetcdf.la
noinst_PROGRAMS=
# Note which tests depend on other tests. Necessary for make -j check.
TEST_EXTENSIONS = .sh
XFAIL_TESTS=""

# This is the program we're building, and it's sources.
bin_PROGRAMS = ncdump
ncdump_SOURCES = ncdump.c vardata.c dumplib.c indent.c nctime0.c        \
ncdump.h vardata.h dumplib.h indent.h nctime0.h cdl.h utils.h   \
utils.c nciter.h nciter.c nccomps.h

# Another utility program that copies any netCDF file using only the
# netCDF API
bin_PROGRAMS += nccopy
nccopy_SOURCES = nccopy.c nciter.c nciter.h chunkspec.h chunkspec.c     \
utils.h utils.c dimmap.h dimmap.c list.c list.h

# Wei-keng Liao's (wkliao@eecs.northwestern.edu)
# netcdf-3 validator program
# (https://github.com/Parallel-NetCDF/PnetCDF/blob/master/src/utils/ncvalidator/ncvalidator.c)
# that prints out the structure of a netcdf-3 file.
# This program is built but not installed.
noinst_PROGRAMS += ncvalidator
ncvalidator_SOURCES = ncvalidator.c

# An equivalent to echo that always implements -n flag
noinst_PROGRAMS += echon
echon_SOURCES = echon.c

# A non-installed utility program to convert paths; similar to cygpath
noinst_PROGRAMS += ncpathcvt
ncpathcvt_SOURCES = ncpathcvt.c

# A non-installed utility program to test if a filter is available
noinst_PROGRAMS += ncfilteravail
ncfilteravail_SOURCES = ncfilteravail.c

# A simple netcdf-4 metadata -> xml printer. Do not install.
if USE_HDF5
bin_PROGRAMS += nc4print
noinst_PROGRAMS += nc4print
nc4print_SOURCES = nc4print.c nc4printer.c

# Create a helper program for tst_scope.sh
# Program prints out the fqn of the type of
# a specified variable in the .nc file.
noinst_PROGRAMS += printfqn
printfqn_SOURCES = printfqn.c

# A non-installed utility program to return
# the HDF5 Version as an integer of the form XXXYYYZZZ with no newline.
# Where XXX is the major version number, YYY is the minor version number
# and ZZZ is the patch level.
noinst_PROGRAMS += nchdf5version
nchdf5version_SOURCES = nchdf5version.c

endif

# Conditionally build the ocprint program, but do not install
if NETCDF_ENABLE_DAP
bin_PROGRAMS += ocprint
noinst_PROGRAMS += ocprint
ocprint_SOURCES = ocprint.c
endif

# This is the man page.
man_MANS = ncdump.1 nccopy.1

if BUILD_TESTSETS
# C programs needed by shell scripts for classic tests.
check_PROGRAMS = rewrite-scalar ref_ctest ref_ctest64 ncdump tst_utf8   \
bom tst_dimsizes nctrunc tst_rcmerge tst_rcapi

# Tests for classic and 64-bit offset files.
TESTS = tst_inttags.sh run_tests.sh tst_64bit.sh ref_ctest	\
ref_ctest64 tst_lengths.sh tst_calendars.sh	\
run_utf8_tests.sh tst_nccopy3_subset.sh		\
tst_charfill.sh tst_iter.sh tst_formatx3.sh tst_bom.sh		\
tst_dimsizes.sh run_ncgen_tests.sh tst_ncgen4_classic.sh        \
test_radix.sh test_rcmerge.sh

# The tst_nccopy3.sh test uses output from a bunch of other
# tests. This records the dependency so parallel builds work.
tst_nccopy3.log: tst_calendars.log run_utf8_tests.log tst_output.log	\
                 tst_64bit.log run_tests.log tst_lengths.log

TESTS += tst_null_byte_padding.sh
if USE_STRICT_NULL_BYTE_HEADER_PADDING
XFAIL_TESTS += tst_null_byte_padding.sh
endif

if LARGE_FILE_TESTS
TESTS += tst_iter.sh
endif

TESTS += testpathcvt.sh

if USE_HDF5
# HDF5 has some extra C programs to build. These will be run by
# the shell script tests.
check_PROGRAMS += tst_fileinfo tst_create_files tst_h_rdc0	\
tst_group_data tst_enum_data tst_opaque_data tst_string_data	\
tst_vlen_data tst_comp tst_comp2 tst_nans tst_special_atts	\
tst_unicode tst_fillbug tst_compress tst_chunking tst_h_scalar  \
tst_enum_undef

check_PROGRAMS += tst_vlen_demo

# Tests for netCDF-4 behavior.
TESTS += tst_fileinfo.sh tst_hdf5_offset.sh tst_inttags4.sh		\
tst_netcdf4.sh tst_fillbug.sh \
tst_grp_spec.sh tst_mud.sh tst_h_scalar.sh tst_formatx4.sh		\
run_utf8_nc4_tests.sh run_ncgen_nc4_tests.sh	\
tst_ncgen4.sh test_scope.sh

# Record interscript dependencies so parallel builds work.
tst_nccopy4.log: run_ncgen_tests.log tst_output.log tst_ncgen4.log	\
                 tst_fillbug.log tst_netcdf4_4.log tst_h_scalar.log tst_netcdf4.log
tst_nccopy5.log: tst_nccopy4.log

endif #USE_HDF5

TESTS += tst_inmemory_nc3.sh tst_nccopy_w3.sh
if USE_HDF5
TESTS += tst_inmemory_nc4.sh tst_nccopy_w4.sh
endif

if USE_HDF5
# Re-activate the ncgen -lc tests
TESTS += tst_ctests.sh
endif

if NETCDF_ENABLE_CDF5
# Test for keywords as identifiers
TESTS += test_keywords.sh
endif

if !ISMINGW
if !ISCYGWIN
TESTS += tst_output.sh
TESTS += tst_nccopy3.sh
if USE_HDF5
TESTS += run_back_comp_tests.sh tst_netcdf4_4.sh
TESTS += tst_nccopy4.sh tst_nccopy5.sh tst_calendars_nc4.sh
endif
endif
endif

# The unicode tests
if !ISMINGW
TESTS += test_unicode_directory.sh  test_unicode_path.sh
endif

endif BUILD_TESTSETS

# These files all have to be included with the distribution.
EXTRA_DIST = run_tests.sh tst_64bit.sh tst_output.sh test0.cdl		\
ref_ctest1_nc4.cdl ref_ctest1_nc4c.cdl ref_tst_solar_1.cdl		\
ref_tst_solar_2.cdl tst_netcdf4.sh tst_netcdf4_4.sh ref_tst_small.cdl	\
tst_lengths.sh tst_ncml.cdl ref1.ncml ref_tst_group_data.cdl		\
ref_tst_enum_data.cdl ref_tst_opaque_data.cdl ref_tst_string_data.cdl	\
ref_tst_vlen_data.cdl ref_tst_comp.cdl ref_tst_unicode.cdl		\
ref_tst_nans.cdl small.cdl small2.cdl $(man_MANS) run_utf8_tests.sh	\
ref_tst_utf8.cdl ref_tst_fillbug.cdl tst_fillbug.sh tst_calendars.cdl	\
tst_calendars.sh ref_times.cdl ref_tst_special_atts.cdl			\
ref_tst_noncoord.cdl ref_tst_compounds2.nc ref_tst_compounds2.cdl	\
ref_tst_compounds3.nc ref_tst_compounds3.cdl ref_tst_compounds4.nc	\
ref_tst_compounds4.cdl ref_tst_group_data_v23.cdl tst_mslp.cdl		\
tst_bug321.cdl ref_tst_format_att.cdl ref_tst_format_att_64.cdl		\
tst_nccopy3.sh tst_nccopy4.sh tst_nccopy5.sh tst_calendars_nc4.sh	\
ref_nc_test_netcdf4_4_0.nc run_back_comp_tests.sh			\
ref_nc_test_netcdf4.cdl ref_tst_special_atts3.cdl tst_brecs.cdl		\
ref_tst_grp_spec0.cdl ref_tst_grp_spec.cdl tst_grp_spec.sh		\
ref_tst_charfill.cdl tst_charfill.cdl tst_charfill.sh tst_iter.sh	\
tst_mud.sh ref_tst_mud4.cdl ref_tst_mud4-bc.cdl				\
ref_tst_mud4_chars.cdl inttags.cdl inttags4.cdl ref_inttags.cdl		\
ref_inttags4.cdl ref_tst_ncf213.cdl tst_h_scalar.sh			\
run_utf8_nc4_tests.sh tst_formatx3.sh tst_formatx4.sh			\
ref_tst_utf8_4.cdl ref_tst_nc4_utf8_4.cdl tst_inttags.sh		\
tst_inttags4.sh CMakeLists.txt tst_bom.sh tst_inmemory_nc3.sh		\
tst_dimsizes.sh tst_inmemory_nc4.sh tst_fileinfo.sh			\
run_ncgen_tests.sh ref_test_360_day_1900.nc ref_test_365_day_1900.nc	\
ref_test_366_day_1900.nc ref_test_360_day_1900.cdl			\
ref_test_365_day_1900.cdl ref_test_366_day_1900.cdl			\
tst_hdf5_offset.sh run_ncgen_nc4_tests.sh tst_nccopy3_subset.sh		\
ref_nccopy3_subset.nc ref_test_corrupt_magic.nc tst_ncgen_shared.sh	\
tst_ncgen4.sh tst_ncgen4_classic.sh tst_ncgen4_diff.sh			\
tst_ncgen4_cycle.sh tst_null_byte_padding.sh				\
ref_null_byte_padding_test.nc ref_tst_irish_rover.nc			\
ref_provenance_v1.nc ref_tst_radix.cdl tst_radix.cdl test_radix.sh	\
ref_nccopy_w.cdl tst_nccopy_w3.sh tst_nccopy_w4.sh			\
ref_no_ncproperty.nc test_unicode_directory.sh test_unicode_path.sh      \
ref_roman_szip_simple.cdl ref_roman_szip_unlim.cdl ref_tst_perdimspecs.cdl \
test_keywords.sh ref_keyword1.cdl ref_keyword2.cdl ref_keyword3.cdl ref_keyword4.cdl \
ref_tst_nofilters.cdl test_scope.sh  \
test_rcmerge.sh ref_rcmerge1.txt ref_rcmerge2.txt ref_rcmerge3.txt \
scope_ancestor_only.cdl scope_ancestor_subgroup.cdl scope_group_only.cdl scope_preorder.cdl \
ref_rcapi.txt ref_tst_enum_undef.cdl tst_calendars_nc4.cdl ref_times_nc4.cdl ref_tst_comp2.cdl

# The L512.bin file is file containing exactly 512 bytes each of value 0.
# It is used for creating hdf5 files with varying offsets for testing.
EXTRA_DIST += L512.bin

EXTRA_DIST += tst_ctests.sh ref_ctest_small_3.c ref_ctest_small_4.c \
	ref_ctest_special_atts_4.c test_ncdump.sh

EXTRA_DIST += testpathcvt.sh ref_pathcvt.txt

# CDL files and Expected results
SUBDIRS = cdl expected

CLEANFILES = tst_*.nc tmp*.nc test*.nc iter.* tmp*.cdl tmp*.txt         \
tst_output_*.cdl tst_output_*.c tst_utf8_*.cdl *.tmp tst_tst8.cdl	\
tst_netcdf4_*.cdl test1_ncdump.cdl test2_ncdump.cdl test1.cdl		\
ctest1.cdl test1_cdf5.cdl test2_cdf5.cdl test1_offset.cdl		\
test2_offset.cdl ctest0.nc ctest0_64.nc c1.cdl c1_4.cdl ctest1_64.cdl	\
c0.nc c0_4.nc small.nc small2.nc c0tmp.nc c1.ncml utf8.cdl		\
utf8_64.cdl utf8.nc utf8_64.nc nc4_utf8.cdl nc4_utf8.nc			\
tst_unicode.cdl tst_group_data.cdl tst_compounds2.cdl tst_comp.cdl	\
tst_enum_data.cdl tst_small.cdl tst_times.cdl tst_solar_2.cdl		\
tst_string_data.cdl tst_fillbug.cdl tst_opaque_data.cdl			\
tst_compounds4.cdl tst_utf8.cdl tst_compounds3.cdl			\
tst_special_atts.cdl tst_nans.cdl tst_format_att_64.cdl			\
tst_vlen_data.cdl tst_solar_1.cdl tst_format_att.cdl			\
tst_nc_test_netcdf4_4_0.cdl tst_mud4.cdl tst_mud4-bc.cdl		\
tst_ncf213.cdl tst_h_scalar.cdl tst_mud4_chars.cdl inttags.nc		\
inttags4.nc tst_inttags.cdl tst_inttags4.cdl nc4_fileinfo.nc		\
hdf5_fileinfo.hdf nccopy3_subset_out.nc c5.nc				\
compound_datasize_test.nc compound_datasize_test2.nc ncf199.nc		\
tst_c0.cdl tst_c0_4.cdl tst_c0_4c.cdl tst_c0_64.cdl			\
tst_compound_datasize_test.cdl tst_compound_datasize_test2.cdl		\
tst_ncf199.cdl tst_tst_gattenum.cdl tst_tst_usuffix.cdl ctest.c		\
ctest64.c nccopy3_subset_out.nc camrun.c tst_ncf213.cdl tst_ncf213.nc	\
tst_radix.nc tmp_radix.cdl ctest_small_3.c ctest_small_4.c		\
ctest_special_atts_4.c tst_roman_szip_simple.cdl			\
tst_roman_szip_unlim.cdl tst_perdimpspecs.nc tmppds.*                   \
keyword1.nc keyword2.nc keyword3.nc keyword4.nc                         \
tmp_keyword1.cdl tmp_keyword2.cdl tmp_keyword3.cdl tmp_keyword4.cdl     \
type_*.nc copy_type_*.cdl                                               \
scope_*.nc copy_scope_*.cdl keyword5.nc tst_enum_undef.cdl tst_times_nc4.cdl

# Remove directories
clean-local:
	rm -fr rcmergedir rchome testset_*
