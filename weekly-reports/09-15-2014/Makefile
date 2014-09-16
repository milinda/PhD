.PHONY: main.pdf all clean

all: update.pdf

update.pdf: update.tex
	latexmk update.tex -pdf -pdflatex="pdflatex -interaction=nonstopmode"

clean:
	latexmk -CA
