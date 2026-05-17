# Logitech MX Keys S on Linux

Annoyingly, Logitech have co-opted the `Print` key on a regular keyboard to run the snipping/screenshot tool on Windows or macOS by invoking some weird key chord and not sending a keyboard scan code.

This obviously does not work on Linux, and opens some media menu or something instead.

Fixing the issue is not straightforward because Solaar does not seem to work quite propertly.

## Working Steps

### Divert the Snipping Tool key

Unlbock the `Key/Button Diversion` option, locate `Snipping Tool` and set it to `Diverted`.

This will cause the keyboard to send HID++ notifications for this key.

Note that this keypress will *not* show up in e.g. `xev`, but Solaar can detect it via its custom rules.

The settings should look like this:

![Solaar key diversion settings](https://github.com/caprica/mx-keys-s-linux/raw/master/etc/solaar-key-diversion.png "Solaar key diversion")

### Configure a custom rule

Using the Solaar UI does *not* work for some reason, it does not provide an affordance to set the desired key.

However, editing the configuration file directly *does* work:

In `~/.config/solaar/rules.yaml`:

```yaml
%YAML 1.3
---
- Rule:
  - Key: [Snipping Tool, pressed]
  - KeyPress:
    - Print
    - click
...
```

You *must* restart Solaar for it to pick up the configuration file change.

![Solaar rule editor](https://github.com/caprica/mx-keys-s-linux/raw/master/etc/solaar-rule-editor.png "Solaar rule editor")
