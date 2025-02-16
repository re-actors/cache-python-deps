# `re-actors/cache-python-deps@release/v1` — ABI-sensitive cache management

A GitHub Action maintaining caches dependent on Python runtime and ABI
stability.

The action is meant to be run instead of using the `cache` input of
`actions/setup-python`.


## Usage

To use the action add the following step to your workflow file (e.g.
`.github/workflows/ci-cd.yml`)

```yaml
- name: Restore pip cache
  uses: re-actors/cache-python-deps@release/v1
  with:
    cache-key-for-dependency-files: >-
      ${{
        hashFiles(
          '.pre-commit-config.yaml',
          'requirements/**',
          'tox.ini',
          'tox.toml',
          'pyproject.toml'
        )
      }}
```


## Options

### `cache-directory-lookup-command`

A command that prints cache directory to standard output. Defaults to
pip cache lookup.
**(DEFAULT: `python -Im pip cache dir`)**

### `cache-key-for-dependency-files`

A cache key string derived from the dependency declaration files. Should
be pre-computed by the caller. Something like `hashFiles()` should do.
**(REQUIRED)**


## Gotchas

The `cache-key-for-dependency-files` must be computed before calling the
action. It would typically depend on the contents of files containing
dependency declarations. This means that the dependency files should be
on disk before that (they can be retrieved by calling one of
[`re-actors/checkout-python-sdist`] or `actions/checkout`, for example).

In case the `cache-key-for-dependency-files` computation is done in an
early job in the workflow and the value is just being passed around, this
action does not necessarily have to be called after checking out the
repository. It should however still be invoked after `actions/setup-python`
so that the correct cache entry is picked up.


## Any users?

At the moment, it is used by [@aio-libs], [@CherryPy], with more to come.

[`re-actors/checkout-python-sdist`]:
https://github.com/marketplace/actions/checkout-python-sdist
[@aio-libs]: https://github.com/aio-libs
[@CherryPy]: https://github.com/cherrypy
