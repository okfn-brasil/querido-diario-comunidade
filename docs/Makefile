compile: 
	pip-compile --allow-unsafe --no-annotate requirements.in

env:
	python3 -m venv .venv

build-br:
	cd pt_BR/; \
	make html
	
build-en:
	cd en/; \
	make html

build:
	make build-br; \
	make build-en;