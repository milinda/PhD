.PHONY: main.pdf all clean

all: main.pdf

main.pdf: main.tex
	latexmk main.tex -pdf -pdflatex="pdflatex -interaction=nonstopmode"

clean:
	latexmk -CA
