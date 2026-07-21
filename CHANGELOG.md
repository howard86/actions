# Changelog

## [2.0.0](https://github.com/howard86/actions/compare/v1.0.2...v2.0.0) (2026-07-21)


### ⚠ BREAKING CHANGES

* **ci:** consumers of ci.yml must add a `knip` script before bumping to v2, or CI fails with `Script not found "knip"`. Expect to tune knip.json first; the initial run on an established repo is noisy. Repos not ready stay pinned to v1.

### Features

* **ci:** require knip in the reusable CI workflow ([75c359d](https://github.com/howard86/actions/commit/75c359d920c158918a28babbcb381d4d74c8a903))

## [1.0.2](https://github.com/howard86/actions/compare/v1.0.1...v1.0.2) (2026-07-17)


### Bug Fixes

* **quality:** lint workflows without calling the GitHub API ([986ac8a](https://github.com/howard86/actions/commit/986ac8a437a2aa80d4e8fa8f5190c2fd71ae60da))
* **quality:** lint workflows without calling the GitHub API ([aa7a93c](https://github.com/howard86/actions/commit/aa7a93ca94a8a170c6b5e0ce7a3c88745fcb6522))


### Dependencies

* **deps:** bump the github-actions group across 2 directories with 1 update ([a4e4b6e](https://github.com/howard86/actions/commit/a4e4b6e22d952a8e3bae479520c08dc37c7687f5))
* **deps:** bump the github-actions group across 2 directories with 1 update ([4cd8cec](https://github.com/howard86/actions/commit/4cd8cecc275c403a98f019844a1c105d47708f42))

## [1.0.1](https://github.com/howard86/actions/compare/v1.0.0...v1.0.1) (2026-07-11)


### Performance Improvements

* **setup:** optional Bun/Turbo caches and stable turbo key ([03bd27f](https://github.com/howard86/actions/commit/03bd27f83a6bbdb0ca5ba697cfcc64354ba3d0b3))
* **setup:** optional caches and stable turbo key ([51ac1c0](https://github.com/howard86/actions/commit/51ac1c089b722054ae300c046408b87ad8ccf604))


### Dependencies

* **deps:** bump the github-actions group across 1 directory with 1 update ([bcfdf36](https://github.com/howard86/actions/commit/bcfdf36695cf6537ab5b3d0482075237766c16d2))
* **deps:** bump the github-actions group across 1 directory with 1 update ([65605b3](https://github.com/howard86/actions/commit/65605b33eb9d6354460a8052d2d5ae2d12eef202))
* **deps:** bump the github-actions group across 2 directories with 4 updates ([3b25f91](https://github.com/howard86/actions/commit/3b25f910df20f736e6258ef99a5eaaf27f283a60))
* **deps:** bump the github-actions group across 2 directories with 4 updates ([9b6ecfd](https://github.com/howard86/actions/commit/9b6ecfddb15f49812f5840097193b334790db87e))
* **deps:** bump the github-actions group across 3 directories with 2 updates ([7fc6efa](https://github.com/howard86/actions/commit/7fc6efa0e47977701c2ed7237e57cc5aa867eb79))
* **deps:** bump the github-actions group across 3 directories with 2 updates ([8c092b6](https://github.com/howard86/actions/commit/8c092b61af1b1fa7844d18382cfb4dffd6566e14))

## 1.0.0 (2026-05-21)


### Features

* add quality composite action with bundled configs ([87f42a7](https://github.com/howard86/actions/commit/87f42a7853ef559a44931451cfe0ac8b92099465))
* add reusable CI workflow ([03cbcd4](https://github.com/howard86/actions/commit/03cbcd41b2f7f8787613c17a7dd71e7e93747802))
* add setup composite action ([0fbbbf8](https://github.com/howard86/actions/commit/0fbbbf810b1bacd7a48003d96a06d487ab15fa17))
