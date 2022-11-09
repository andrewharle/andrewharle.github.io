# Create redirects in github pages

Note that these are not particularly useful as wget and curl don't follow these, non http header, redirects.

```sh
find * -type f -name "*.xz" -exec sh -c 'echo "---'\n'layout: redirected'\n'sitemap: false'\n'redirect_to:'\n'  - https://github.com/andrewharle/andrewharle.github.io/releases/download/s6-overlay/v3.1.2.1/{}'\n'---" > {}.md' \;
```

# Create html links

For the local files
```sh
find * -type f ! -name "*.xz.md" ! -name "*.html" -exec sh -c 'echo {} | awk "{ printf \"<a href=\x22./%s\x22>%s\n\", \$1, \$1 }"' \; >> tmp.html
```

For the redirected files.
```sh
find * -type f -name "*.xz.md" -exec sh -c 'basename -s ".md" {} | awk "{ printf \"<a href=\x22https://github.com/andrewharle/andrewharle.github.io/releases/download/s6-overlay/v3.1.2.1/%s\x22>%s\n\", \$1, \$1 }"' \; >> tmp.html
```
# Create releases file

`find */* -type f \( -name "*.tar.*" -o -name "*.sha*" \) -exec sh -c "echo https://github.com/andrewharle/andrewharle.github.io/releases/download/s6-overlay/\$(dirname {})/\$(basename -s ".md" {})" \; > s6-overlay.releases`

To sign: `gpg2 --detach-sig --armour s6-overlay.releases`