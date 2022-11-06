# Release Overview

 * GitHub releases are used for now, until, or in addition, till a local network host option is used.
 * Binaries (+ signed checksum) live on the github releases page.
 * Whereas only the metadata and signing live on the github website repo.

# Collect Build

The sha256 file and the signed file will goto both the releases page and the github repo to index.

To collect the individual sha256 files for each arch into a single file...
```sh
find . -maxdepth 1 -type f -name "*.sha256" -exec bash -c "cat {} | sed -e 's/..\/builds\///' >> linux-musl-\$(echo '{}' | grep -Po '\-\Kgcc_(\S+)(?=.tar)').sha256sum" \;
```
To check `sha256sum -c linux-musl-gcc_12.2.0-musl_1.2.3.sha256sum`

To sign `gpg2 --detach-sig --armour linux-musl-gcc_12.2.0-musl_1.2.3.sha256sum`

Create a build manifest:

```sh
csplit -z --suppress-matched ./compilers/musl/mussel/mussel.sh %"Package Versions"%-1 /URLs/ {*}
rm xx01 && mv xx00 manifest.md
```

We should be left with:
 * A tar.xz file for each arch;
 * An sha256sum file;
 * An asc file;
 * A coherent build manifest.

# Procedure
Create release on github in the source repo. Tag according to musl/v$-?
    where:
     $ is the musl version
     ? is my release version

    NOT NECESSARY TO UPLOAD, as this isn't a public repo.

Copy the SHA of the commit into the manifest file.

Create the directory file on the website, and add the links + the sha256sum + asc file.

Create release on github in the andrewharle.github.io repo. Tag identically to the source repo.

Upload the manifest + set of release files.

Test all is working!!