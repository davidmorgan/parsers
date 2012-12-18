PAGES=out/userguide.html out/dartdoc

.PHONY: all clean out gen

all: out gen  $(PAGES)

gen:
	mkdir -p gen

out:
	mkdir -p out

clean:
	rm -rf out gen

out/images: images/*
	cp -r images out
	touch out/images

out/style.css: style.css
	cp style.css $@

out/pygments.css:
	pygmentize -S default -f html > $@

gen/filter: filter.hs
	ghc --make $< -outputdir gen -o $@

out/%.html: %.md out/style.css out/pygments.css gen/filter out/images
	pandoc -S -t json $< | gen/filter | pandoc -f json -s -c style.css -c pygments.css > $@

gen/parsers:
	mkdir -p gen/parsers
	wget -P gen http://commondatastorage.googleapis.com/pub.dartlang.org/packages/parsers-0.9.1.tar.gz
	tar xvzf gen/parsers-0.9.1.tar.gz --directory=gen/parsers

gen/parsers/packages: gen/parsers
	cd gen/parsers; pub install

out/dartdoc: gen/parsers/packages gen/parsers
	dartdoc --out=out/dartdoc --pkg=gen/parsers/packages/ --link-api gen/parsers/lib/parsers.dart
