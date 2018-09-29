default: html slides

html:
	docker run -it \
		-u $(shell id -u):$(shell id -g) \
		-v $(shell pwd):/documents/ \
		asciidoctor/docker-asciidoctor \
		asciidoctor -D docs/html/ index.adoc 

slides:
	docker run -it --rm \
		-u $(shell id -u):$(shell id -g) \
		-v $(shell pwd):/documents/ \
		asciidoctor/docker-asciidoctor \
		asciidoctor-revealjs \
			-a "revealjsdir=https://cdnjs.cloudflare.com/ajax/libs/reveal.js/3.6.0" \
			-D docs/slides/ \
			index.adoc 

presentation_mode: slides
	# Clean old container
	docker rm -f rust-slides > /dev/null 2>&1 || true
	# Serve the presentation from an HTTP Server
	docker run -d --rm \
		-v $(shell pwd)/docs/slides:/usr/share/nginx/html \
		-p 4987:80 \
		--name=rust-slides \
		nginx
	# Open the presentation
	firefox http://localhost:4987