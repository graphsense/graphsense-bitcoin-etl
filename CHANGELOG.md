# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

## [22.10] 2022-10-11
### Changed
- introduced --dry-run flag for coindesk and coinmarketcap rates ingest that does not write to cassandra but on the stdout
- introduced --abort-on-gaps that aborts the ingest if NaN values are found in the exchange rates. 

## [1.0.0] 2022-07-08
### Fixed
- Fixed `Dockerfile`
- Fixed dependencies in `requirements.txt`

## [0.5.2] 2022-03-16
### Changed
- Minor improvements

## [0.5.1] 2021-11-29
### Added
- Initial release
