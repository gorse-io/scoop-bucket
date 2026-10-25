# Gorse Scoop Bucket

Install `gorse-cli` with [Scoop](https://scoop.sh/):

```powershell
scoop bucket add gorse https://github.com/gorse-io/scoop-bucket
scoop install gorse-cli
```

## Automatic updates

After a stable `gorse-io/cli` release has uploaded its binaries, its release
workflow sends a `gorse-cli-release` repository dispatch to this repository.
The Excavator workflow uses `ScoopInstaller/GithubActions` to check the bucket's
manifests and commit available updates, including download URLs and SHA256 hashes,
directly to `main`. It can also be run manually with `workflow_dispatch`.
Excavator checks all manifests in the bucket, so another package with an available
update may be updated in the same run. It checks the latest stable releases rather
than trusting a version from the dispatch payload.

### Setup

1. Merge the Excavator workflow into this repository's default branch before
   enabling the sender in `gorse-io/cli`.
2. Create a fine-grained personal access token with access only to
   `gorse-io/scoop-bucket` and **Contents: Read and write**. Complete any required
   organization approval, then save it as the `SCOOP_BUCKET_TOKEN` Actions secret
   in **gorse-io/cli**. The CLI repository's built-in `GITHUB_TOKEN` cannot dispatch
   events to another repository.
3. Ensure repository rules allow the bucket workflow's `GITHUB_TOKEN` to push
   manifest updates to `main`.

The receiver uses its own `GITHUB_TOKEN`; no additional secret is needed here.
A missing or expired sender token fails the Scoop notification job without
blocking the independent npm publication job. Retry the notification job after
fixing the secret, or run Excavator manually.
