# Repository Guidelines

## Project Structure & Module Organization

This repository contains ESPHome YAML for Deye hybrid inverters. Reusable configuration lives in `pv_inverter/`: family entry points select packages from `pv_inverter/packages/deye_hybrid_1p/` or `deye_hybrid_3p/`. Keep domain-specific entities in files such as `battery.yaml`, `grid.yaml`, and `tou.yaml`. Root `pv-inverter*.yaml` files are user-facing examples. Board-specific and factory-flash configurations belong under `devices/<board>/`. Documentation is in `docs/`, while `tests/` contains compile fixtures for the supported 1P LV, 3P LV, and 3P HV families.

## Build, Test, and Development Commands

Install the CLI with `pip install esphome` (the repository pins a Python version in `.python-version`). Then mirror CI locally:

```bash
cp tests/*.yaml .
esphome config deye_hybrid_1p_lv.yaml
esphome compile deye_hybrid_1p_lv.yaml
```

`config` performs a fast validation; `compile` builds firmware. Repeat the compile for `deye_hybrid_3p_lv.yaml` and `deye_hybrid_3p_hv.yaml`. CI builds all three against ESPHome stable, beta, and dev. To flash a configured device, run `esphome run pv-inverter.yaml`.

## Coding Style & Naming Conventions

Use two-space YAML indentation and follow the ordering and quoting style of adjacent entities. Use `snake_case` for substitutions and IDs, lowercase family paths (`deye_hybrid_3p`), and descriptive Home Assistant display names. Preserve register metadata together: `address`, `value_type`, scaling filters, unit, device class, and state class. Changes to shared 3P packages must be checked against both LV and HV fixtures. Retain the Apache license header in new configuration files.

## Testing Guidelines

There is no unit-test framework or coverage target; successful ESPHome validation and compilation are the acceptance checks. Add or update a fixture in `tests/` when introducing a new inverter family. Never make tests depend on real credentials or attached hardware.

## Commit & Pull Request Guidelines

History generally follows Conventional Commit-style subjects: `feat(3P): ...`, `fix(1P): ...`, `docs: ...`, `ci: ...`, and `release: ...`. Keep subjects imperative and scoped when relevant. PRs should describe behavior and hardware impact, identify the affected family using the template checkboxes, link related issues, document register sources or assumptions, and report which fixtures and ESPHome versions were compiled. Include screenshots only for documentation or Home Assistant UI changes.

## Security & Configuration

Do not commit Wi-Fi credentials, API keys, OTA passwords, inverter serials, or logs containing secrets. Keep local values in ignored `secrets.yaml`; `tests/secrets.yaml` must contain placeholders only.
