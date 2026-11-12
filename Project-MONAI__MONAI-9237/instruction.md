# Issue

**Title:** Check nnUNet plans network names against allowed modules

## Description

### Description

`get_network_from_nnunet_plans()` and `ModelnnUNetWrapper` (used by `get_nnunet_monai_predictor()`) build the network from a bundle's `plans.json`. nnUNet resolves `network_class_name`, and every keyword argument listed in `_kw_requires_import`, by dotted name with `pydoc.locate` and then calls them. Nothing restricted which names a plans file could reference.

This PR adds a check that runs before nnUNet is called:
- `network_class_name` and each imported keyword argument must sit under an allowed module prefix, by default `DEFAULT_NNUNET_NETWORK_MODULES = ("dynamic_network_architectures", "torch.nn")`. The prefix is checked before anything is imported.
- If the name can be imported, it must be a `torch.nn.Module` subclass. Names that can't be imported are left to nnUNet's existing fallback, which only searches inside `dynamic_network_architectures`.
- A new `allowed_network_modules` argument on `get_network_from_nnunet_plans()`, `get_nnunet_monai_predictor()` and `ModelnnUNetWrapper` lets users load a trusted custom architecture.

Standard nnUNet plans (`PlainConvUNet`, `ResidualEncoderUNet`, with `torch.nn` conv, norm, dropout and nonlinearity ops) load as before.

### Tests
- New `tests/apps/nnunet/test_nnunet_bundle_network_spec.py`:
  - unit tests for the check: accepted defaults, rejected classes and imported arguments, names under an allowed prefix that are not `Module` classes, and custom allowed modules;
  - with `nnunetv2` installed, end-to-end tests of `get_network_from_nnunet_plans()` that build a real `PlainConvUNet` and check that a disallowed class or imported argument is rejected without being called.
- Run locally with nnunetv2 2.8.1 / torch 2.15 CPU: 10 passed. Without nnunetv2, the end-to-end class is skipped.

### Types of changes
- [x] Non-breaking change (fix or new feature that would not break existing functionality).
- [x] New tests added to cover the changes.
- [x] Documentation updated.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2c54111cbde1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.