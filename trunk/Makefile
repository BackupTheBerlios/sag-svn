docs = sag.sgml
figs = overview-kernel.fig backup-timeline.fig fstree.fig hd-layout.fig \
	hd-mount-mounted.fig hd-mount-separate.fig hd-schematic.fig \
	logins-via-terminals.fig

# You should not need to touch anything below this.
JADE = openjade
JADETEX = jadetex
HTML_DSL = stylesheet.dsl
HTML_DCL = /usr/share/sgml/openjade/xml.dcl
TEX_DSL = $(HTML_DSL)

.sgml.html:
	rm -rf $@
	mkdir $@
	sed 's/graphic fileref="[^"]*/&.png/' $< > temp.sgml
	cp gwiazda.png $@
	(set -e; cd $@ && SP_ENCODING=ISO-8859-2 $(JADE) -t sgml -i output-html \
		-d ../$(HTML_DSL) $(HTML_DCL) ../temp.sgml; ../logo-add.pl *.html; chmod a+r *.* ) || rm -rf $@
	rm -f temp.sgml

.sgml.rtf:
	sed 's/graphic fileref="[^"]*/&.png/' $< > temp.sgml
	cp gwiazda.png $@
	SP_ENCODING=ISO-8859-2 $(JADE) -t rtf -i output-print \
		-d $(HTML_DSL) $(HTML_DCL) temp.sgml
	rm -f temp.sgml
	mv temp.rtf sag.rtf

.sgml.ps:
	sed 's/graphic fileref="[^"]*/&.ps/' sag.sgml > temp.sgml
	$(JADE) -t tex -d $(TEX_DSL) temp.sgml
	$(JADETEX) temp.tex
	$(JADETEX) temp.tex
	$(JADETEX) temp.tex
	dvips -o temp.ps temp.dvi
	rm -f temp.aux temp.log temp.dvi temp.tex temp.sgml
	mv temp.ps $*.ps

.fig.png:
	fig2dev -Lpng $< $@

.fig.ps:
	fig2dev -Lps $< $@

.SUFFIXES: $(SUFFIXES) .sgml .html .tex .ps .dvi .png .fig .rtf

docsps = $(docs:.sgml=.ps)
docshtml = $(docs:.sgml=.html)
docsrtf = $(docs:.sgml=.rtf)
docstex = $(docs:.sgml=.tex) $(docs:.sgml=.log) $(docs:.sgml=.dvi) \
	  $(docs:.sgml=.aux)

pngs = $(figs:.fig=.png) gwiazda.png
psfigs = $(figs:.fig=.ps) gwiazda.png

all: html rtf
html: $(pngs) $(docshtml)
	for dir in $(docshtml); do \
		cp $(pngs) *.css $$dir; \
	done

pkg-html: html
	tar czf sag.html{.tar.gz,}

rtf: $(pngs) $(docsrtf)

ps: $(psfigs) $(docsps)

sag-png.sgml: sag.sgml
	sed 's/graphic fileref="[^"]*/&.png/' sag.sgml > sag-png.sgml
sag-ps.sgml: sag.sgml
	sed 's/graphic fileref="[^"]*/&.ps/' sag.sgml > sag-ps.sgml

clean:
	rm -rf $(docsps) $(docshtml) $(docstex) $(pngs) $(psfigs)

mirror:
	rsync -vCr --delete --rsh "ssh -l paweln" sag.html/* shell.berlios.de:/home/groups/sag/htdocs/
