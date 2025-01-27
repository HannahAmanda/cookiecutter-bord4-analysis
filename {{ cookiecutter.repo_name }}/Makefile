.PHONY: lab docs build site deploy toc template new nvdb eda etl publish extra

#################################################################################
# GLOBALS                                                                       #
#################################################################################

PROJECT_DIR := $(shell dirname $(realpath $(lastword $(MAKEFILE_LIST))))
PROJECT_NAME = {{ cookiecutter.project_name }}
PYTHON_INTERPRETER = python

#################################################################################
# COMMANDS                                                                      #
#################################################################################

## Run Jupyter lab
lab:
	pipenv run jupyter lab

## serve the contents of docs
docs:
	pipenv run mkdocs serve

## build the contents of docs
build:
	make toc
	pipenv run mkdocs build

## build the contents of docs and test that it works before deploying
site:
	make build
	cd report_build; \
  	python -m SimpleHTTPServer 1234
	
## deploy the built docs to server
deploy:
	make build

## Create a TOC for the docs folder.
toc:
	pipenv run python src/scripts/populate_mkdocs.py

## Use a template from bord4-templates
template:
	pipenv run python src/scripts/download_templates.py

## Download scripts from the extra folder from the template repo
extra:
	pipenv run python src/scripts/download_extra.py

## Create new file in etl from generic-header
etl:
	@read -p "Enter file name for the new template:" filename; \
	curl -o etl/$$filename.ipynb;  https://raw.githubusercontent.com/BergensTidende/bord4-analysis-templates/master/templates/generic-header.ipynb \
	echo "Created etl/$$filename.ipynb"

## Create new file in eda from generic-header
eda:
	@read -p "Enter file name for the new template:" filename; \
	curl -o eda/$$filename.ipynb;  https://raw.githubusercontent.com/BergensTidende/bord4-analysis-templates/master/templates/generic-header.ipynb \
	echo "Created eda/$$filename.ipynb"

publish:
	@read -p "Enter file name for the new template:" filename; \
	curl -o publish/$$filename.ipynb;  https://raw.githubusercontent.com/BergensTidende/bord4-analysis-templates/master/templates/generic-header.ipynb \
	echo "Created pubish/$$filename.ipynb"

## Download the nvdb api code
nvdb:
	pipenv run python src/scripts/download_nvdb.py	


#################################################################################
# Self Documenting Commands                                                     #
#################################################################################

.DEFAULT_GOAL := help

# Inspired by <http://marmelab.com/blog/2016/02/29/auto-documented-makefile.html>
# sed script explained:
# /^##/:
# 	* save line in hold space
# 	* purge line
# 	* Loop:
# 		* append newline + line to hold space
# 		* go to next line
# 		* if line starts with doc comment, strip comment character off and loop
# 	* remove target prerequisites
# 	* append hold space (+ newline) to line
# 	* replace newline plus comments by `---`
# 	* print line
# Separate expressions are necessary because labels cannot be delimited by
# semicolon; see <http://stackoverflow.com/a/11799865/1968>
.PHONY: help
help:
	@echo "$$(tput bold)Available rules:$$(tput sgr0)"
	@echo
	@sed -n -e "/^## / { \
		h; \
		s/.*//; \
		:doc" \
		-e "H; \
		n; \
		s/^## //; \
		t doc" \
		-e "s/:.*//; \
		G; \
		s/\\n## /---/; \
		s/\\n/ /g; \
		p; \
	}" ${MAKEFILE_LIST} \
	| LC_ALL='C' sort --ignore-case \
	| awk -F '---' \
		-v ncol=$$(tput cols) \
		-v indent=19 \
		-v col_on="$$(tput setaf 6)" \
		-v col_off="$$(tput sgr0)" \
	'{ \
		printf "%s%*s%s ", col_on, -indent, $$1, col_off; \
		n = split($$2, words, " "); \
		line_length = ncol - indent; \
		for (i = 1; i <= n; i++) { \
			line_length -= length(words[i]) + 1; \
			if (line_length <= 0) { \
				line_length = ncol - indent - length(words[i]) - 1; \
				printf "\n%*s ", -indent, " "; \
			} \
			printf "%s ", words[i]; \
		} \
		printf "\n"; \
	}' \
	| more $(shell test $(shell uname) = Darwin && echo '--no-init --raw-control-chars')
