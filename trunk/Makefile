docs = sag.sgml
figs = overview-kernel.fig backup-timeline.fig fstree.fig hd-layout.fig \
	hd-mount-mounted.fig hd-mount-separate.fig hd-schematic.fig \
	logins-via-terminals.fig

# You should not need to touch anything below this.
JADE = openjade
JADETEX = jadetex
HTML_DSL = /usr/local/share/sgml/openjade/builtins.dsl
TEX_DSL = $(HTML_DSL)

.sgml.html:
	rm -rf $@
	mkdir $@
	sed 's/graphic fileref="[^"]*/&.png/' $< > temp.sgml
	cp gwiazda.png $@
	(set -e; cd $@ && $(JADE) -t sgml -d $(HTML_DSL) ../temp.sgml && \
		ln -s book1.html index.html) || rm -rf $@
	rm -f temp.sgml

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

.SUFFIXES: $(SUFFIXES) .sgml .html .tex .ps .dvi .png .fig

docsps = $(docs:.sgml=.ps)
docshtml = $(docs:.sgml=.html)
docstex = $(docs:.sgml=.tex) $(docs:.sgml=.log) $(docs:.sgml=.dvi) \
	  $(docs:.sgml=.aux)

pngs = $(figs:.fig=.png)
psfigs = $(figs:.fig=.ps)

all: html ps
html: $(pngs) $(docshtml)
	for dir in $(docshtml); do cp $(pngs) $$dir/.; done
ps: $(psfigs) $(docsps)

sag-png.sgml: sag.sgml
	sed 's/graphic fileref="[^"]*/&.png/' sag.sgml > sag-png.sgml
sag-ps.sgml: sag.sgml
	sed 's/graphic fileref="[^"]*/&.ps/' sag.sgml > sag-ps.sgml

clean:
	rm -rf $(docsps) $(docshtml) $(docstex) $(pngs) $(psfigs)
