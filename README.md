# Strict Guard APK build

GitHub Actions builds the source archive automatically when a root-level `.zip` file or the build workflow changes on a push. The workflow can also be started manually from **Actions → Build Strict Guard APK → Run workflow**.

## Uploading a new version

1. Replace the old source ZIP in the repository root with the new one (keep **exactly one** ZIP there). A deletion and an upload in separate commits can cause an expected failed run between them; the upload starts another run.
2. The ZIP must contain exactly one `build.sh`, compatible with the existing Java 17, Android SDK 35, and Kotlin 1.9.22 environment. It must put signed APKs in `output/` next to that script. The current archive follows this layout.
3. Open **Actions → Build Strict Guard APK**, select the latest run, and download the **Strict-Guard-Build** artifact. The APK is inside its `apk/` directory; `build.log` contains the build and test output. Artifacts expire after 14 days.

The workflow checks the APK signature, but a successful build does not replace testing on a device. It cannot guarantee that arbitrary future ZIPs will build: the archive's build script and dependencies must remain compatible.

**Signing:** The current source build script generates a fresh debug signing key on each run. APKs from different runs cannot normally update each other in place. Uninstalling an existing app can erase its data; do not uninstall without a backup. Stable updates require a securely managed, persistent signing key and changes to the build script.
