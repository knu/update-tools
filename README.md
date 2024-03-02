# Update Tools Action for GitHub Actions

This Action updates a tool version listed in `.tool-versions` at the top level of the target repository, one at a time.

It uses [mise](https://github.com/jdx/mise) to get the latest version of a specified tool.

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
    uses: knu/update-tools@v2
    with:
      tool: golang
      constraint: "1"
      release_url: https://go.dev/doc/devel/release

  update_buf:
    uses: knu/update-tools@v2
    with:
      tool: buf
      constraint: "1"
      release_url: https://github.com/bufbuild/buf/releases
```

### Inputs

- `tool` (string, required)

  Name of the mise/asdf plugin.  e.g. `golang`, `ruby`, `python`

- `constraint` (string, optional)

  [Version constraint](https://mise.jdx.dev/configuration.html#tool-versions) to pass to `mise latest`.

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
