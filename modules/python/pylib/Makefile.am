EXTRA_DIST += \
	modules/python/pylib/syslogng/__init__.py				\
	modules/python/pylib/syslogng/dest.py					\
	modules/python/pylib/syslogng/logger.py					\
	modules/python/pylib/syslogng/message.py				\
	modules/python/pylib/syslogng/parser.py					\
	modules/python/pylib/syslogng/persist.py				\
	modules/python/pylib/syslogng/source.py					\
	modules/python/pylib/syslogng/template.py				\
	modules/python/pylib/syslogng/debuggercli/__init__.py			\
	modules/python/pylib/syslogng/debuggercli/choicecompleter.py		\
	modules/python/pylib/syslogng/debuggercli/commandlinelexer.py		\
	modules/python/pylib/syslogng/debuggercli/completer.py			\
	modules/python/pylib/syslogng/debuggercli/completerlang.py		\
	modules/python/pylib/syslogng/debuggercli/debuglang.py			\
	modules/python/pylib/syslogng/debuggercli/debuggercli.py		\
	modules/python/pylib/syslogng/debuggercli/editline.py \
	modules/python/pylib/syslogng/debuggercli/getoptlexer.py		\
	modules/python/pylib/syslogng/debuggercli/langcompleter.py		\
	modules/python/pylib/syslogng/debuggercli/lexer.py			\
	modules/python/pylib/syslogng/debuggercli/lexertoken.py			\
	modules/python/pylib/syslogng/debuggercli/macrocompleter.py		\
	modules/python/pylib/syslogng/debuggercli/readline.py			\
	modules/python/pylib/syslogng/debuggercli/syslognginternals.py		\
	modules/python/pylib/syslogng/debuggercli/tablexer.py			\
	modules/python/pylib/syslogng/debuggercli/templatelang.py		\
	modules/python/pylib/syslogng/debuggercli/templatelexer.py		\
	modules/python/pylib/syslogng/debuggercli/tflang.py			\
	modules/python/pylib/syslogng/debuggercli/tests/__init__.py		\
	modules/python/pylib/syslogng/debuggercli/tests/test_completer.py    	\
	modules/python/pylib/syslogng/debuggercli/tests/test_commandlinelexer.py	\
	modules/python/pylib/syslogng/debuggercli/tests/test_completerlang.py	\
	modules/python/pylib/syslogng/debuggercli/tests/test_choicecompleter.py	\
	modules/python/pylib/syslogng/debuggercli/tests/test_debuglang.py	\
	modules/python/pylib/syslogng/debuggercli/tests/test_debuggercli.py	\
	modules/python/pylib/syslogng/debuggercli/tests/test_getoptlexer.py	\
	modules/python/pylib/syslogng/debuggercli/tests/test_langcompleter.py	\
	modules/python/pylib/syslogng/debuggercli/tests/test_lexer.py		\
	modules/python/pylib/syslogng/debuggercli/tests/test_macrocompleter.py	\
	modules/python/pylib/syslogng/debuggercli/tests/test_tablexer.py	\
	modules/python/pylib/syslogng/debuggercli/tests/test_templatelexer.py	\
	modules/python/pylib/syslogng/debuggercli/tests/test_templatelang.py	\
	modules/python/pylib/syslogng/debuggercli/tests/test_tflang.py		\
	modules/python/pylib/setup.py						\
	modules/python/pylib/pylintrc						\
	modules/python/pylib/test_pylib.sh	\
	modules/python/pylib/CMakeLists.txt

if ENABLE_PYTHON


PYLIB_PATH = modules/python/pylib
PYLIB_BUILDDIR = $(abs_builddir)/$(PYLIB_PATH)
PYLIB_SRCDIR = $(top_srcdir)/modules/python/pylib
SETUPPY_MANIFEST = $(PYLIB_BUILDDIR)/install-manifest.txt
PYTHON_ROOT = $(if $(DESTDIR),$(DESTDIR),/)

INSTALL_EXEC_HOOKS += install-pylib
UNINSTALL_HOOKS += uninstall-pylib
CLEAN_HOOKS += clean-pylib

PYSETUP_OPTIONS ?= --root="$(PYTHON_ROOT)" --install-lib="$(python_moduledir)"
IS_PYTEST_INSTALLED := $(shell ${PYTHON} -m pytest --version > /dev/null 2>&1 ; echo $$?)

install-pylib:
	(cd $(PYLIB_SRCDIR) && $(PYTHON) setup.py \
		build --build-base="$(PYLIB_BUILDDIR)/build" \
		install --record=$(SETUPPY_MANIFEST) ${PYSETUP_OPTIONS})

uninstall-pylib:
	sed -e 's,^,$(PYTHON_ROOT),g' $(SETUPPY_MANIFEST) | tr '\n' '\0' | xargs -0 rm -f

clean-pylib:
	rm -rf "$(PYLIB_BUILDDIR)/build"
	rm -rf "$(SETUPPY_MANIFEST)"

python-checks: python-unit python-pep8 python-pylint

python-unit:
	@if [ $(IS_PYTEST_INSTALLED) -eq 0 ]; then \
			echo "pytest is installed"; \
			${PYTHON} -m pytest --showlocals --verbosity=3 $(PYLIB_SRCDIR)/syslogng; \
	else \
			echo "pytest is not installed, can not run tests"; \
	fi;

python-pep8:
	pep8 --ignore=E501 $(PYLIB_SRCDIR)/syslogng

if PYLINT
python-pylint:
	@PYLINT@ -r n --rcfile=$(PYLIB_SRCDIR)/pylintrc $(PYLIB_SRCDIR)/syslogng
else
python-pylint:
	$(error "missing pylint")
endif


modules_python_pylib_tests_TESTS = modules/python/pylib/test_pylib.sh
check_SCRIPTS += $(modules_python_pylib_tests_TESTS)

endif

.PHONY: install-pylib
.PHONY: uninstall-pylib
.PHONY: clean-pylib
.PHONY: python-checks python-unit python-pep8 python-pylint
