reveal_theme := simple
pandoc_args := -t revealjs -s -V revealjs-url=./reveal.js -V theme=$(reveal_theme)
pandoc_args += -f markdown+fenced_divs+link_attributes

.PHONY: serve site

site: index.html

serve: index.html
	python -m http.server

index.html: presentation.md Makefile
	pandoc $(pandoc_args) $< -o $@
