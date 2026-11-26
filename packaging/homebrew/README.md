# Homebrew release

`gloam.rb` is the formula for `londonpixelexchange/homebrew-tap/Formula/gloam.rb`.
It installs an isolated Python environment, uses Homebrew's NumPy and ffmpeg,
and includes certifi and sounddevice with Homebrew's PortAudio.

For 1.1.0, first commit the Gloam changes, push the branch and publish the tag:

```sh
git tag v1.1.0
git push origin main
git push origin v1.1.0
```

Run these from Gloam's repository, after committing the release. The tag must
point to the commit containing `__version__ = "1.1.0"` and all release changes.
Do not move a published release tag.

Copy `gloam.rb` into the tap's `Formula` directory. Before publishing the tap:

```sh
brew style londonpixelexchange/tap/gloam
brew install --build-from-source londonpixelexchange/tap/gloam
brew test londonpixelexchange/tap/gloam
```

Then commit and push the tap changes. Users can install with:

```sh
brew install londonpixelexchange/tap/gloam
```

For later releases, publish the tag, then update the formula's tag and full
`revision` commit hash, copy the updated formula to the tap, and test it before
pushing. The Python version must match a
version supported by Homebrew's NumPy formula. Resource URLs and SHA-256 checksums
pin the Python dependencies; refresh them when upgrading those dependencies.
