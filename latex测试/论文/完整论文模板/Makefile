LATEX = latex
BIBTEX = bibtex
L2H = latex2html
PDFLATEX = ps2pdf
DVIPS = dvips -t a4  #letter
DVIPDFM = dvipdfm

RERUN = "(There were undefined references|Rerun to get (cross-references|the bars) right)"
RERUNBIB = "No file.*\.bbl|Citation.*undefined"

FILENAME = template
GOALS = $(FILENAME).pdf
#GOALS = $(FILENAME).pdf
DVIFILES = $(FILENAME).dvi

COPY = if test -r $*.toc; then cp $*.toc $*.toc.bak; fi
RM = rm -f

all:$(GOALS)  main

main:$(DVIFILES)

%.dvi:%.tex
	$(COPY);$(LATEX) $<
	egrep -c $(RERUNBIB) $*.log && ($(BIBTEX) $*;$(COPY);$(LATEX) $<) ;true
	egrep $(RERUN) $*.log && ($(COPY);$(LATEX) $<) ; true
	egrep $(RERUN) $*.log && ($(COPY);$(LATEX) $<) ; true
	if cmp -s $*.toc $*.toc.bak; then . ;else $(LATEX) $< ; fi
	$(RM) $*.toc.bak
# Display relevant warnings
	egrep -i "(Reference|Citation).*undefined" $*.log ; true

%.ps:%.dvi
	$(DVIPS) $< -o $@

%.pdf:%.dvi
	$(DVIPDFM) $<

%.pdf:%.ps
	$(PDFLATEX) $<

%.html:%.tex
	$(L2H) $<

clean:
	-rm -f *.aux *.log *.bbl *.blg *.brf *.cb *.ind *.idx *.ilg  \
	*.inx *.ps *.dvi *.pdf *.toc *.out

