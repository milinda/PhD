
.PHONY: samza-sql-milinda.pdf all clean

all: samza-sql-milinda.pdf

%.tex: %.raw
	./raw2tex $< > $@

%.tex: %.dat
	./dat2tex $< > $@

samza-sql-milinda.pdf: samza-sql-milinda.tex
	latexmk -pdf -pdflatex="pdflatex -interactive=nonstopmode" -use-make samza-sql-milinda.tex

clean:
	latexmk -CA
