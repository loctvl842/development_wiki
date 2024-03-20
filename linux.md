# Linux

### Updating Specific Configuration in BSPWM Configuration File using `sed`

To update a specific configuration setting in a BSPWM configuration file (`$BSPWM_CONFIG`) while avoiding unintended changes, you can use `sed` with a targeted search pattern.

```bash
sed -i -e "/bspc config -m $EDP_OUTPUT top_padding/ { s/top_padding .*/top_padding $bspwm_pt/ }" $BSPWM_CONFIG
```

Explanation:

- `/bspc config -m $EDP_OUTPUT top_padding/` is the search pattern that targets the specific configuration setting.
- `{ s/top_padding .*/top_padding $bspwm_pt/ }` is the replacement pattern that updates the configuration setting with the new value.
  - `s/top_padding` is the `sed` command to substitute the matched pattern.
  - `.*` matches any character.
  - `top_padding $bspwm_pt` is the new value to replace the matched pattern.
