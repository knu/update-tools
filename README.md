# Update Tools Action for GitHub Actions

This Action updates a tool version listed in `.tool-versions` at the top level of the target repository, one at a time.

It uses [rtx](https://github.com/jdx/rtx) to get the latest version of a specified tool.

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

  Name of the rtx/asdf plugin.  e.g. `golang`, `ruby`, `python`

- `constraint` (string, optional)

  Version constraint to pass to [`rtx latest`](https://github.com/jdx/rtx#tool-versions).

  It is most common to specify a major version or `major.minor`.

- `labels` (string, optional)

  Comma separated list of labels for a created PR.

  Default: `dependencies`

- `release_url` (string, optional)

  URL from which the reviewer can get release information of the tool.

  Default: A Google search URL generated from the tool name and the latest version.

- `token` (string, optional)

  GitHub access token to use for calling GitHub APIs and creating a PR.

  Default: `${{ github.token }}`

## Outputs

- `current_version`
- `latest_version`

## Author

Copyright (c) 2023 Akinori MUSHA.

Licensed under the 2-clause BSD license.  See `LICENSE.txt` for details.

Visit the [GitHub Repository](https://github.com/knu/update-tools) for the latest information.
