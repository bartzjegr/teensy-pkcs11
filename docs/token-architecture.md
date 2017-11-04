# Teensy PKCS-11 Token Design

## Token Limitations

- It does not support protected "protected authentication path" thus `CKF_PROTECTED_AUTHENTICATION_PATH` will always be false.
