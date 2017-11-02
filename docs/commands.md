# Teensy PKCS Command

Commands are encoded in [msgpack](https://github.com/msgpack/msgpack/blob/master/spec.md) encoding

## Table Of Contents
- [Functions](#functions)
    - [Slot and Token Management Functions](#slot-and-token-management-functions)
    - [Session Management Functions](#session-management-functions)
    - [Object Management Functions](#object-management-functions)
    - [Encryption Functions](#encryption-functions)
    - [Decryption Functions](#decryption-functions)
    - [Message digesting Functions](#message-digesting-functions)
        - [Digest Init](#digest-init)
        - [Digest](#digest)
        - [Digest Update](#digest-update)
        - [Digest Key](#digest-key)
        - [Digest Final](#digest-final)
    - [Signing and MACing Functions](#signing-and-macing-functions)
    - [Functions for Verifying signatures and MACs](#functions-for-verifying-signatures-and-macs)
    - [Dual-function Cryptographic Functions](#dual-function-cryptographic-functions)
    - [Key Management Functions](#key-management-functions)
    - [Random Number Generation Functions](#random-number-generation-functions)
- [Types](#types)
    - [Return Value](#return-value)
    - [Mechanism Codes](#mechanism-codes)

## Functions

### Slot and Token Management Functions

### Session Management Functions

### Object Management Functions

### Encryption Functions

### Decryption Functions

### Message Digesting Functions

#### Digest Init

Initializes a message-digesting operation.

**Request**

| Name      | Type                                  | Representation  | Description        |
|-----------|---------------------------------------|-----------------|--------------------|
| hSession  | CK_SESSION_HANDLE                     | positive fixint | Session handle     |
| mechanism | [CK_MECHANISM_TYPE](#mechanism-codes) | uint 32         | Digest mechanism   |
| parameter | byte array                            | bin 8/16/32     | Optional parameter |

**Response**

| Name   | Type                   | Representation | Description  |
|--------|------------------------|----------------|--------------|
| status | [CK_RV](#return-value) | uint 32        | Return value |

**Applicable Mechanism Codes**

- `CKM_MD2`
- `CKM_MD5`
- `CKM_SHA_1`
- `CKM_RIPEMD128`
- `CKM_RIPEMD160`
- `CKM_SHA256`
- `CKM_SHA224`
- `CKM_SHA384`
- `CKM_SHA512`

#### Digest

Digests data in a single part.

**Request**

| Name      | Type                                  | Representation  | Description          |
|-----------|---------------------------------------|-----------------|----------------------|
| hSession  | CK_SESSION_HANDLE                     | positive fixint | Session handle       |
| data      | byte array                            | bin 8/16/32     | Data to be processed |

**Response**

| Name   | Type                   | Representation | Description    |
|--------|------------------------|----------------|----------------|
| status | [CK_RV](#return-value) | uint 32        | Return value   |
| digest | byte array             | bin 8/16       | Digest of data |

#### Digest Update

Continues a multiple-part message-digesting operation, processing another data part.

**Request**

| Name      | Type                                  | Representation  | Description                  |
|-----------|---------------------------------------|-----------------|------------------------------|
| hSession  | CK_SESSION_HANDLE                     | positive fixint | Session handle               |
| part      | byte array                            | bin 8/16/32     | Part of data to be processed |

**Response**

| Name   | Type                   | Representation | Description    |
|--------|------------------------|----------------|----------------|
| status | [CK_RV](#return-value) | uint 32        | Return value   |

#### Digest Key

#### Digest Final

### Signing and MACing Functions

### Functions for Verifying signatures and MACs

### Dual-function Cryptographic Functions

### Key Management Functions

### Random Number Generation Functions

## Types

### Return Value

Possible values of `CK_RV`

| Code                                    | Value      |
|-----------------------------------------|------------|
| `CKR_OK`                                | 0x00000000 |
| `CKR_CANCEL`                            | 0x00000001 |
| `CKR_HOST_MEMORY`                       | 0x00000002 |
| `CKR_SLOT_ID_INVALID`                   | 0x00000003 |
| `CKR_GENERAL_ERROR`                     | 0x00000005 |
| `CKR_FUNCTION_FAILED`                   | 0x00000006 |
| `CKR_ARGUMENTS_BAD`                     | 0x00000007 |
| `CKR_NO_EVENT`                          | 0x00000008 |
| `CKR_NEED_TO_CREATE_THREADS`            | 0x00000009 |
| `CKR_CANT_LOCK`                         | 0x0000000A |
| `CKR_ATTRIBUTE_READ_ONLY`               | 0x00000010 |
| `CKR_ATTRIBUTE_SENSITIVE`               | 0x00000011 |
| `CKR_ATTRIBUTE_TYPE_INVALID`            | 0x00000012 |
| `CKR_ATTRIBUTE_VALUE_INVALID`           | 0x00000013 |
| `CKR_DATA_INVALID`                      | 0x00000020 |
| `CKR_DATA_LEN_RANGE`                    | 0x00000021 |
| `CKR_DEVICE_ERROR`                      | 0x00000030 |
| `CKR_DEVICE_MEMORY`                     | 0x00000031 |
| `CKR_DEVICE_REMOVED`                    | 0x00000032 |
| `CKR_ENCRYPTED_DATA_INVALID`            | 0x00000040 |
| `CKR_ENCRYPTED_DATA_LEN_RANGE`          | 0x00000041 |
| `CKR_FUNCTION_CANCELED`                 | 0x00000050 |
| `CKR_FUNCTION_NOT_PARALLEL`             | 0x00000051 |
| `CKR_FUNCTION_NOT_SUPPORTED`            | 0x00000054 |
| `CKR_KEY_HANDLE_INVALID`                | 0x00000060 |
| `CKR_KEY_SIZE_RANGE`                    | 0x00000062 |
| `CKR_KEY_TYPE_INCONSISTENT`             | 0x00000063 |
| `CKR_KEY_NOT_NEEDED`                    | 0x00000064 |
| `CKR_KEY_CHANGED`                       | 0x00000065 |
| `CKR_KEY_NEEDED`                        | 0x00000066 |
| `CKR_KEY_INDIGESTIBLE`                  | 0x00000067 |
| `CKR_KEY_FUNCTION_NOT_PERMITTED`        | 0x00000068 |
| `CKR_KEY_NOT_WRAPPABLE`                 | 0x00000069 |
| `CKR_KEY_UNEXTRACTABLE`                 | 0x0000006A |
| `CKR_MECHANISM_INVALID`                 | 0x00000070 |
| `CKR_MECHANISM_PARAM_INVALID`           | 0x00000071 |
| `CKR_OBJECT_HANDLE_INVALID`             | 0x00000082 |
| `CKR_OPERATION_ACTIVE`                  | 0x00000090 |
| `CKR_OPERATION_NOT_INITIALIZED`         | 0x00000091 |
| `CKR_PIN_INCORRECT`                     | 0x000000A0 |
| `CKR_PIN_INVALID`                       | 0x000000A1 |
| `CKR_PIN_LEN_RANGE`                     | 0x000000A2 |
| `CKR_PIN_EXPIRED`                       | 0x000000A3 |
| `CKR_PIN_LOCKED`                        | 0x000000A4 |
| `CKR_SESSION_CLOSED`                    | 0x000000B0 |
| `CKR_SESSION_COUNT`                     | 0x000000B1 |
| `CKR_SESSION_HANDLE_INVALID`            | 0x000000B3 |
| `CKR_SESSION_PARALLEL_NOT_SUPPORTED`    | 0x000000B4 |
| `CKR_SESSION_READ_ONLY`                 | 0x000000B5 |
| `CKR_SESSION_EXISTS`                    | 0x000000B6 |
| `CKR_SESSION_READ_ONLY_EXISTS`          | 0x000000B7 |
| `CKR_SESSION_READ_WRITE_SO_EXISTS`      | 0x000000B8 |
| `CKR_SIGNATURE_INVALID`                 | 0x000000C0 |
| `CKR_SIGNATURE_LEN_RANGE`               | 0x000000C1 |
| `CKR_TEMPLATE_INCOMPLETE`               | 0x000000D0 |
| `CKR_TEMPLATE_INCONSISTENT`             | 0x000000D1 |
| `CKR_TOKEN_NOT_PRESENT`                 | 0x000000E0 |
| `CKR_TOKEN_NOT_RECOGNIZED`              | 0x000000E1 |
| `CKR_TOKEN_WRITE_PROTECTED`             | 0x000000E2 |
| `CKR_UNWRAPPING_KEY_HANDLE_INVALID`     | 0x000000F0 |
| `CKR_UNWRAPPING_KEY_SIZE_RANGE`         | 0x000000F1 |
| `CKR_UNWRAPPING_KEY_TYPE_INCONSISTENT`  | 0x000000F2 |
| `CKR_USER_ALREADY_LOGGED_IN`            | 0x00000100 |
| `CKR_USER_NOT_LOGGED_IN`                | 0x00000101 |
| `CKR_USER_PIN_NOT_INITIALIZED`          | 0x00000102 |
| `CKR_USER_TYPE_INVALID`                 | 0x00000103 |
| `CKR_USER_ANOTHER_ALREADY_LOGGED_IN`    | 0x00000104 |
| `CKR_USER_TOO_MANY_TYPES`               | 0x00000105 |
| `CKR_WRAPPED_KEY_INVALID`               | 0x00000110 |
| `CKR_WRAPPED_KEY_LEN_RANGE`             | 0x00000112 |
| `CKR_WRAPPING_KEY_HANDLE_INVALID`       | 0x00000113 |
| `CKR_WRAPPING_KEY_SIZE_RANGE`           | 0x00000114 |
| `CKR_WRAPPING_KEY_TYPE_INCONSISTENT`    | 0x00000115 |
| `CKR_RANDOM_SEED_NOT_SUPPORTED`         | 0x00000120 |
| `CKR_RANDOM_NO_RNG`                     | 0x00000121 |
| `CKR_DOMAIN_PARAMS_INVALID`             | 0x00000130 |
| `CKR_BUFFER_TOO_SMALL`                  | 0x00000150 |
| `CKR_SAVED_STATE_INVALID`               | 0x00000160 |
| `CKR_INFORMATION_SENSITIVE`             | 0x00000170 |
| `CKR_STATE_UNSAVEABLE`                  | 0x00000180 |
| `CKR_CRYPTOKI_NOT_INITIALIZED`          | 0x00000190 |
| `CKR_CRYPTOKI_ALREADY_INITIALIZED`      | 0x00000191 |
| `CKR_MUTEX_BAD`                         | 0x000001A0 |
| `CKR_MUTEX_NOT_LOCKED`                  | 0x000001A1 |
| `CKR_NEW_PIN_MODE`                      | 0x000001B0 |
| `CKR_NEXT_OTP`                          | 0x000001B1 |
| `CKR_EXCEEDED_MAX_ITERATIONS`           | 0x000001B5 |
| `CKR_FIPS_SELF_TEST_FAILED`             | 0x000001B6 |
| `CKR_LIBRARY_LOAD_FAILED`               | 0x000001B7 |
| `CKR_PIN_TOO_WEAK`                      | 0x000001B8 |
| `CKR_PUBLIC_KEY_INVALID`                | 0x000001B9 |
| `CKR_FUNCTION_REJECTED`                 | 0x00000200 |
| `CKR_VENDOR_DEFINED`                    | 0x80000000 |

### Mechanism Codes

Possible Values of CK_MECHANISM_TYPE

| Code                                  | Value      |
|---------------------------------------|------------|
| `CKM_RSA_PKCS_KEY_PAIR_GEN`           | 0x00000000 |
| `CKM_RSA_PKCS`                        | 0x00000001 |
| `CKM_RSA_9796`                        | 0x00000002 |
| `CKM_RSA_X_509`                       | 0x00000003 |
| `CKM_MD2_RSA_PKCS`                    | 0x00000004 |
| `CKM_MD5_RSA_PKCS`                    | 0x00000005 |
| `CKM_SHA1_RSA_PKCS`                   | 0x00000006 |
| `CKM_RIPEMD128_RSA_PKCS`              | 0x00000007 |
| `CKM_RIPEMD160_RSA_PKCS`              | 0x00000008 |
| `CKM_RSA_PKCS_OAEP`                   | 0x00000009 |
| `CKM_RSA_X9_31_KEY_PAIR_GEN`          | 0x0000000A |
| `CKM_RSA_X9_31`                       | 0x0000000B |
| `CKM_SHA1_RSA_X9_31`                  | 0x0000000C |
| `CKM_RSA_PKCS_PSS`                    | 0x0000000D |
| `CKM_SHA1_RSA_PKCS_PSS`               | 0x0000000E |
| `CKM_DSA_KEY_PAIR_GEN`                | 0x00000010 |
| `CKM_DSA`                             | 0x00000011 |
| `CKM_DSA_SHA1`                        | 0x00000012 |
| `CKM_DSA_SHA224`                      | 0x00000013 |
| `CKM_DSA_SHA256`                      | 0x00000014 |
| `CKM_DSA_SHA384`                      | 0x00000015 |
| `CKM_DSA_SHA512`                      | 0x00000016 |
| `CKM_DH_PKCS_KEY_PAIR_GEN`            | 0x00000020 |
| `CKM_DH_PKCS_DERIVE`                  | 0x00000021 |
| `CKM_X9_42_DH_KEY_PAIR_GEN`           | 0x00000030 |
| `CKM_X9_42_DH_DERIVE`                 | 0x00000031 |
| `CKM_X9_42_DH_HYBRID_DERIVE`          | 0x00000032 |
| `CKM_X9_42_MQV_DERIVE`                | 0x00000033 |
| `CKM_SHA256_RSA_PKCS`                 | 0x00000040 |
| `CKM_SHA384_RSA_PKCS`                 | 0x00000041 |
| `CKM_SHA512_RSA_PKCS`                 | 0x00000042 |
| `CKM_SHA256_RSA_PKCS_PSS`             | 0x00000043 |
| `CKM_SHA384_RSA_PKCS_PSS`             | 0x00000044 |
| `CKM_SHA512_RSA_PKCS_PSS`             | 0x00000045 |
| `CKM_SHA224_RSA_PKCS`                 | 0x00000046 |
| `CKM_SHA224_RSA_PKCS_PSS`             | 0x00000047 |
| `CKM_RC2_KEY_GEN`                     | 0x00000100 |
| `CKM_RC2_ECB`                         | 0x00000101 |
| `CKM_RC2_CBC`                         | 0x00000102 |
| `CKM_RC2_MAC`                         | 0x00000103 |
| `CKM_RC2_MAC_GENERAL`                 | 0x00000104 |
| `CKM_RC2_CBC_PAD`                     | 0x00000105 |
| `CKM_RC4_KEY_GEN`                     | 0x00000110 |
| `CKM_RC4`                             | 0x00000111 |
| `CKM_DES_KEY_GEN`                     | 0x00000120 |
| `CKM_DES_ECB`                         | 0x00000121 |
| `CKM_DES_CBC`                         | 0x00000122 |
| `CKM_DES_MAC`                         | 0x00000123 |
| `CKM_DES_MAC_GENERAL`                 | 0x00000124 |
| `CKM_DES_CBC_PAD`                     | 0x00000125 |
| `CKM_DES2_KEY_GEN`                    | 0x00000130 |
| `CKM_DES3_KEY_GEN`                    | 0x00000131 |
| `CKM_DES3_ECB`                        | 0x00000132 |
| `CKM_DES3_CBC`                        | 0x00000133 |
| `CKM_DES3_MAC`                        | 0x00000134 |
| `CKM_DES3_MAC_GENERAL`                | 0x00000135 |
| `CKM_DES3_CBC_PAD`                    | 0x00000136 |
| `CKM_DES3_CMAC_GENERAL`               | 0x00000137 |
| `CKM_DES3_CMAC`                       | 0x00000138 |
| `CKM_CDMF_KEY_GEN`                    | 0x00000140 |
| `CKM_CDMF_ECB`                        | 0x00000141 |
| `CKM_CDMF_CBC`                        | 0x00000142 |
| `CKM_CDMF_MAC`                        | 0x00000143 |
| `CKM_CDMF_MAC_GENERAL`                | 0x00000144 |
| `CKM_CDMF_CBC_PAD`                    | 0x00000145 |
| `CKM_DES_OFB64`                       | 0x00000150 |
| `CKM_DES_OFB8`                        | 0x00000151 |
| `CKM_DES_CFB64`                       | 0x00000152 |
| `CKM_DES_CFB8`                        | 0x00000153 |
| `CKM_MD2`                             | 0x00000200 |
| `CKM_MD2_HMAC`                        | 0x00000201 |
| `CKM_MD2_HMAC_GENERAL`                | 0x00000202 |
| `CKM_MD5`                             | 0x00000210 |
| `CKM_MD5_HMAC`                        | 0x00000211 |
| `CKM_MD5_HMAC_GENERAL`                | 0x00000212 |
| `CKM_SHA_1`                           | 0x00000220 |
| `CKM_SHA_1_HMAC`                      | 0x00000221 |
| `CKM_SHA_1_HMAC_GENERAL`              | 0x00000222 |
| `CKM_RIPEMD128`                       | 0x00000230 |
| `CKM_RIPEMD128_HMAC`                  | 0x00000231 |
| `CKM_RIPEMD128_HMAC_GENERAL`          | 0x00000232 |
| `CKM_RIPEMD160`                       | 0x00000240 |
| `CKM_RIPEMD160_HMAC`                  | 0x00000241 |
| `CKM_RIPEMD160_HMAC_GENERAL`          | 0x00000242 |
| `CKM_SHA256`                          | 0x00000250 |
| `CKM_SHA256_HMAC`                     | 0x00000251 |
| `CKM_SHA256_HMAC_GENERAL`             | 0x00000252 |
| `CKM_SHA224`                          | 0x00000255 |
| `CKM_SHA224_HMAC`                     | 0x00000256 |
| `CKM_SHA224_HMAC_GENERAL`             | 0x00000257 |
| `CKM_SHA384`                          | 0x00000260 |
| `CKM_SHA384_HMAC`                     | 0x00000261 |
| `CKM_SHA384_HMAC_GENERAL`             | 0x00000262 |
| `CKM_SHA512`                          | 0x00000270 |
| `CKM_SHA512_HMAC`                     | 0x00000271 |
| `CKM_SHA512_HMAC_GENERAL`             | 0x00000272 |
| `CKM_SECURID_KEY_GEN`                 | 0x00000280 |
| `CKM_SECURID`                         | 0x00000282 |
| `CKM_HOTP_KEY_GEN`                    | 0x00000290 |
| `CKM_HOTP`                            | 0x00000291 |
| `CKM_ACTI`                            | 0x000002A0 |
| `CKM_ACTI_KEY_GEN`                    | 0x000002A1 |
| `CKM_CAST_KEY_GEN`                    | 0x00000300 |
| `CKM_CAST_ECB`                        | 0x00000301 |
| `CKM_CAST_CBC`                        | 0x00000302 |
| `CKM_CAST_MAC`                        | 0x00000303 |
| `CKM_CAST_MAC_GENERAL`                | 0x00000304 |
| `CKM_CAST_CBC_PAD`                    | 0x00000305 |
| `CKM_CAST3_KEY_GEN`                   | 0x00000310 |
| `CKM_CAST3_ECB`                       | 0x00000311 |
| `CKM_CAST3_CBC`                       | 0x00000312 |
| `CKM_CAST3_MAC`                       | 0x00000313 |
| `CKM_CAST3_MAC_GENERAL`               | 0x00000314 |
| `CKM_CAST3_CBC_PAD`                   | 0x00000315 |
| `CKM_CAST5_KEY_GEN`                   | 0x00000320 |
| `CKM_CAST128_KEY_GEN`                 | 0x00000320 |
| `CKM_CAST5_ECB`                       | 0x00000321 |
| `CKM_CAST128_ECB`                     | 0x00000321 |
| `CKM_CAST5_CBC`                       | 0x00000322 |
| `CKM_CAST128_CBC`                     | 0x00000322 |
| `CKM_CAST5_MAC`                       | 0x00000323 |
| `CKM_CAST128_MAC`                     | 0x00000323 |
| `CKM_CAST5_MAC_GENERAL`               | 0x00000324 |
| `CKM_CAST128_MAC_GENERAL`             | 0x00000324 |
| `CKM_CAST5_CBC_PAD`                   | 0x00000325 |
| `CKM_CAST128_CBC_PAD`                 | 0x00000325 |
| `CKM_RC5_KEY_GEN`                     | 0x00000330 |
| `CKM_RC5_ECB`                         | 0x00000331 |
| `CKM_RC5_CBC`                         | 0x00000332 |
| `CKM_RC5_MAC`                         | 0x00000333 |
| `CKM_RC5_MAC_GENERAL`                 | 0x00000334 |
| `CKM_RC5_CBC_PAD`                     | 0x00000335 |
| `CKM_IDEA_KEY_GEN`                    | 0x00000340 |
| `CKM_IDEA_ECB`                        | 0x00000341 |
| `CKM_IDEA_CBC`                        | 0x00000342 |
| `CKM_IDEA_MAC`                        | 0x00000343 |
| `CKM_IDEA_MAC_GENERAL`                | 0x00000344 |
| `CKM_IDEA_CBC_PAD`                    | 0x00000345 |
| `CKM_GENERIC_SECRET_KEY_GEN`          | 0x00000350 |
| `CKM_CONCATENATE_BASE_AND_KEY`        | 0x00000360 |
| `CKM_CONCATENATE_BASE_AND_DATA`       | 0x00000362 |
| `CKM_CONCATENATE_DATA_AND_BASE`       | 0x00000363 |
| `CKM_XOR_BASE_AND_DATA`               | 0x00000364 |
| `CKM_EXTRACT_KEY_FROM_KEY`            | 0x00000365 |
| `CKM_SSL3_PRE_MASTER_KEY_GEN`         | 0x00000370 |
| `CKM_SSL3_MASTER_KEY_DERIVE`          | 0x00000371 |
| `CKM_SSL3_KEY_AND_MAC_DERIVE`         | 0x00000372 |
| `CKM_SSL3_MASTER_KEY_DERIVE_DH`       | 0x00000373 |
| `CKM_TLS_PRE_MASTER_KEY_GEN`          | 0x00000374 |
| `CKM_TLS_KEY_AND_MAC_DERIVE`          | 0x00000376 |
| `CKM_TLS_MASTER_KEY_DERIVE`           | 0x00000375 |
| `CKM_TLS_MASTER_KEY_DERIVE_DH`        | 0x00000377 |
| `CKM_TLS_PRF`                         | 0x00000378 |
| `CKM_SSL3_MD5_MAC`                    | 0x00000380 |
| `CKM_SSL3_SHA1_MAC`                   | 0x00000381 |
| `CKM_MD5_KEY_DERIVATION`              | 0x00000390 |
| `CKM_MD2_KEY_DERIVATION`              | 0x00000391 |
| `CKM_SHA1_KEY_DERIVATION`             | 0x00000392 |
| `CKM_SHA256_KEY_DERIVATION`           | 0x00000393 |
| `CKM_SHA384_KEY_DERIVATION`           | 0x00000394 |
| `CKM_SHA512_KEY_DERIVATION`           | 0x00000395 |
| `CKM_SHA224_KEY_DERIVATION`           | 0x00000396 |
| `CKM_PBE_MD2_DES_CBC`                 | 0x000003A0 |
| `CKM_PBE_MD5_DES_CBC`                 | 0x000003A1 |
| `CKM_PBE_MD5_CAST_CBC`                | 0x000003A2 |
| `CKM_PBE_MD5_CAST3_CBC`               | 0x000003A3 |
| `CKM_PBE_MD5_CAST5_CBC`               | 0x000003A4 |
| `CKM_PBE_MD5_CAST128_CBC`             | 0x000003A4 |
| `CKM_PBE_SHA1_CAST5_CBC`              | 0x000003A5 |
| `CKM_PBE_SHA1_CAST128_CBC`            | 0x000003A5 |
| `CKM_PBE_SHA1_RC4_40`                 | 0x000003A7 |
| `CKM_PBE_SHA1_RC4_128`                | 0x000003A6 |
| `CKM_PBE_SHA1_DES3_EDE_CBC`           | 0x000003A8 |
| `CKM_PBE_SHA1_DES2_EDE_CBC`           | 0x000003A9 |
| `CKM_PBE_SHA1_RC2_128_CBC`            | 0x000003AA |
| `CKM_PBE_SHA1_RC2_40_CBC`             | 0x000003AB |
| `CKM_PKCS5_PBKD2`                     | 0x000003B0 |
| `CKM_PBA_SHA1_WITH_SHA1_HMAC`         | 0x000003C0 |
| `CKM_WTLS_PRE_MASTER_KEY_GEN`         | 0x000003D0 |
| `CKM_WTLS_MASTER_KEY_DERIVE_DH_ECC`   | 0x000003D2 |
| `CKM_WTLS_MASTER_KEY_DERIVE`          | 0x000003D1 |
| `CKM_WTLS_PRF`                        | 0x000003D3 |
| `CKM_WTLS_SERVER_KEY_AND_MAC_DERIVE`  | 0x000003D4 |
| `CKM_WTLS_CLIENT_KEY_AND_MAC_DERIVE`  | 0x000003D5 |
| `CKM_KEY_WRAP_LYNKS`                  | 0x00000400 |
| `CKM_KEY_WRAP_SET_OAEP`               | 0x00000401 |
| `CKM_CMS_SIG`                         | 0x00000500 |
| `CKM_KIP_DERIVE`	                    | 0x00000510 |
| `CKM_KIP_WRAP`	                    | 0x00000511 |
| `CKM_KIP_MAC`	                        | 0x00000512 |
| `CKM_CAMELLIA_KEY_GEN`                | 0x00000550 |
| `CKM_CAMELLIA_ECB`                    | 0x00000551 |
| `CKM_CAMELLIA_CBC`                    | 0x00000552 |
| `CKM_CAMELLIA_MAC`                    | 0x00000553 |
| `CKM_CAMELLIA_MAC_GENERAL`            | 0x00000554 |
| `CKM_CAMELLIA_CBC_PAD`                | 0x00000555 |
| `CKM_CAMELLIA_ECB_ENCRYPT_DATA`       | 0x00000556 |
| `CKM_CAMELLIA_CBC_ENCRYPT_DATA`       | 0x00000557 |
| `CKM_CAMELLIA_CTR`                    | 0x00000558 |
| `CKM_ARIA_KEY_GEN`                    | 0x00000560 |
| `CKM_ARIA_ECB`                        | 0x00000561 |
| `CKM_ARIA_CBC`                        | 0x00000562 |
| `CKM_ARIA_MAC`                        | 0x00000563 |
| `CKM_ARIA_MAC_GENERAL`                | 0x00000564 |
| `CKM_ARIA_CBC_PAD`                    | 0x00000565 |
| `CKM_ARIA_ECB_ENCRYPT_DATA`           | 0x00000566 |
| `CKM_ARIA_CBC_ENCRYPT_DATA`           | 0x00000567 |
| `CKM_SEED_KEY_GEN`                    | 0x00000650 |
| `CKM_SEED_ECB`                        | 0x00000651 |
| `CKM_SEED_CBC`                        | 0x00000652 |
| `CKM_SEED_MAC`                        | 0x00000653 |
| `CKM_SEED_MAC_GENERAL`                | 0x00000654 |
| `CKM_SEED_CBC_PAD`                    | 0x00000655 |
| `CKM_SEED_ECB_ENCRYPT_DATA`           | 0x00000656 |
| `CKM_SEED_CBC_ENCRYPT_DATA`           | 0x00000657 |
| `CKM_SKIPJACK_KEY_GEN`                | 0x00001000 |
| `CKM_SKIPJACK_ECB64`                  | 0x00001001 |
| `CKM_SKIPJACK_CBC64`                  | 0x00001002 |
| `CKM_SKIPJACK_OFB64`                  | 0x00001003 |
| `CKM_SKIPJACK_CFB64`                  | 0x00001004 |
| `CKM_SKIPJACK_CFB32`                  | 0x00001005 |
| `CKM_SKIPJACK_CFB8`                   | 0x00001007 |
| `CKM_SKIPJACK_CFB16`                  | 0x00001006 |
| `CKM_SKIPJACK_WRAP`                   | 0x00001008 |
| `CKM_SKIPJACK_PRIVATE_WRAP`           | 0x00001009 |
| `CKM_SKIPJACK_RELAYX`                 | 0x0000100a |
| `CKM_KEA_KEY_PAIR_GEN`                | 0x00001010 |
| `CKM_KEA_KEY_DERIVE`                  | 0x00001011 |
| `CKM_FORTEZZA_TIMESTAMP`              | 0x00001020 |
| `CKM_BATON_KEY_GEN`                   | 0x00001030 |
| `CKM_BATON_ECB128`                    | 0x00001031 |
| `CKM_BATON_ECB96`                     | 0x00001032 |
| `CKM_BATON_CBC128`                    | 0x00001033 |
| `CKM_BATON_COUNTER`                   | 0x00001034 |
| `CKM_BATON_SHUFFLE`                   | 0x00001035 |
| `CKM_BATON_WRAP`                      | 0x00001036 |
| `CKM_ECDSA_KEY_PAIR_GEN`              | 0x00001040 |
| `CKM_EC_KEY_PAIR_GEN`                 | 0x00001040 |
| `CKM_ECDSA`                           | 0x00001041 |
| `CKM_ECDSA_SHA1`                      | 0x00001042 |
| `CKM_ECDSA_SHA224`                    | 0x00001043 |
| `CKM_ECDSA_SHA256`                    | 0x00001044 |
| `CKM_ECDSA_SHA384`                    | 0x00001045 |
| `CKM_ECDSA_SHA512`                    | 0x00001046 |
| `CKM_ECDH1_DERIVE`                    | 0x00001050 |
| `CKM_ECMQV_DERIVE`                    | 0x00001052 |
| `CKM_ECDH1_COFACTOR_DERIVE`           | 0x00001051 |
| `CKM_JUNIPER_KEY_GEN`                 | 0x00001060 |
| `CKM_JUNIPER_ECB128`                  | 0x00001061 |
| `CKM_JUNIPER_CBC128`                  | 0x00001062 |
| `CKM_JUNIPER_COUNTER`                 | 0x00001063 |
| `CKM_JUNIPER_SHUFFLE`                 | 0x00001064 |
| `CKM_JUNIPER_WRAP`                    | 0x00001065 |
| `CKM_FASTHASH`                        | 0x00001070 |
| `CKM_AES_KEY_GEN`                     | 0x00001080 |
| `CKM_AES_ECB`                         | 0x00001081 |
| `CKM_AES_CBC`                         | 0x00001082 |
| `CKM_AES_MAC`                         | 0x00001083 |
| `CKM_AES_MAC_GENERAL`                 | 0x00001084 |
| `CKM_AES_CBC_PAD`                     | 0x00001085 |
| `CKM_AES_CTR`                         | 0x00001086 |
| `CKM_AES_CTS`                         | 0x00001089 |
| `CKM_AES_CMAC`                        | 0x0000108A |
| `CKM_AES_CMAC_GENERAL`                | 0x0000108B |
| `CKM_BLOWFISH_KEY_GEN`                | 0x00001090 |
| `CKM_BLOWFISH_CBC`                    | 0x00001091 |
| `CKM_TWOFISH_KEY_GEN`                 | 0x00001092 |
| `CKM_TWOFISH_CBC`                     | 0x00001093 |
| `CKM_AES_GCM`                         | 0x00001087 |
| `CKM_AES_CCM`                         | 0x00001088 |
| `CKM_AES_KEY_WRAP`                    | 0x00001090 |
| `CKM_AES_KEY_WRAP_PAD`                | 0x00001091 |
| `CKM_TWOFISH_CBC_PAD`                 | 0x00001095 |
| `CKM_BLOWFISH_CBC_PAD`                | 0x00001094 |
| `CKM_DES_ECB_ENCRYPT_DATA`            | 0x00001100 |
| `CKM_DES_CBC_ENCRYPT_DATA`            | 0x00001101 |
| `CKM_DES3_ECB_ENCRYPT_DATA`           | 0x00001102 |
| `CKM_DES3_CBC_ENCRYPT_DATA`           | 0x00001103 |
| `CKM_AES_ECB_ENCRYPT_DATA`            | 0x00001104 |
| `CKM_AES_CBC_ENCRYPT_DATA`            | 0x00001105 |
| `CKM_GOSTR3410_KEY_PAIR_GEN`          | 0x00001200 |
| `CKM_GOSTR3410`                       | 0x00001201 |
| `CKM_GOSTR3410_WITH_GOSTR3411`        | 0x00001202 |
| `CKM_GOSTR3410_KEY_WRAP`              | 0x00001203 |
| `CKM_GOSTR3410_DERIVE`                | 0x00001204 |
| `CKM_GOSTR3411`                       | 0x00001210 |
| `CKM_GOSTR3411_HMAC`                  | 0x00001211 |
| `CKM_GOST28147_KEY_GEN`               | 0x00001220 |
| `CKM_GOST28147_ECB`                   | 0x00001221 |
| `CKM_GOST28147`                       | 0x00001222 |
| `CKM_GOST28147_MAC`                   | 0x00001223 |
| `CKM_GOST28147_KEY_WRAP`              | 0x00001224 |
| `CKM_DSA_PARAMETER_GEN`               | 0x00002000 |
| `CKM_DH_PKCS_PARAMETER_GEN`           | 0x00002001 |
| `CKM_X9_42_DH_PARAMETER_GEN`          | 0x00002002 |
| `CKM_AES_OFB`                         | 0x00002104 |
| `CKM_AES_CFB8`                        | 0x00002106 |
| `CKM_AES_CFB64`                       | 0x00002105 |
| `CKM_AES_CFB128`                      | 0x00002107 |
| `CKM_RSA_PKCS_TPM_1_1`                | 0x00004001 |
| `CKM_RSA_PKCS_OAEP_TPM_1_1`           | 0x00004002 |
| `CKM_VENDOR_DEFINED`                  | 0x80000000 |
