CORES := 16
FLAGS := --use-conda --use-singularity $(flags) -j $(CORES)
REPORT := report.html
PROFILE := slurm

all: install snakemake report

snakemake:
	poetry run snakemake $(FLAGS) -R $(rule)

cluster:
	poetry run snakemake --profile $(PROFILE) $(FLAGS) -R $(rule)

report:
	poetry run snakemake --report $(REPORT) $(FLAGS)

dry:
	poetry run snakemake -n $(FLAGS) $(REPORT) -R $(rule)

profile:
	@mkdir -p ~/.config/snakemake
	@cd ~/.config/snakemake
	cookiecutter https://github.com/Snakemake-Profiles/$(PROFILE).git

pre-commit: install-pre-commit
	poetry run pre-commit run --all-files

install: poetry install-pre-commit
	poetry install

install-pre-commit:
	poetry run pre-commit install

poetry:
ifeq (true,true)
	curl -sSL "https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py" | python
	@source $HOME/.poetry/env
else
	@echo "This is experimental and may not work yet"
	(Invoke-WebRequest -Uri https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py -UseBasicParsing).Content | python
endif

# Convenience aliases
i: install
pc: pre-commit
smk: snakemake
