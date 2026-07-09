# spack-config-test

Use `common.yaml` as a remote Spack include and control behavior with environment variables.

## Usage

In your `spack.yaml`:

```yaml
spack:
  include:
  - path: https://raw.githubusercontent.com/AlexanderRichert-NOAA/spack-config-test/<commit>/common.yaml
    sha256: <sha256>
```

Set environment variables as needed before concretization & installation:

```bash
export SITE=acorn
export COMPILER=oneapi@2024.2.1
export USE_SPACK_STACK_VERSIONS=YES
```

## Variables

`common.yaml` currently checks these variables:

| Variable | Allowed values used in `common.yaml` | Effect |
|---|---|---|
| `USE_SPACK_STACK_VERSIONS` | `YES` | Includes spack-stack package settings, including site-specific  `packages.yaml`. |
| `SITE` | `acorn`, `ursa` | Selects which spack-stack site to get configurations for. |
| `COMPILER` | `intel@19.1.3.304`, `oneapi@2024.2.1`, `oneapi@2025.3.1` | Selects compiler-specific include files for matching site rules. |

Default behavior for all variables is to not match the relevant include's.

### Rule summary

Here are some example variable configurations and their effects on included files:

| $SITE | $COMPILER | $USE_SPACK_STACK_VERSIONS | **Included config files from spack-stack** |
|---|---|---|---|
| unset | unset | `YES` | common `packages.yaml` |
| `acorn` | unset | unset | Acorn `packages.yaml` |
| `ursa` | unset | `YES` | common `packages.yaml` <br> Ursa `packages.yaml` |
| `ursa` | `oneapi@2025.3.1` | unset | Ursa `packages_oneapi-2025.3.1-hpcx.yaml` |
| `ursa` | `oneapi@2025.3.1` | `YES` | common `packages.yaml` <br> Ursa `packages.yaml` <br> Ursa `packages_oneapi-2025.3.1-hpcx.yaml` |
