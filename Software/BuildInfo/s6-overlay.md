# Release Overview

 * GitHub releases are used for now, until, or in addition, till a local network host option is used.
 * Binaries (+ signed checksum) live on the github releases page.
 * Whereas only the metadata and signing live on the github website repo.

# Collect Build

The sha256 file and the signed file will goto both the releases page and the github repo to index.

To collect the individual sha256 files for each arch into a single file...
```sh
cd builds
find * -maxdepth 2 -type f -name "*.sha256" -exec bash -c "cat {} >> \$(dirname '{}')/\$(dirname '{}').sha256sum" \;
```
To check: `sha256sum -c s6-overlay_v3.1.2.1.sha256sum`

To sign: `gpg2 --detach-sig --armour s6-overlay_v3.1.2.1.sha256sum`

To compress the build log: `xz -z -9 *.log`

We should be left with:
 * A set of tar.xz files, also for the corresponding architectures;
 * An sha256sum file;
 * An asc file;
 * A Release Notes file.

# Procedure
Create release on github in the source repo. Tag according to s6-overlay/v$
    where:
     $ is the s6-overlay version

    NOT NECESSARY TO UPLOAD, as this isn't a public repo.

Copy the SHA of the commit into the manifest file.

Create the directory file on the website, and add the links + the sha256sum + asc file.

Create release on github in the andrewharle.github.io repo. Tag identically to the source repo.

Upload the manifest + set of release files.

Test all is working!!