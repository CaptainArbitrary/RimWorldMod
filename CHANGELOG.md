## Release 1.0.0-rc.1

### New Features

Add setup script to create variables and labels ([PR #1](https://github.com/CaptainArbitrary/RimWorldMod/pull/1))

- Add new file `setup-repo.sh`
- Set repository variables: `MOD_NAME`, `SLN_PATH`, `CSPROJ_PATH`, `ASSEMBLY_PATH`, `ZIP_CONTENTS`
- Create pull request type labels: "chore", "bug", "enhancement", "feature", "release"

Add PR label workflow to enforce required labels ([PR #2](https://github.com/CaptainArbitrary/RimWorldMod/pull/2))

- Added a new file `.github/workflows/pr-label.yml`
- Configured the workflow to trigger on pull request events
- Set up concurrency settings for the workflow
- Defined a job named `pr-label` that runs on Ubuntu latest
- Granted necessary permissions for the job to write pull requests
- Added a step that uses the `mheap/github-action-required-labels@v5` action
- Configured the action with required labels, count, and message options

Add About.xml file for RimWorld Mod ([PR #3](https://github.com/CaptainArbitrary/RimWorldMod/pull/3))

- Add About.xml file with mod metadata
- Include packageId, name, author, description, modVersion, url, and supportedVersions in the XML structure

Add RimWorldMod.cs, RimWorldMod.csproj, and RimWorldMod.sln ([PR #4](https://github.com/CaptainArbitrary/RimWorldMod/pull/4))

- Add new file RimWorldMod.sln with project reference to RimWorldMod.csproj
- Add new file RimWorldMod.csproj with target framework net472 and package reference Krafs.Rimworld.Ref version 1.4.3704
- Add new file RimWorldMod.cs with a static class containing a static constructor that logs "Hello World!"

Add CI workflow for building, testing, linting, and artifact upload ([PR #5](https://github.com/CaptainArbitrary/RimWorldMod/pull/5))

- Add new file `.github/workflows/ci.yml`
- Define workflow name as "ci"
- Trigger on push events excluding the `main` branch
- Set concurrency group as "ci-{branch_name}"
- Enable canceling in-progress jobs
- Create a job named "build" that runs on Ubuntu latest
- Checkout the repository using `actions/checkout@v3`
- Setup .NET 6.0.x using `actions/setup-dotnet@v3`
- Restore, build, and test the solution using dotnet commands
- Lint all XML files using libxml2-utils and xmllint
- Get short SHA of the commit and store it in an output variable
- Upload an artifact with a name based on module name and short SHA

Add make-release workflow ([PR #6](https://github.com/CaptainArbitrary/RimWorldMod/pull/6))

- Add make-release.yml file with necessary configurations for making a release
- Set up workflow_dispatch event to trigger the workflow manually
- Define input parameter "release_type" with options: patch, minor, major and default value as patch
- Configure concurrency settings to group workflows by github.ref and cancel in-progress workflows

Increment mod version

- Use Mudlet/xmlstarlet-action@v1.1 to update the modVersion in About/About.xml file with the incremented version
- Add About/About.xml file to git staging area
- Commit the change with a message indicating the incremented modVersion

Increment assembly version and build

- Use Mudlet/xmlstarlet-action@v1.1 to update the AssemblyVersion in CSPROJ_PATH file with the incremented version followed by .0
- Set up dotnet environment using actions/setup-dotnet@v3 with dotnet-version 6.0.x
- Restore and build the solution using dotnet restore and dotnet build commands respectively for Release configuration without restoring dependencies
- Add ASSEMBLY_PATH file to git staging area
- Commit the change with a message indicating the incremented assembly version

Update CHANGELOG.txt

- Use mikepenz/release-changelog-builder-action@v4 to generate changelog from increment-version.outputs.current-version tag to github.ref tag using .github/workflows/configurations/changelog-txt.json configuration file
- Write generated changelog along with "Release [incremented-version]" header into CHANGELOG.txt.temp file
- Concatenate contents of CHANGELOG.txt.temp and existing CHANGELOG.txt files into CHANGELOG.txt file using echo command
- Add CHANGELOG.txt file to git staging area

Update CHANGELOG.bbcode

-Similar steps as above but use .github/workflows/configurations/changelog-bbcode.json configuration file instead of changelog-txt.json.

Update CHANGELOG.md 

-Similar steps as above but use .github/workflows/configurations/changelog-md.json configuration file instead of changelog-txt.json.

Commit CHANGELOG files

- Commit the changes made to CHANGELOG.txt, CHANGELOG.bbcode, and CHANGELOG.md files with a message indicating the updated version

Push release branch to origin

- Push the release branch named "release/[incremented-version]" to the remote repository's origin

Create and merge pull request

- Create a pull request with title "Release [incremented-version]", empty body, and label "release" using gh pr create command
- Merge the pull request using gh pr merge command with --merge and --delete-branch options

Create and push release tag

- Create a git tag with the incremented version using git tag command
- Push the created tag to the remote repository's origin using git push command

Create release

- Generate changelog for GitHub release using mikepenz/release-changelog-builder-action@v4 with .github/workflows/configurations/changelog-github.json configuration file
- Create a zip artifact containing MOD_NAME-[incremented-version].zip by zipping ZIP_CONTENTS
- Use ncipollo/release-action@v1.12.0 to create a GitHub release with incremented version as tag, generated changelog as body, and MOD_NAME-[incremented-version].zip as an artifact

### Enhancements

Updated .gitignore ([PR #13](https://github.com/CaptainArbitrary/RimWorldMod/pull/13))

- https://www.toptal.com/developers/gitignore/api/macos,rider,visualstudio

Switched to shared workflows ([PR #34](https://github.com/CaptainArbitrary/RimWorldMod/pull/34))

### Bug Fixes

Refactor CI workflow to rename "build" job to "ci" ([PR #7](https://github.com/CaptainArbitrary/RimWorldMod/pull/7))

- Rename the "build" job in the CI workflow to "ci" for clarity and consistency.

Add a double newline at the end of the CHANGELOGs's PR template ([PR #10](https://github.com/CaptainArbitrary/RimWorldMod/pull/10))

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

Fix CD workflow to handle cases where there is no previous tag ([PR #25](https://github.com/CaptainArbitrary/RimWorldMod/pull/25))

- Update CD workflow to handle cases where there is no previous tag
- Use `git rev-list --max-parents=0 HEAD` as fallback when `git describe` fails
- This should return the SHA of the very first commit to the repository

Update CD workflow to handle quotes in the changelog ([PR #26](https://github.com/CaptainArbitrary/RimWorldMod/pull/26))

Fix CD workflow permissions ([PR #35](https://github.com/CaptainArbitrary/RimWorldMod/pull/35))

Restored changelog.json, which we need ([PR #37](https://github.com/CaptainArbitrary/RimWorldMod/pull/37))

Fix permissions in CD workflow ([PR #38](https://github.com/CaptainArbitrary/RimWorldMod/pull/38))

