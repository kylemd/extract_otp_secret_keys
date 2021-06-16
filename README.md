# Extract TOTP/HOTP secret keys from Google Authenticator

Extract two-factor authentication (2FA, TFA) secret keys from export QR codes of "Google Authenticator" app

## Usage

1. Export the QR codes from "Google Authenticator" app
2. Read QR codes with QR code reader
3. Save the captured QR codes in a text file. Save each QR code on a new line. (The captured QR codes look like `otpauth-migration://offline?data=...`)
4. Call this script with the file as input:

        python extract_otp_secret_keys.py example_export.txt

### Optional Arguments

    -h, --help     show help message and exit
    --verbose, -v  verbose output
    --saveqr, -s   save QR code(s) as images to the "qr" subfolder
    --printqr, -p  print QR code(s) as text to the terminal (requires qrcode module listed below)
    
## Requirement

The protobuf package of Google for proto3 is required for running this script. protobuf >= 3.14 is recommended.

    pip install protobuf

Known to work with

* Python 3.6.12 and protobuf 3.14.0
* Python 3.8.5 and protobuf 3.14.0

### Optional

For printing QR codes, the qrcode module is required

    pip install qrcode[pil]

You can then call a printable QR code with the -p argument.

## Technical background

The export QR code of "Google Authenticator" contains the URL `otpauth-migration://offline?data=...`.
The data parameter is a base64 encoded proto3 message (Google Protocol Buffers).

Command for regeneration of Python code from proto3 message definition file (only necessary in case of changes of the proto3 message definition):

    protoc --python_out=generated_python google_auth.proto

## References

* Proto3 documentation: https://developers.google.com/protocol-buffers/docs/pythontutorial
* Template code: https://github.com/beemdevelopment/Aegis/pull/406
