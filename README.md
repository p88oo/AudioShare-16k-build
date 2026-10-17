# AudioShare-16k-build

GitHub Actions workflow that builds the [p88oo/AudioShare](https://github.com/p88oo/AudioShare) APK for **Android 16 (API 36)** with **16 KB page-size alignment**.

## What it does
1. Uses **NDK r28** (16 KB ELF alignment by default) to rebuild `libflacJNI.so` from source
   (androidx/media `libraries/decoder_flac/src/main/jni` + libFLAC 1.3.2, via ndk-build).
2. Packages the 16 KB-aligned library into a local `:flac` module of AudioShare.
3. Builds `app-release.apk` with **AGP 8.5.1**, `compileSdk/targetSdk 36`.
4. Verifies with the official steps: `llvm-objdump` LOAD alignment `>= 2**14` and
   `zipalign -v -c -P 16 4` -> "Verification successful".
5. Uploads the APK as a workflow artifact (`app-release-16kb`).

## Run
- Actions tab -> **Run workflow** (workflow_dispatch), or push to the default branch.
- Download the APK from the artifact `app-release-16kb`.

No secrets required: all sources come from public repositories; the release APK is signed
with the `sign.jks` shipped in the AudioShare repo.
