# `rtd`: simple [Readthedocs] CLI

## Install
```bash
pip install rtd-cli
# Set this env var, or write the token to ~/.config/rtd-cli/token
 RTD_TOKEN=your_token
```

## Usage
```bash
rtd --help
# Usage: rtd [OPTIONS] COMMAND [ARGS]...
#
#   Simple CLI wrapper for the Readthedocs REST API.
#
# Options:
#   --help  Show this message and exit.
#
# Commands:
#   api         Make a request to the Readthedocs REST API.
#   deactivate  Deactivate one or more versions
#   hide        Hide one or more versions
```

## Examples

List projects:
```bash
rtd api projects
# {
#   "count": …,
#   "next": null,
#   "previous": null,
#   "results": [
#     …
#   ]
# }
```

Hide all versions except for a specific list:

```bash
project=your_project
keep=(latest 1.11.)  # grep patterns to keep
limit=100

keep_args=()
for v in "${keep[@]}"; do
  keep_args+=(-e "$v")
done

rtd api "projects/$project/versions?limit=$limit&active=true&built=true" \
| jq -r '.results[] | select(.hidden | not) | .slug' \
| grep -v "${keep_args[@]}" \
| xargs rtd hide -p $p
```

[Readthedocs]: https://readthedocs.org
