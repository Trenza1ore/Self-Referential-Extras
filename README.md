# Self-Referential Extras

> Created to support [pypa/packaging.python.org#2087](https://github.com/pypa/packaging.python.org/pull/2087)

CI matrix for checking package managers' support for [self-referential extras](https://pip.pypa.io/en/latest/reference/requirement-specifiers/#self-referential-extras): optional dependencies that refer back to the same package with other extras. Workflows run weekly with the latest release version of each package manager.

## Current status

> Conda does not support extras before [CEP 44](https://github.com/conda/ceps/blob/main/cep-0044.md) implementation

[![pip](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/pip.yml/badge.svg)](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/pip.yml)
[![uv](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/uv.yml/badge.svg)](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/uv.yml)
[![poetry](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/poetry.yml/badge.svg)](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/poetry.yml)
[![pdm](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/pdm.yml/badge.svg)](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/pdm.yml)
[![pipenv](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/pipenv.yml/badge.svg)](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/pipenv.yml)
[![hatch](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/hatch.yml/badge.svg)](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/hatch.yml)
[![whl2conda](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/whl2conda.yml/badge.svg)](https://github.com/Trenza1ore/Self-Referential-Extras/actions/workflows/whl2conda.yml)

## Commands for the test cases

| Tool | `add` | `install` | `sync` |
|------|-------|-----------|--------|
| pip | - | `python -m pip install "${WHEEL}[all]"` | - |
| uv | `uv add "${WHEEL}[all]"` | `uv pip install --system "${WHEEL}[all]"` | `uv lock`<br>`uv sync` |
| poetry | `poetry add "${WHEEL}[all]"` | - | `poetry lock`<br>`poetry sync --no-root` |
| pdm | `pdm add "${WHEEL}[all]"` | - | `pdm lock`<br>`pdm sync` |
| pipenv | - | `pipenv install "${WHEEL}[all]"` | `pipenv lock`<br>`pipenv sync` |
| hatch | - | - | `hatch env lock`<br>`hatch dep sync` |
| whl2conda | - | `whl2conda convert "$WHEEL" --resolve-extras`<br>`whl2conda install "$CONDA_PKG" --create -n self_ref_extras --yes` | - |

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

## When was this supported?

> This part may contain errors, it's kinda like my personal notes~

| Tool      | Version   | Release Date | Related PRs     |
|-----------|-----------|--------------|-----------------|
| pip       |      21.2 |      2021.07 | Not Intentional |
| pipenv    | 2022.8.15 |      2022.08 | Not Intentional |
| pdm       |     2.3.1 |      2022.12 | [#1481](https://github.com/pdm-project/pdm/pull/1481), [#1541](https://github.com/pdm-project/pdm/issues/1541) |
| uv        |    0.5.27 |      2025.02 | [#11142](https://github.com/astral-sh/uv/pull/11142) |
| poetry    |     2.1.0 |      2025.02 | [#10106](https://github.com/python-poetry/poetry/pull/10106) |
| hatch     |    1.16.3 |      2026.01 | [#2127](https://github.com/pypa/hatch/pull/2127) |
| whl2conda |         - |            - | -               |
