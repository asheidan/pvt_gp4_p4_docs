SOURCE ?= plan.tex
TARGET ?= $(SOURCE:.tex=.pdf)

VIEW	 := osascript -e "tell application \"Skim\" to revert document \"$(TARGET)\"" || open -a Skim "$(TARGET)"

default: view


### Checking for dependencies ################################################
ifneq ($(strip $(shell grep '^[^%]*\\tableofcontents' $(SOURCE))),)
DEPS += %.toc
endif

ifneq ($(strip $(shell grep '^[^%]*\\listoffigures' $(SOURCE))),)
DEPS += %.lof
endif

ifneq ($(strip $(shell grep '^[^%]*\\printindex' $(SOURCE))),)
DEPS += %.ind
endif

ifneq ($(strip $(shell grep '^[^%]*\\printglossary' $(SOURCE))),)
DEPS += %.gls
endif

ifneq ($(strip $(shell grep '^[^%]*\\printnomenclature' $(SOURCE))),)
DEPS += %.nls
endif

ifneq ($(strip $(shell grep '^[^%]*\\bibliography{[^}]*}' $(SOURCE))),)
DEPS += %.bbl
BIBDB	:= $(strip $(shell grep '^[^%]*\\bibliography{[^}]*}' $(SOURCE) | sed 's/.*{\(.*\).*}/\1/' ))
ifeq ($(filter %.bib,$(BIBDB)),)
BIBDB :=$(BIBDB).bib
endif
endif

### Rules ####################################################################
# Remove default rule
%.dvi : %.tex

test:
	@echo $(TARGET) depends on $(DEPS)

view: $(TARGET)
	$(VIEW)

%.aux %.idx %.lof %.lot %.nlo %.glo %.toc: %.tex | $(INCLUDES) $(filter %.eps,$(FIGURES))
	pdflatex $<

%.ind: %.idx
	makeindex $<

%.bbl: %.aux $(BIBDB)
	bibtex $<

%.gls: %.glo
	makeindex $< -s nomencl.ist -o $@

%.nls: %.nlo
	makeindex $< -s nomencl.ist -o $@

%.pdf: %.tex $(DEPS)
	pdflatex $<

%.dvi: %.tex $(DEPS)
	pdflatex $<

clean:
	-rm -f *.aux *.bbl *.blg *.glo *.gls *.idx *.ilg *.ind *.lof *.log *.out *.toc

clean_all: clean
	-rm -f *.pdf *.dvi

.PHONY: default clean clean_all view
.SECONDARY:
