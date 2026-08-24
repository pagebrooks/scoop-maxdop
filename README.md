# scoop-maxdop

A [Scoop](https://scoop.sh) bucket for [maxdop](https://github.com/pagebrooks/maxdop) — an
opinionated T-SQL formatter that runs in CI, understands the whole language, and checks its own work.

```powershell
scoop bucket add maxdop https://github.com/pagebrooks/scoop-maxdop
scoop install maxdop
```

Then:

```powershell
maxdop query.sql        # formatted SQL to stdout
maxdop --write src\     # format every .sql under src\, in place
maxdop --check src\     # exit 1 if anything would change
```

x64 and arm64 are both published; Scoop picks the right one. The package is a single ~18 MB native
executable with no .NET runtime to install, so `scoop install` unpacks one file and is done.

## Why a bucket rather than `extras`

This one works today and needs nobody's approval. A submission to
[ScoopInstaller/Extras](https://github.com/ScoopInstaller/Extras) — which would remove the
`scoop bucket add` step for everyone — makes more sense once maxdop has visible traction, and it does
not preclude this bucket continuing to exist.

## How it stays current

`bucket/maxdop.json` carries `checkver` and `autoupdate`, and the Excavator workflow runs them daily
against maxdop's GitHub releases. New hashes come out of the release's published `SHA256SUMS` rather
than from downloading and hashing the archives, so an update is a few HTTP requests.

After cutting a maxdop release you can run **Actions → Excavator → Run workflow** instead of waiting
for the schedule.

Manifest changes are checked by `.github/workflows/ci.yml`, which downloads what the manifest points
at and confirms each hash. A manifest whose hash does not match the published archive installs for
nobody, and it is the one mistake worth catching automatically.
