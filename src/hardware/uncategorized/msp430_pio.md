# Using MSP430 with PlatformIO

There are some hurdles when configuring PlatformIO for development with MSP430 as there are no real examples and incomplete docs concerning uploading the code.
The following config seemed to work.

```ini
[env:lpmsp430g2553]
platform = timsp430
board = lpmsp430g2553
framework = arduino
debug_tool = mspdebug
upload_protocol = rf2500
```

The important parts are `debug_tool` and `upload_protocol`.
