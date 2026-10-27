# Vendored auth library

`supabase-js-2.80.0.js` is `@supabase/supabase-js` 2.80.0 from npm, bundled into one browser file so the
sign-in page loads nothing from another website (issue #145, engineering standard rule 21).

| | |
|---|---|
| Package | `@supabase/supabase-js@2.80.0` and its `@supabase/*-js@2.80.0` parts |
| npm integrity | `sha512-n8pkXQxuo5zCWXX5cbSNZj1vuWS8IVNGWTmP1m31Iq1k0e8lPZ07PF08TRV79HHq3mEPP/Ko//BQuflHvY2o8w==` |
| Bundler | esbuild 0.25.11 |
| File hash | `sha384-TReyjPkYfIrJmw50nJRCPhIRRFV7k6t3o+CQbeIuO/npyWX4wluylmYmsGAQcQwf` |

The page names this hash in its import map, and `connector/tests/issue-145-pinned-auth-library.test.mjs`
fails if the hash, the file or the version drift apart.

## Rebuild or check it

In an empty folder outside the repo:

```sh
npm init -y
npm install --ignore-scripts --save-exact @supabase/supabase-js@2.80.0 esbuild@0.25.11
npm view @supabase/supabase-js@2.80.0 dist.integrity   # must match the npm integrity above and package-lock.json
echo "export * from '@supabase/supabase-js';" > entry.mjs
npx esbuild entry.mjs --bundle --format=esm --platform=browser --minify --legal-comments=inline --outfile=supabase-js-2.80.0.js
openssl dgst -sha384 -binary supabase-js-2.80.0.js | openssl base64 -A   # must match the file hash above
```

The build is reproducible: the same command gives the same bytes. To move to a new version, build the new
file under its own versioned name, change the import and the import map hash in `../index.html`, update this
note, and copy the folder to the public sign-in repo (`docs/hybrid-data.md`).
