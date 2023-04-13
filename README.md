# Update Tools Action for GitHub Actions

This Action updates a tool version listed in `.tool-version` at the top level of the target repository, one at a time.

It uses [asdf](https://asdf-vm.com/) to get the latest version of a specified tool, so it depends on the asdf plugin how fast the latest releases are found and available.  For example, [the ruby plugin](https://github.com/asdf-vm/asdf-ruby) requires ruby-build to be updated before it can install a new release of Ruby.

## Usage

``` yaml
on:
  schedule:
    - cron: "0 0 * * 1"  # At 00:00 UTC on Mondays

  workflow_dispatch:

  push:
    branches:
      - main
    paths:
      - ".tool-versions"

name: "Update Tools"

permissions:
  contents: write
  pull-requests: write

jobs:
  update_golang:
    uses: knu/update_tools@v1
    with:
      tool: golang
      constraint: "1"
      release_url: https://go.dev/doc/devel/release

  update_buf:
    uses: knu/update_tools@v1
    with:
      tool: buf
      constraint: "1"
      release_url: https://github.com/bufbuild/buf/releases
```

### Inputs

- `tool` (string, required)

  Name of the asdf plugin.  e.g. `golang`, `ruby`, `python`

- `constraint` (string, optional)

  Version constraint to pass to [`asdf latest`](https://asdf-vm.com/manage/versions.html#show-latest-stable-version).

  The latest version that begins with the given string.

- `labels` (string, optional)

  Comma separated list of labels for a created PR.

  Default: `automerge,dependencies`

- `release_url` (string, optional)

  URL from which the reviewer can get release information of the tool.

  Default: A Google search URL generated from the tool name and the latest version.

## Outputs

- `current_version`
- `latest_version`

## Author

Copyright (c) 2023 Akinori MUSHA.

Licensed under the 2-clause BSD license.  See `LICENSE.txt` for details.

Visit the [GitHub Repository](https://github.com/knu/update-tools) for the latest information.
