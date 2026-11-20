# To do for PiCompose-Package-Driver-Satellite1

Open points and work that was postponed. Remove an entry in the change that settles it.

1. **Use an APT repository of FutureProofHomes:** As soon as FutureProofHomes provides an APT repository, drop the download in `debian/rules`.

2. **Remove `picompose-driver-satellite1` from the APT repository:** The workflow that removes a package from the APT repository has to be run for `picompose-driver-satellite1` in both suites, as the package is no longer built.

3. **Marker for one audio driver:** The packages of FutureProofHomes cannot provide `picompose-audio-driver`, so APT does not stop an installation next to another `picompose-driver-*` package. Decide whether that is acceptable.
