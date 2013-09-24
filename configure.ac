AC_PREREQ(2.63)

AC_INIT([appdata-tools],[0.1.3])
AC_CONFIG_SRCDIR(src)
AM_INIT_AUTOMAKE([1.9 no-dist-gzip dist-xz tar-ustar])
AC_CONFIG_HEADERS([config.h])
AC_CONFIG_MACRO_DIR([m4])
AC_SYS_LARGEFILE

# enable nice build output on automake1.11
m4_ifdef([AM_SILENT_RULES],[AM_SILENT_RULES([yes])])

AS_ALL_LINGUAS
AC_PROG_CC
AC_PROG_INSTALL
AC_HEADER_STDC
LT_INIT
AM_PROG_CC_C_O

# Internationalisation
IT_PROG_INTLTOOL([0.35.0])
GETTEXT_PACKAGE=appdata-tools
AC_SUBST([GETTEXT_PACKAGE])
AM_GLIB_GNU_GETTEXT
AC_DEFINE_UNQUOTED([GETTEXT_PACKAGE],["$GETTEXT_PACKAGE"],[gettext domain])

PKG_CHECK_MODULES(GLIB, glib-2.0 >= 2.14.0 gobject-2.0 gthread-2.0)

AC_ARG_ENABLE(man,
              [AS_HELP_STRING([--enable-man],
                              [generate man pages [default=auto]])],,
              enable_man=maybe)
AS_IF([test "$enable_man" != no], [
  AC_PATH_PROG([XSLTPROC], [xsltproc])
  AS_IF([test -z "$XSLTPROC"], [
    AS_IF([test "$enable_man" = yes], [
      AC_MSG_ERROR([xsltproc is required for --enable-man])
    ])
    enable_man=no
  ])
])
AM_CONDITIONAL(ENABLE_MAN, test "$enable_man" != no)

AC_ARG_ENABLE(schemas,
              [AS_HELP_STRING([--enable-schemas],
                              [generate schemas [default=auto]])],,
              enable_schemas=maybe)
AS_IF([test "$enable_schemas" != no], [
  AC_PATH_PROG([TRANG], [trang])
  AS_IF([test -z "$TRANG"], [
    AS_IF([test "$enable_schemas" = yes], [
      AC_MSG_ERROR([trang is required for --enable-schemas])
    ])
    enable_schemas=no
  ])
  AM_PATH_PYTHON(,[
    AC_MSG_CHECKING([for lxml])
    AS_IF([eval test -d $pythondir/lxml], [
      AC_MSG_RESULT([yes])
    ], [
      AC_MSG_RESULT([no])
      AS_IF([test "$enable_schemas" = yes], [
        AC_MSG_ERROR([lxml $pythondir is required for --enable-schemas])
      ])
      enable_schemas=no
    ])
  ], [
    AS_IF([test "$enable_schemas" = yes], [
      AC_MSG_ERROR([python is required for --enable-schemas])
      enable_schemas=no
    ])
  ])
])
AM_CONDITIONAL(ENABLE_SCHEMAS, test "$enable_schemas" != no)

dnl ---------------------------------------------------------------------------
dnl - Makefiles, etc.
dnl ---------------------------------------------------------------------------
AC_CONFIG_FILES([
Makefile
po/Makefile.in
src/Makefile
data/Makefile
])
AC_OUTPUT

dnl ==========================================================================
echo "
                    appdata-tools $VERSION
                  =======================

        prefix:                    ${prefix}
        datadir:                   ${datadir}
        compiler:                  ${CC}
        cflags:                    ${CFLAGS}
        cppflags:                  ${CPPFLAGS}
"

