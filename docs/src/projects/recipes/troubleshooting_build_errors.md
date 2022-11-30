# troubleshooting build errors

## `sed: no input files`

**Error message**:
```bash
...
Setting up cross-python
sed: no input files

```


**Solutuion**:

An error message like the one above for packages using `cross-pyton` usually means
`python` is missing from the `host`-dependencies.

