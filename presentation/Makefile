ROOTNAME=presentation

GENERATED = generated
MAXPAGES = 20

PGFSETUP = "\def\pgf{}\def\generated{$(GENERATED)}\input{$(ROOTNAME).tex}"
NONPGFSETUP = "\def\generated{$(GENERATED)}\input{$(ROOTNAME).tex}"

dirs :=  figures
figureTeXFiles := $(foreach dir,$(dirs),$(shell find $(dir)/ -type f -name '*.tex'))
figurePDFs = $(figureTeXFiles:%.tex=$(GENERATED)/%-external.pdf)

pdf $(ROOTNAME).pdf : $(ROOTNAME).tex $(figurePDFs)
	pdflatex $(NONPGFSETUP); \
	pdflatex $(NONPGFSETUP);
	ln -sf $(ROOTNAME).pdf pdf

clean : 
	rm -f $(ROOTNAME).ps $(ROOTNAME).4.ps $(ROOTNAME).ps.? $(ROOTNAME).ps.gz $(ROOTNAME).dvi $(ROOTNAME).pdf $(ROOTNAME).log $(ROOTNAME).txt $(ROOTNAME).bbl $(ROOTNAME).lot $(ROOTNAME).lof $(ROOTNAME).blg $(ROOTNAME).toc $(ROOTNAME).aux ps pdf dvi $(ROOTNAME).zip 

$(GENERATED)/%-external.pdf: %.tex
	mkdir -p $(GENERATED)/$(dir $(<:%-external=%))
	pdflatex --jobname $(GENERATED)/$(<:%.tex=%)-external $(PGFSETUP); 

figures: createDummies $(figureTeXFiles:%.tex=%-figure) $(figureTeXFiles)
	pdflatex $(NONPGFSETUP);

%-figure: %.tex
	mkdir -p $(GENERATED)/$(dir $(<:%-figure=%))
	pdflatex --jobname $(GENERATED)/$(<:%.tex=%)-external $(PGFSETUP); 

createDummies: dummy.pdf $(figureTeXFiles:%.tex=%-dummy)
	pdflatex $(NONPGFSETUP);

dummy.pdf:
	pdflatex --jobname=dummy "\documentclass{article}\usepackage{pgffor}\begin{document}\foreach \p in {0,..,$(MAXPAGES)}{dummy\newpage}\end{document}"
	rm dummy.log dummy.aux

%-dummy: %.tex
	mkdir -p $(GENERATED)/$(dir $(<:%.tex=%))
	cp dummy.pdf $(<:%.tex=$(GENERATED)/%-external.pdf) 


cleanFigures:
	rm -f $(figureTeXFiles:%.tex=$(GENERATED)/%-external*)

FIGURENAME=

figure: $(FIGURENAME)-dummy 
	pdflatex $(NONPGFSETUP);
	pdflatex --jobname $(GENERATED)/$(FIGURENAME)-external $(PGFSETUP); 
	pdflatex $(NONPGFSETUP);

tikz:
	pdflatex $(PGFSETUP); \
	pdflatex $(PGFSETUP) 
	ln -sf $(ROOTNAME).pdf pdf;

