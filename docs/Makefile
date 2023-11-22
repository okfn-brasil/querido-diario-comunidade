# Minimal makefile for Sphinx documentation
#

# You can set these variables from the command line, and also
# from the environment for the first two.
SPHINXOPTS    ?=
SPHINXBUILD   ?= sphinx-build
SPHINXAUX     ?= sphinx-intl
SOURCEDIR     = source
BUILDDIR      = _local_build
POTDIR        = _build/gettext

# Put it first so that "make" without argument is like "make help".
help:
	@$(SPHINXBUILD) -M help "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O)

.PHONY: help Makefile

compile:
	pip-compile --upgrade --no-annotate --allow-unsafe requirements.in

env:
	python3 -m venv .venv

install:
	pip install -r requirements.txt

translatable:
	@$(SPHINXBUILD) -b gettext "$(SOURCEDIR)" "$(POTDIR)"

english:
	@$(SPHINXAUX) update -p "$(POTDIR)" -l en

build-english:
	@$(SPHINXBUILD) -b html -D language=en "$(SOURCEDIR)" "$(BUILDDIR)/en"

# Catch-all target: route all unknown targets to Sphinx using the new
# "make mode" option.  $(O) is meant as a shortcut for $(SPHINXOPTS).
%: Makefile
	@$(SPHINXBUILD) -M $@ "$(SOURCEDIR)" "$(BUILDDIR)/pt_BR" $(SPHINXOPTS) $(O)
