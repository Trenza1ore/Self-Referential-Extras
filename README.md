# Self-Referential Extras

CI matrix for checking installer / dependency manager support for [self-referential extras](https://pip.pypa.io/en/latest/reference/requirement-specifiers/#self-referential-extras): optional dependencies that refer back to the same package with other extras. Workflows run weekly with the latest release version of each installer / dependency manager.

## Current Status

[![pip](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/pip.yml/badge.svg)](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/pip.yml)
[![uv](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/uv.yml/badge.svg)](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/uv.yml)
[![poetry](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/poetry.yml/badge.svg)](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/poetry.yml)
[![pdm](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/pdm.yml/badge.svg)](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/pdm.yml)
[![pipenv](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/pipenv.yml/badge.svg)](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/pipenv.yml)
[![hatch](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/hatch.yml/badge.svg)](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/hatch.yml)
[![whl2conda](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/whl2conda.yml/badge.svg)](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/whl2conda.yml)

## Commands for the Test Cases

> Conda does not support extras, so unless whl2conda provides a way to "bake in" extras during wheel conversion, it's guaranteed to fail.

| Tool | `add` | `install` | `sync` |
|------|-------|-----------|--------|
| pip | — | `python -m pip install "${WHEEL}[all]"` | — |
| uv | `uv add "${WHEEL}[all]"` | `uv pip install --system "${WHEEL}[all]"` | `uv lock`<br>`uv sync` |
| poetry | `poetry add "${WHEEL}[all]"` | — | `poetry lock`<br>`poetry sync --no-root` |
| pdm | `pdm add "${WHEEL}[all]"` | - | `pdm lock`<br>`pdm sync` |
| pipenv | — | `pipenv install "${WHEEL}[all]"` | `pipenv lock`<br>`pipenv sync` |
| hatch | — | — | `hatch env lock`<br>`hatch dep sync` |
| whl2conda | — | `whl2conda convert "$WHEEL" --resolve-extras`<br>`whl2conda install "$CONDA_PKG" --create -n self_ref_extras --yes` | — |

## Package extras

```toml
[project.optional-dependencies]
test = ["pytest"]
format = ["ruff"]
docs = ["sphinx"]
dev = [
  "self-referential-extras[test]",
  "self-referential-extras[format]",
]
all = ["self-referential-extras[test,format,docs]"]
```

Installing `self-referential-extras[all]` should pull in `pytest`, `ruff`, and `sphinx`.
