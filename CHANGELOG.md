## Release 1.1.0

### Enhancements

Updated .gitignore ([PR #13](https://github.com/CaptainArbitrary/RimWorldMod/pull/13))

- https://www.toptal.com/developers/gitignore/api/macos,rider,visualstudio

### Bug Fixes

Update CI workflow concurrency settings ([PR #12](https://github.com/CaptainArbitrary/RimWorldMod/pull/12))

- Set `cancel-in-progress` to false in the CI workflow concurrency settings.

Fix assembly incrementing in make-release.yml ([PR #14](https://github.com/CaptainArbitrary/RimWorldMod/pull/14))

- Add git add command to the make-release workflow

Update copyright holder in LICENSE file ([PR #15](https://github.com/CaptainArbitrary/RimWorldMod/pull/15))

- Update copyright holder from "CaptainArbitrary" to "Jeffery Harrell" in the LICENSE file.

Fix pull request creation command in CD workflow ([PR #19](https://github.com/CaptainArbitrary/RimWorldMod/pull/19))

- Removed the `--body` flag from the `gh pr create` command
- The `--body` flag was causing an error and is not necessary for creating a pull request
- Updated the command to only include required flags: `--title`, `--base`, `--head`, and `--label`
- This change ensures that pull requests are created correctly during the CD process

Fix pull request creation command in CD workflow ([PR #20](https://github.com/CaptainArbitrary/RimWorldMod/pull/20))

- Added an empty body parameter to the command

