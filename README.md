# multica-ipa

Unofficial **unsigned** iOS builds of the [Multica](https://github.com/multica-ai/multica) app, packaged as `.ipa` files for [LiveContainer](https://github.com/LiveContainer/LiveContainer).

## What this is

- The Multica iOS client is not on the App Store yet, and upstream does not publish `.ipa` files — the official route is building from source on a Mac with Xcode.
- This repo automates that build on GitHub Actions macOS runners and publishes the result to [Releases](../../releases): an **unsigned** IPA built from the official production configuration (connects to `api.multica.ai`, same as multica.ai on the web).
- Source code comes from [multica-ai/multica](https://github.com/multica-ai/multica), checked out at the upstream release tag matching each release here. Nothing in the app is modified.

## Important: unsigned

The IPA is built with code signing disabled (`CODE_SIGNING_ALLOWED=NO`). It has **no signature and no provisioning profile**, so it **cannot be installed the usual way** (not via Xcode device install, TestFlight, or sideloaders that expect a signed app). It is meant to be imported into **LiveContainer**, which runs unsigned apps without an Apple ID or developer account.

## How builds happen

`.github/workflows/build-ipa.yml` (manual `workflow_dispatch`):

1. Resolves the upstream ref (input `ref`, or the latest multica-ai/multica release tag).
2. Checks out upstream, installs the pnpm workspace, loads `apps/mobile/.env.production` with `APP_ENV=production`.
3. `expo prebuild -p ios` → `pod install` → `xcodebuild` (Release, iphoneos) with signing disabled.
4. Packages the `.app` into `Payload/` as `multica-<version>-unsigned.ipa`, uploads it as a workflow artifact, and attaches it to the GitHub Release named after the upstream version.

## Disclaimer

Unofficial build for personal use. Multica itself is developed by its own team — see [multica-ai/multica](https://github.com/multica-ai/multica) for the source and license.
