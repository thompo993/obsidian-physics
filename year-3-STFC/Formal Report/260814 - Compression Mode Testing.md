---
tags:
  - note
  - daq121
  - super-musr
created: 2026-08-14
---
# Links: 
[[260814 - Data Pipeline Meeting]]
# Notes:
We want to experiment with three different compression types, split across each of the three system config files. we will have some settings the same (`super_rt-1-1.json` will remain the same, with only compression mode changing in `sysconfig`.

| `sysconfig` file:        | compression: | emu.enable_pulse: | emu.enable: | emu.ch_map_mode: |
| ------------------------ | ------------ | ----------------- | ----------- | ---------------- |
| `sysconfig-super-rt-000` | lz4          | true              | true        | true             |
| `sysconfig-super-rt-001` | zstd         | true              | true        | true             |
| `sysconfig-super-rt-002` | none         | true              | true        | true             |



