# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- The fields for Collections can also be used in Item properties
- `eodash:flatstyle` can be used at the top-level of Collections, in Item Asset Definitions, Asset Templates and Link Templates
- `eodash:flatstyle` can be provided as a style object in addition to a URL

### Changed
- `eox:colorlegend` renamed to `eodash:colorlegend`
- `eox:flatstyle` renamed to `eodash:flatstyle`
- Generalized the wording so that the extension can be implemented by any client, eodash being one implementation

### Removed
- `eodash:proj4_def` in favor of the fields of the Projection Extension (`proj:code`, `proj:wkt2`, `proj:projjson`)

### Fixed
- The JSON Schema validates the fields in Assets and rejects fields that are used in the wrong place
- The JSON Schema identifier matches the location where the schema is published

## [0.2.0](https://github.com/eodash/eodash-extension/tree/v0.2.0)

### Added
- `eodash:rasterform` to collections

### Changed

### Fixed

## [0.1.0](https://github.com/eodash/eodash-extension/tree/v0.1)

### Added
- Initial eodash STAC extension specification

### Changed

### Fixed

[Unreleased]: https://github.com/eodash/eodash-extension/compare/v0.2...main
