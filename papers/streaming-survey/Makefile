.PHONY: paper.pdf all clean

all: paper.pdf

paper.pdf: streaming-survey.tex
	latexmk streaming-survey.tex -pdf -pdflatex="pdflatex -interaction=nonstopmode"

clean:
	latexmk -CA
