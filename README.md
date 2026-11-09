# multica-ipa

[![Latest Release](https://img.shields.io/github/v/release/yoruraven/multica-ipa?label=Latest%20Release)](https://github.com/yoruraven/multica-ipa/releases/latest)

**[⬇️ Download the latest unsigned IPA](https://github.com/yoruraven/multica-ipa/releases/latest)**

Unofficial **unsigned** iOS builds of the [Multica](https://github.com/multica-ai/multica) app, packaged as `.ipa` files.

## What this is

- The Multica iOS client is not on the App Store yet, and upstream does not publish `.ipa` files — the official route is building from source on a Mac with Xcode.
- This repo automates that build on GitHub Actions macOS runners and publishes the result to [Releases](../../releases): an **unsigned** IPA built from the official production configuration (connects to `api.multica.ai`, same as multica.ai on the web).
- Source code comes from [multica-ai/multica](https://github.com/multica-ai/multica), checked out at the upstream release tag matching each release here. Nothing in the app is modified.

## How builds happen

`.github/workflows/build-ipa.yml` (manual `workflow_dispatch`, plus a daily scheduled run that builds only when upstream publishes a new release):

1. Resolves the upstream ref (input `ref`, or the latest multica-ai/multica release tag).
2. Checks out upstream, installs the pnpm workspace, loads `apps/mobile/.env.production` with `APP_ENV=production`.
3. `expo prebuild -p ios` → `pod install` → `xcodebuild` (Release, iphoneos) with signing disabled.
4. Packages the `.app` into `Payload/` as `multica-<version>-unsigned.ipa`, uploads it as a workflow artifact, and attaches it to the GitHub Release named after the upstream version.

## Disclaimer

Unofficial build for personal use. Multica itself is developed by its own team — see [multica-ai/multica](https://github.com/multica-ai/multica) for the source and license.
