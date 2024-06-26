# Makefile to run latex, dvips, and pdflatex on the thesis guide
# Can also run feynmf/feynmp/tikz on files in a directory

GUIDE = thesis_guide
EXTRACMD = -shell-escape
FEYNDIR = ../feynmf
FEYNFILES = $(wildcard $(FEYNDIR)/*.tex)
TIKZDIR = ../tikz
TIKZFILES = $(wildcard $(TIKZDIR)/*.tex)
PYFEYNDIR = ../pyfeyn
PYFEYNFILES = $(wildcard $(PYFEYNDIR)/*.py)
PYFEYNHANDDIR = ../pyfeynhand
PYFEYNHANDFILES = $(wildcard $(PYFEYNHANDDIR)/*.py)
ifdef file
FEYNFILES = $(FEYNDIR)/$(file).tex
TIKZFILES = $(TIKZDIR)/$(file).tex
PYFEYNFILES = $(PYFEYNDIR)/$(file).py
PYFEYNHANDFILES = $(PYFEYNHANDDIR)/$(file).py
endif
AWKDIR=..

.PHONY: guide11 phdsubmit \
	feynmf feynmp tikz pyfeyn pyfeynhand \
	cleanguide \
	cleanfeynmf cleanfeynmp cleantikz cleanpyfeyn cleanpyfeynhand cleanpictpdf \
	cleanblx cleanbbl \
	cleanglo cleanphd \
	help test

guide: run_latexmk

guidexe: run_xelatexmk

guidelua: run_lualatexmk

run_latexmk: *.tex
	latexmk -e '$$pdflatex=q/pdflatex %O -shell-escape %S/' -pdf $(GUIDE)
	# latexmk -pdf $(GUIDE)

run_lualatexmk: *.tex
	latexmk -e '$$lualatex=q/lualatex %O -shell-escape %S/' -lualatex $(GUIDE)
	# latexmk -lualatex $(GUIDE)

run_xelatexmk: *.tex
	latexmk -e '$$xelatex=q/xelatex %O -shell-escape %S/' -xelatex $(GUIDE)
	# latexmk -xelatex $(GUIDE)

guide11:
	pdflatex   $(EXTRACMD) $(GUIDE)
	biber      $(GUIDE)
	makeindex  $(GUIDE)
	makeglossaries $(GUIDE)
	pdflatex   $(EXTRACMD) $(GUIDE)
	pdflatex   $(GUIDE)

feynmf:
	make -f ../Makefile FEYNDIR=$(FEYNDIR) FEYNFILES="$(FEYNFILES)" \
	 AWKDIR=$(AWKDIR) feynmf

feynmp:
	make -f ../Makefile FEYNDIR=$(FEYNDIR) FEYNFILES="$(FEYNFILES)" \
	 AWKDIR=$(AWKDIR) feynmp

tikz:
	make -f ../Makefile TIKZDIR=$(TIKZDIR) TIKZFILES="$(TIKZFILES)" tikz

pyfeyn:
	make -f ../Makefile PYFEYNDIR=$(PYFEYNDIR) PYFEYNFILES="$(PYFEYNFILES)" pyfeyn

pyfeynhand:
	make -f ../Makefile PYFEYNHANDDIR=$(PYFEYNHANDDIR) PYFEYNHANDFILES="$(PYFEYNHANDFILES)" pyfeynhand

phdsubmit:
	pdflatex  PhD_submit
	pdflatex  PhD_submit

cleanall: clean cleanbbl

clean: cleanguide cleanphd cleanfeynmf cleanfeynmp cleanblx cleanglo

cleanguide:
	-rm $(GUIDE).log $(GUIDE).aux $(GUIDE).toc
	-rm $(GUIDE).lof $(GUIDE).lot $(GUIDE).out
	-rm $(GUIDE).blg $(GUIDE).bbl $(GUIDE).pdf
	-rm $(GUIDE)-blx.bib $(GUIDE).bcf $(GUIDE).run.xml
	-rm $(GUIDE).fdb_latexmk .$(GUIDE).lb $(GUIDE).synctex.gz
	-rm $(GUIDE).ind $(GUIDE).idx $(GUIDE).ilg
	-rm $(GUIDE).acn $(GUIDE).acr $(GUIDE).alg
	-rm $(GUIDE).glg $(GUIDE).glo $(GUIDE).gls
	-rm $(GUIDE).ist $(GUIDE).fls $(GUIDE).xdv $(GUIDE).wrt $(GUIDE).listing
	-rm $(GUIDE).listing $(GUIDE).wrt
	-rm $(GUIDE).tex.bak
	-rm *.aux

cleanfeynmf:
	make -f ../Makefile cleanfeynmf FEYNDIR=$(FEYNDIR)

cleanfeynmp:
	-rm *.1 *.mp
	make -f ../Makefile cleanfeynmp FEYNDIR=$(FEYNDIR)

cleantikz:
	make -f ../Makefile cleantikz TIKZDIR=$(TIKZDIR)

cleanpyfeyn:
	make -f ../Makefile cleanpyfeyn PYFEYNDIR=$(PYFEYNDIR)

cleanpyfeynhand:
	make -f ../Makefile cleanpyfeynhand PYFEYNHANDDIR=$(PYFEYNHANDDIR)

cleanpictpdf:
	make -f ../Makefile cleanpictpdf FEYNDIR=$(FEYNDIR) TIKZDIR=$(TIKZDIR) \
		PYFEYNHANDDIR=$(PYFEYNHANDDIR)

cleanblx:
	-rm *-blx.bib
	-rm *.bcf
	-rm *.run.xml

cleanbbl:
	-rm *.bbl

cleanglo:
	-rm *.acn *.acr *.alg
	-rm *.glg *.glo *.gls
	-rm *.ist

cleanphd:
	-rm PhD_submit.log PhD_submit.aux PhD_submit.out
	-rm PhD_submit.pdf

help:
	@echo "Possible commands:"
	@echo "guide: Compile thesis guide using latexmk"
	@echo "guidexe: Compile thesis guide using xelatex and latexmk"
	@echo "guidelua: Compile thesis guide using lualatex and latexmk"
	@echo "guide11: Compile thesis guide using pdflatex and without using latexmk"
	@echo "feynmf: Run feynmf for all .tex files in $(FEYNDIR)"
	@echo "feynmp: Run feynmp for all .tex files in $(FEYNDIR)"
	@echo "tikz:   Run tikz   for all .tex files in $(TIKZDIR)"
	@echo "pyfeyn: Make Feynman graphs with PyFeyn in $(PYFEYNDIR)"
	@echo "pyfeynhand: Make Feynman grpahs with PyFeynHand in $(PYFEYNHANDDIR)"
	@echo "phdsubmit: Summary of PhD submission instructions"
	@echo "cleanall: Thorough cleanup"
	@echo "clean: Thorough cleanup except for bbl file"
	@echo "cleanguide:   Clean up guide  LaTeX output files"
	@echo "cleanfeynmf:  Clean up feynmf output files"
	@echo "cleanfeynmp:  Clean up feynmp output files"
	@echo "cleantikz:    Clean up tikz output files"
	@echo "cleanpyfeyn:  Clean up pyfeyn output files"
	@echo "cleanpyfeynhand: Clean up pyfeynhnad output files"
	@echo "cleanpictpdf: Clean up tikz, feynmp and pyfeynhand output PDF files"

test:
	@echo "Guide $(GUIDE)"
	@echo "Feynmf Feynman graphs dir: $(FEYNDIR)"
	@echo "TikZ   Feynman graphs dir: $(TIKZDIR)"
	@echo "PyFeyn Feynman graphs dir: $(PYFEYNDIR)"
	@echo "PyFeynHand Feynman graphs dir: $(PYFEYNHANDDIR)"
