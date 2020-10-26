pandoc_args := -t revealjs -V revealjs-url=./reveal.js -V theme=solarised
pandoc_args += -f markdown+fenced_divs

.PHONY: serve site

site: index.html

serve: index.html
	python -m http.server

index.html: presentation.md
	pandoc $(pandoc_args) $< -o $@
