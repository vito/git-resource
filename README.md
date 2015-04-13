# Git Resource

Tracks the commits in a [git](http://git-scm.com/) repository.


## Source Configuration

* `uri`: *Required.* The location of the repository.

* `branch`: *Required.* The branch to track. If not specified, the repository's
  default branch is assumed.

* `private_key`: *Optional.* Private key to use when pulling/pushing.
    Example:
    ```
    private_key: |
      -----BEGIN RSA PRIVATE KEY-----
      MIIEowIBAAKCAQEAtCS10/f7W7lkQaSgD/mVeaSOvSF9ql4hf/zfMwfVGgHWjj+W
      <Lots more text>
      DWiJL+OFeg9kawcUL6hQ8JeXPhlImG6RTUffma9+iGQyyBMCGd1l
      -----END RSA PRIVATE KEY-----
    ```

* `paths`: *Optional.* If specified (as a list of glob patterns), only changes
  to the specified files will yield new versions.

* `ignore_paths`: *Optional.* The inverse of `paths`; changes to the specified
  files are ignored.


## Behavior

### `check`: Check for new commits.

The repository is cloned (or pulled if already present), and any commits
made after the given version are returned. If no version is given, the ref
for `HEAD` is returned.


### `get`: Clone the repository, at the given ref.

Clones the repository to the destination, and locks it down to a given ref.
Returns the resulting ref as the version.

Submodules are initialized and updated recursively.


#### Parameters

* `fetch`: *Optional.* Additional branches to fetch so that they're available
  to the build.

* `submodules`: *Optional.* If `none`, submodules will not be
  fetched. If specified as a list of paths, only the given paths will be
  fetched. If not specified, or if `all` is explicitly specified, all
  submodules are fetched.


### `put`: Push to a repository.

Push a repository to the source's URI and branch. All tags are also pushed
to the source.

#### Parameters

* `repository`: *Required.* The path of the repository to push to the source.

* `rebase`: *Optional.* If pushing fails with non-fast-forward, continuously
  attempt rebasing and pushing.

* `tag`: *Optional* If this is set then HEAD will be tagged. The value should be
  a path to a file containing the name of the tag.

* `tag_prefix`: *Optional.* If specified, the tag read from the file will be
prepended with this string. This is useful for adding `v` in front of
version numbers.
