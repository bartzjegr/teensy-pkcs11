# Teensy PKCS Command

Commands are encoded in [msgpack](https://github.com/msgpack/msgpack/blob/master/spec.md) encoding

## Table Of Contents
- [Functions](#functions)
    - [Slot and Token Management Functions](#slot-and-token-management-functions)
        - [Get Slot Info](#get-slot-info)
        - [Get Token Info](#get-token-info)
        - [Get Mechanism List](#get-mechanism-list)
        - [Get Mechanism Info](#get-mechanism-info)
        - [Init Token](#init-token)
        - [Init PIN](#init-pin)
        - [Set PIN](#set-pin)
    - [Session Management Functions](#session-management-functions)
        - [Open Session](#open-session)
        - [Close Session](#close-session)
        - [Close All Sessions](#close-all-sessions)
        - [Get Session Info](#get-session-info)
        - [Get Operation State](#get-operation-state)
        - [Set Operation State](#set-operation-state)
        - [Login](#login)
        - [Logout](#logout)
    - [Object Management Functions](#object-management-functions)
        - [Create Object](#create-object)
        - [Copy Object](#copy-object)
        - [Destroy Object](#destroy-object)
        - [Get Object Size](#get-object-size)
        - [Get Attribute Value](#get-attribute-value)
        - [Set Attribute Value](#set-attribute-value)
        - [Find Objects Init](#find-objects-init)
        - [Find Objects](#find-objects)
        - [Find Objects Final](#find-objects-final)
    - [Encryption Functions](#encryption-functions)
        - [Encrypt Init](#encrypt-init)
        - [Encrypt](#encrypt)
        - [Encrypt Update](#encrypt-update)
        - [Encrypt Final](#encrypt-final)
    - [Decryption Functions](#decryption-functions)
        - [Decrypt Init](#decrypt-init)
        - [Decrypt](#decrypt)
        - [Decrypt Update](#decrypt-update)
        - [Decrypt Final](#decrypt-final)
    - [Message digesting Functions](#message-digesting-functions)
        - [Digest Init](#digest-init)
        - [Digest](#digest)
        - [Digest Update](#digest-update)
        - [Digest Key](#digest-key)
        - [Digest Final](#digest-final)
    - [Signing and MACing Functions](#signing-and-macing-functions)
        - [Sign Init](#sign-init)
        - [Sign](#sign)
        - [Sign Update](#sign-update)
        - [Sign Final](#sign-final)
        - [Sign Recover Init](#sign-recover-init)
        - [Sign Recover](#sign-recover)
    - [Functions for Verifying signatures and MACs](#functions-for-verifying-signatures-and-macs)
        - [Verify Init](#verify-init)
        - [Verify](#verify)
        - [Verify Update](#verify-update)
        - [Verify Final](#verify-final)
        - [Verify Recover Init](#verify-recover-init)
        - [Verify Recover](#verify-recover)
    - [Dual-function Cryptographic Functions](#dual-function-cryptographic-functions)
        - [Digest Encrypt Update](#digest-encrypt-update)
        - [Decrypt Digest Update](#decrypt-digest-update)
        - [Sign Encrypt Update](#sign-encrypt-update)
        - [Decrypt Verify Update](#decrypt-verify-update)
    - [Key Management Functions](#key-management-functions)
        - [Generate Key](#generate-key)
        - [Generate Key Pair](#generate-key-pair)
        - [Wrap Key](#wrap-key)
        - [Unwrap Key](#unwrap-key)
        - [Derive Key](#derive-key)
    - [Random Number Generation Functions](#random-number-generation-functions)
        - [Seed Random](#seed-random)
        - [Generate Random](#generate-random)
- [Types](#types)
    - [Return Value](#return-value)
    - [Mechanism Codes](#mechanism-codes)
    - [Attributes](#attributes)
    - [Flags](#flags)

## Functions

### Slot and Token Management Functions

#### Get Slot Info

Obtains information about a particular slot in the system.

**Request**

None

**Response**

| Name            | Type                   | Representation | Description                            |
|-----------------|------------------------|----------------|----------------------------------------|
| status          | [CK_RV](#return-value) | uint 8/16/32   | Return value                           |
| slotDescription | octet-stream           | bin 8          | slot description (64 chars)            |
| manufacturerID  | octet-stream           | bin 8          | manufacturer identification (32 chars) |
| flags           | CK_FLAGS               | uint 8/16/32   | Token [flags](#flags)                  |
| hardwareVersion | (CK_BYTE, CK_BYTE)     | fixarray       | Hardware version (major, minor)        |
| firmwareVersion | (CK_BYTE, CK_BYTE)     | fixarray       | Firmware version (major, minor)        |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OK`
- `CKR_SLOT_ID_INVALID`

#### Get Token Info

Obtains information about a particular token in the system.

**Request**

None

**Response**

| Name                  | Type                   | Representation | Description                                  |
|-----------------------|------------------------|----------------|----------------------------------------------|
| status                | [CK_RV](#return-value) | uint 8/16/32   | Return value                                 |
| label                 | octet-stream           | bin 8          | Token slot label (32 chars)                  |
| manufacturerID        | octet-stream           | bin 8          | Token manufacturer identification (32 chars) |
| model                 | octet-stream           | bin 8          | Token model (16 bytes)                       |
| serialNumber          | octet-stream           | bin 8          | Token serial number (16 bytes)               |
| flags                 | CK_FLAGS               | uint 8/16/32   | Token [flags](#flags)                        |
| ulMaxSessionCount     | CK_ULONG               | uint 8/16/32   | Maximum session count                        |
| ulSessionCount        | CK_ULONG               | uint 8/16/32   | Number of open session                       |
| ulMaxRwSessionCount   | CK_ULONG               | uint 8/16/32   | Maximum number of read/write sessions that can be opened with the token at one time     |  
| ulRwSessionCount      | CK_ULONG               | uint 8/16/32   | Number of read/write sessions that this application currently has open with the token   |
| ulMaxPinLen           | CK_ULONG               | uint 8/16/32   | Maximum length in bytes of the PIN                                                      |
| ulMinPinLen           | CK_ULONG               | uint 8/16/32   | Minimum length in bytes of the PIN                                                      |
| ulTotalPublicMemory   | CK_ULONG               | uint 8/16/32   | The total amount of memory on the token in bytes in which public objects may be stored  |
| ulFreePublicMemory    | CK_ULONG               | uint 8/16/32   | The amount of free (unused) memory on the token in bytes for public objects             |
| ulTotalPrivateMemory  | CK_ULONG               | uint 8/16/32   | The total amount of memory on the token in bytes in which private objects may be stored |
| ulFreePrivateMemory   | CK_ULONG               | uint 8/16/32   | The amount of free (unused) memory on the token in bytes for private objects            |
| hardwareVersion       | (CK_BYTE, CK_BYTE)     | fixarray       | Version number of hardware.                   |
| firmwareVersion       | (CK_BYTE, CK_BYTE)     | fixarray       | Version number of firmware.                   |
| utcTime               | octet-stream           | bin 8          | Current time as a character-string of length 16, represented in the format YYYYMMDDhhmmssxx (4 characters for the year; 2 characters each for the month, the day, the hour, the minute, and the second; and 2 additional reserved '0' characters) |

**Error Codes**

- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OK`
- `CKR_SLOT_ID_INVALID`
- `CKR_TOKEN_NOT_PRESENT`
- `CKR_TOKEN_NOT_RECOGNIZED`
- `CKR_ARGUMENTS_BAD`

#### Get Mechanism List

 Used to obtain a list of mechanism types supported by a token.

 **Request**

 None

 **Response**

 | Name            | Type                                           | Representation | Description                            |
 |-----------------|------------------------------------------------|----------------|----------------------------------------|
 | status          | [CK_RV](#return-value)                         | uint 8/16/32   | Return value                           |
 | slotDescription | array of [CK_MECHANISM_TYPE](#mechanism-codes) | array 8/16     | slot description (64 chars)            |

 **Error Codes**

 - `CKR_BUFFER_TOO_SMALL`
 - `CKR_CRYPTOKI_NOT_INITIALIZED`
 - `CKR_DEVICE_ERROR`
 - `CKR_DEVICE_MEMORY`
 - `CKR_DEVICE_REMOVED`
 - `CKR_FUNCTION_FAILED`
 - `CKR_GENERAL_ERROR`
 - `CKR_HOST_MEMORY`
 - `CKR_OK`
 - `CKR_SLOT_ID_INVALID`
 - `CKR_TOKEN_NOT_PRESENT`
 - `CKR_TOKEN_NOT_RECOGNIZED`
 - `CKR_ARGUMENTS_BAD`


#### Get Mechanism Info

Obtains information about a particular mechanism possibly supported by a token.

**Request**

| Name | Type                                  | Representation | Description           |
|------|---------------------------------------|----------------|-----------------------|
| type | [CK_MECHANISM_TYPE](#mechanism-codes) | uint 8/16/32   | Mechanism identifier  |

**Response**

| Name         | Type                   | Representation | Description                                 |
|--------------|------------------------|----------------|---------------------------------------------|
| status       | [CK_RV](#return-value) | uint 8/16/32   | Return value                                |
| ulMinKeySize | CK_ULONG               | uint 8/16/32   | The minimum size of the key for the mechanism (whether this is measured in bits or in bytes is mechanism-dependent) |
| ulMaxKeySize | CK_ULONG               | uint 8/16/32   | The maximum size of the key for the mechanism (whether this is measured in bits or in bytes is mechanism-dependent) |
| flags        | [CK_FLAGS](#flags)     | uint 8/16/32   | Bit flags specifying mechanism capabilities |

**Error Codes**

- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_MECHANISM_INVALID`
- `CKR_OK`
- `CKR_SLOT_ID_INVALID`
- `CKR_TOKEN_NOT_PRESENT`
- `CKR_TOKEN_NOT_RECOGNIZED`
- `CKR_ARGUMENTS_BAD`

#### Init Token

#### Init PIN

#### Set PIN

### Session Management Functions

#### Open Session

#### Close Session

#### Close All Sessions

#### Get Session Info

#### Get Operation State

#### Set Operation State

#### Login

#### Logout

### Object Management Functions

#### Create Object

#### Copy Object

#### Destroy Object

#### Get Object Size

#### Get Attribute Value

#### Set Attribute Value

#### Find Objects Init

#### Find Objects

#### Find Objects Final

### Encryption Functions

#### Encrypt Init

Initializes an encryption operation.

The `CKA_ENCRYPT` attribute of the encryption key, which indicates whether the key supports encryption, must be `CK_TRUE`.

After calling [EncryptInit](#encrypt-init), the application can either call [Encrypt](#encrypt) to encrypt data in a single part; or call [EncryptUpdate](#encrypt-update) zero or more times, followed by [EncryptFinal](#encrypt-final), to encrypt data in multiple parts. The encryption operation is active until the application uses a call to [Encrypt](#encrypt) or [EncryptFinal](#encrypt-final) to actually obtain the final piece of ciphertext. To process additional data (in single or multiple parts), the application must call [EncryptInit](#encrypt-init) again.

**Request**

| Name      | Type                                  | Representation | Description        |
|-----------|---------------------------------------|----------------|--------------------|
| hSession  | CK_SESSION_HANDLE                     | uint 8/16/32   | Session handle     |
| mechanism | [CK_MECHANISM_TYPE](#mechanism-codes) | uint 8/16/32   | Cipher mechanism   |
| parameter | byte array                            | bin 8/16/32    | Optional parameter |
| hKey      | CK_OBJECT_HANDLE                      | uint 8/16/32   | Key Handle         |

**Response**

| Name   | Type                   | Representation | Description  |
|--------|------------------------|----------------|--------------|
| status | [CK_RV](#return-value) | uint 8/16/32   | Return value |

**Mechanism Codes**

- `CKM_RSA_PKCS`
- `CKM_RSA_PKCS_OAEP`
- `CKM_RSA_X_509`
- `CKM_RC2_ECB`
- `CKM_RC2_CBC`
- `CKM_RC2_CBC_PAD`
- `CKM_RC4`
- `CKM_DES_ECB`
- `CKM_DES_CBC`
- `CKM_DES_CBC_PAD`
- `CKM_DES3_ECB`
- `CKM_DES3_CBC`
- `CKM_DES3_CBC_PAD`
- `CKM_CDMF_ECB`
- `CKM_CDMF_CBC`
- `CKM_CDMF_CBC_PAD`
- `CKM_DES_OFB64`
- `CKM_DES_OFB8`
- `CKM_DES_CFB64`
- `CKM_DES_CFB8`
- `CKM_CAST_ECB`
- `CKM_CAST_CBC`
- `CKM_CAST_CBC_PAD`
- `CKM_CAST3_KEY_GEN`
- `CKM_CAST3_ECB`
- `CKM_CAST3_CBC`
- `CKM_CAST3_CBC_PAD`
- `CKM_CAST5_ECB`
- `CKM_CAST128_ECB`
- `CKM_CAST5_CBC`
- `CKM_CAST128_CBC`
- `CKM_CAST5_CBC_PAD`
- `CKM_CAST128_CBC_PAD`
- `CKM_RC5_ECB`
- `CKM_RC5_CBC`
- `CKM_RC5_CBC_PAD`
- `CKM_IDEA_ECB`
- `CKM_IDEA_CBC`
- `CKM_IDEA_CBC_PAD`
- `CKM_CAMELLIA_ECB`
- `CKM_CAMELLIA_CBC`
- `CKM_CAMELLIA_MAC`
- `CKM_CAMELLIA_CBC_PAD`
- `CKM_CAMELLIA_CTR`
- `CKM_ARIA_ECB`
- `CKM_ARIA_CBC`
- `CKM_ARIA_CBC_PAD`
- `CKM_SEED_ECB`
- `CKM_SEED_CBC`
- `CKM_SEED_CBC_PAD`
- `CKM_SKIPJACK_ECB64`
- `CKM_SKIPJACK_CBC64`
- `CKM_SKIPJACK_OFB64`
- `CKM_SKIPJACK_CFB64`
- `CKM_SKIPJACK_CFB32`
- `CKM_SKIPJACK_CFB16`
- `CKM_SKIPJACK_CFB8`
- `CKM_BATON_ECB128`
- `CKM_BATON_ECB96`
- `CKM_BATON_CBC128`
- `CKM_BATON_COUNTER`
- `CKM_BATON_SHUFFLE`
- `CKM_JUNIPER_ECB128`
- `CKM_JUNIPER_CBC128`
- `CKM_JUNIPER_COUNTER`
- `CKM_JUNIPER_SHUFFLE`
- `CKM_AES_ECB`
- `CKM_AES_CBC`
- `CKM_AES_CBC_PAD`
- `CKM_AES_CTR`
- `CKM_AES_CTS`
- `CKM_BLOWFISH_CBC`
- `CKM_TWOFISH_CBC`
- `CKM_AES_GCM`
- `CKM_AES_CCM`
- `CKM_BLOWFISH_CBC_PAD`
- `CKM_TWOFISH_CBC_PAD`
- `CKM_GOST28147_ECB`
- `CKM_GOST28147`
- `CKM_AES_OFB`
- `CKM_AES_CFB64`
- `CKM_AES_CFB8`
- `CKM_AES_CFB128`
- `CKM_RSA_PKCS_TPM_1_1`
- `CKM_RSA_PKCS_OAEP_TPM_1_1`

**Error Codes**

- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_CANCELED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_KEY_FUNCTION_NOT_PERMITTED`
- `CKR_KEY_HANDLE_INVALID`
- `CKR_KEY_SIZE_RANGE`
- `CKR_KEY_TYPE_INCONSISTENT`
- `CKR_MECHANISM_INVALID`
- `CKR_MECHANISM_PARAM_INVALID`
- `CKR_OK`
- `CKR_OPERATION_ACTIVE`
- `CKR_PIN_EXPIRED`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`
- `CKR_USER_NOT_LOGGED_IN`

#### Encrypt

Encrypts single-part data.

The encryption operation must have been initialized with [EncryptInit](#encrypt-init). A call to [Encrypt](#encrypt) always terminates the active encryption operation unless it returns `CKR_BUFFER_TOO_SMALL` or is a successful call (i.e., one which returns `CKR_OK`) to determine the length of the buffer needed to hold the ciphertext.

[Encrypt](#encrypt) can not be used to terminate a multi-part operation, and must be called after [EncryptInit](#encrypt-init) without intervening [EncryptUpdate](#encrypt-update) calls.

For some encryption mechanisms, the input plaintext data has certain length constraints (either because the mechanism can only encrypt relatively short pieces of plaintext, or because the mechanism's input data must consist of an integral number of blocks). If these constraints are not satisfied, then [Encrypt](#encrypt) will fail with return code `CKR_DATA_LEN_RANGE`.

For most mechanisms, [Encrypt](#encrypt) is equivalent to a sequence of [EncryptUpdate](#encrypt-update) operations followed by [EncryptFinal](#encrypt-final).

**Request**

| Name      | Type              | Representation | Description          |
|-----------|-------------------|----------------|----------------------|
| hSession  | CK_SESSION_HANDLE | uint 8/16/32   | Session handle       |
| data      | byte array        | bin 8/16/32    | Data to be encrypted |

**Response**

| Name          | Type                   | Representation | Description    |
|---------------|------------------------|----------------|----------------|
| status        | [CK_RV](#return-value) | uint 8/16/32   | Return value   |
| encryptedData | byte array             | bin 8/16/32    | Encrypted data |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
- `CKR_BUFFER_TOO_SMALL`
- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DATA_INVALID`
- `CKR_DATA_LEN_RANGE`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_CANCELED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OK`
- `CKR_OPERATION_NOT_INITIALIZED`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`


#### Encrypt Update

Continues a multiple-part encryption operation, processing another data part.

The encryption operation must have been initialized with [EncryptInit](#encrypt-init). This function may be called any number of times in succession. A call to [EncryptUpdate](#encrypt-update) which results in an error other than `CKR_BUFFER_TOO_SMALL` terminates the current encryption operation.

**Request**

| Name      | Type              | Representation | Description               |
|-----------|-------------------|----------------|---------------------------|
| hSession  | CK_SESSION_HANDLE | uint 8/16/32   | Session handle            |
| part      | byte array        | bin 8/16/32    | Data part to be encrypted |

**Response**

| Name          | Type                   | Representation | Description            |
|---------------|------------------------|----------------|------------------------|
| status        | [CK_RV](#return-value) | uint 8/16/32   | Return value           |
| encryptedPart | byte array             | bin 8/16/32    | Encrypted part of data |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
- `CKR_BUFFER_TOO_SMALL`
- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DATA_LEN_RANGE`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_CANCELED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OK`
- `CKR_OPERATION_NOT_INITIALIZED`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`

#### Encrypt Final

Finishes a multiple-part encryption operation.

The encryption operation must have been initialized with [EncryptInit](#encrypt-init). A call to [EncryptFinal](#encrypt-final) always terminates the active encryption operation unless it returns `CKR_BUFFER_TOO_SMALL` or is a successful call (i.e., one which returns `CKR_OK`) to determine the length of the buffer needed to hold the ciphertext.

For some multi-part encryption mechanisms, the input plaintext data has certain length constraints, because the mechanism's input data must consist of an integral number of blocks. If these constraints are not satisfied, then [EncryptFinal](#encrypt-final) will fail with return code `CKR_DATA_LEN_RANGE`.

**Request**

| Name      | Type              | Representation | Description    |
|-----------|-------------------|----------------|----------------|
| hSession  | CK_SESSION_HANDLE | uint 8/16/32   | Session handle |

**Response**

| Name              | Type                   | Representation | Description                 |
|-------------------|------------------------|----------------|-----------------------------|
| status            | [CK_RV](#return-value) | uint 8/16/32   | Return value                |
| lastEncryptedPart | byte array             | bin 8/16/32    | Last encrypted part of data |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
- `CKR_BUFFER_TOO_SMALL`
- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DATA_LEN_RANGE`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_CANCELED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OK`
- `CKR_OPERATION_NOT_INITIALIZED`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`

### Decryption Functions

#### Decrypt Init

Initializes a decryption operation.

The `CKA_DECRYPT` attribute of the decryption key, which indicates whether the key supports decryption, must be `CK_TRUE`.

After calling [DecryptInit](#decrypt-init), the application can either call [Decrypt](#decrypt) to decrypt data in a single part; or call [DecryptUpdate](#decrypt-update) zero or more times, followed by [DecryptFinal](#decrypt-final), to decrypt data in multiple parts. The decryption operation is active until the application uses a call to [Decrypt](#decrypt) or [DecryptFinal](#decrypt-final) to actually obtain the final piece of plaintext. To process additional data (in single or multiple parts), the application must call [DecryptInit](#decrypt-init) again

**Request**

| Name      | Type                                  | Representation | Description        |
|-----------|---------------------------------------|----------------|--------------------|
| hSession  | CK_SESSION_HANDLE                     | uint 8/16/32   | Session handle     |
| mechanism | [CK_MECHANISM_TYPE](#mechanism-codes) | uint 8/16/32   | Cipher mechanism   |
| parameter | byte array                            | bin 8/16/32    | Optional parameter |
| hKey      | CK_OBJECT_HANDLE                      | uint 8/16/32   | Key Handle         |

**Response**

| Name   | Type                   | Representation | Description  |
|--------|------------------------|----------------|--------------|
| status | [CK_RV](#return-value) | uint 8/18/32   | Return value |

**Mechanism Codes**

- `CKM_RSA_PKCS`
- `CKM_RSA_PKCS_OAEP`
- `CKM_RSA_X_509`
- `CKM_RC2_ECB`
- `CKM_RC2_CBC`
- `CKM_RC2_CBC_PAD`
- `CKM_RC4`
- `CKM_DES_ECB`
- `CKM_DES_CBC`
- `CKM_DES_CBC_PAD`
- `CKM_DES3_ECB`
- `CKM_DES3_CBC`
- `CKM_DES3_CBC_PAD`
- `CKM_CDMF_ECB`
- `CKM_CDMF_CBC`
- `CKM_CDMF_CBC_PAD`
- `CKM_DES_OFB64`
- `CKM_DES_OFB8`
- `CKM_DES_CFB64`
- `CKM_DES_CFB8`
- `CKM_CAST_ECB`
- `CKM_CAST_CBC`
- `CKM_CAST_CBC_PAD`
- `CKM_CAST3_KEY_GEN`
- `CKM_CAST3_ECB`
- `CKM_CAST3_CBC`
- `CKM_CAST3_CBC_PAD`
- `CKM_CAST5_ECB`
- `CKM_CAST128_ECB`
- `CKM_CAST5_CBC`
- `CKM_CAST128_CBC`
- `CKM_CAST5_CBC_PAD`
- `CKM_CAST128_CBC_PAD`
- `CKM_RC5_ECB`
- `CKM_RC5_CBC`
- `CKM_RC5_CBC_PAD`
- `CKM_IDEA_ECB`
- `CKM_IDEA_CBC`
- `CKM_IDEA_CBC_PAD`
- `CKM_CAMELLIA_ECB`
- `CKM_CAMELLIA_CBC`
- `CKM_CAMELLIA_MAC`
- `CKM_CAMELLIA_CBC_PAD`
- `CKM_CAMELLIA_CTR`
- `CKM_ARIA_ECB`
- `CKM_ARIA_CBC`
- `CKM_ARIA_CBC_PAD`
- `CKM_SEED_ECB`
- `CKM_SEED_CBC`
- `CKM_SEED_CBC_PAD`
- `CKM_SKIPJACK_ECB64`
- `CKM_SKIPJACK_CBC64`
- `CKM_SKIPJACK_OFB64`
- `CKM_SKIPJACK_CFB64`
- `CKM_SKIPJACK_CFB32`
- `CKM_SKIPJACK_CFB16`
- `CKM_SKIPJACK_CFB8`
- `CKM_BATON_ECB128`
- `CKM_BATON_ECB96`
- `CKM_BATON_CBC128`
- `CKM_BATON_COUNTER`
- `CKM_BATON_SHUFFLE`
- `CKM_JUNIPER_ECB128`
- `CKM_JUNIPER_CBC128`
- `CKM_JUNIPER_COUNTER`
- `CKM_JUNIPER_SHUFFLE`
- `CKM_AES_ECB`
- `CKM_AES_CBC`
- `CKM_AES_CBC_PAD`
- `CKM_AES_CTR`
- `CKM_AES_CTS`
- `CKM_BLOWFISH_CBC`
- `CKM_TWOFISH_CBC`
- `CKM_AES_GCM`
- `CKM_AES_CCM`
- `CKM_BLOWFISH_CBC_PAD`
- `CKM_TWOFISH_CBC_PAD`
- `CKM_GOST28147_ECB`
- `CKM_GOST28147`
- `CKM_AES_OFB`
- `CKM_AES_CFB64`
- `CKM_AES_CFB8`
- `CKM_AES_CFB128`
- `CKM_RSA_PKCS_TPM_1_1`
- `CKM_RSA_PKCS_OAEP_TPM_1_1`

**Error Codes**

- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_CANCELED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_KEY_FUNCTION_NOT_PERMITTED`
- `CKR_KEY_HANDLE_INVALID`
- `CKR_KEY_SIZE_RANGE`
- `CKR_KEY_TYPE_INCONSISTENT`
- `CKR_MECHANISM_INVALID`
- `CKR_MECHANISM_PARAM_INVALID`
- `CKR_OK`
- `CKR_OPERATION_ACTIVE`
- `CKR_PIN_EXPIRED`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`
- `CKR_USER_NOT_LOGGED_IN`

#### Decrypt

Decrypts encrypted data in a single part.

The decryption operation must have been initialized with [DecryptInit](#decrypt-init). A call to [Decrypt](#decrypt) always terminates the active decryption operation unless it returns `CKR_BUFFER_TOO_SMALL` or is a successful call (i.e., one which returns `CKR_OK`) to determine the length of the buffer needed to hold the plaintext.

[Decrypt](#decrypt) can not be used to terminate a multi-part operation, and must be called after [DecryptInit](#decrypt-init) without intervening [DecryptUpdate](#decrypt-update) calls.

If the input ciphertext data cannot be decrypted because it has an inappropriate length, then either `CKR_ENCRYPTED_DATA_INVALID` or `CKR_ENCRYPTED_DATA_LEN_RANGE` may be returned.

For most mechanisms, [Decrypt](#decrypt) is equivalent to a sequence of [DecryptUpdate](#decrypt-update) operations followed by [DecryptFinal](#decrypt-final).

**Request**

| Name          | Type              | Representation | Description          |
|---------------|-------------------|----------------|----------------------|
| hSession      | CK_SESSION_HANDLE | uint 8/16/32   | Session handle       |
| encryptedData | byte array        | bin 8/16/32    | Data to be decrypted |

**Response**

| Name          | Type                   | Representation | Description          |
|---------------|------------------------|----------------|----------------------|
| status        | [CK_RV](#return-value) | uint 8/16/32   | Return value         |
| data          | byte array             | bin 8/16/32    | Decrypted data       |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
- `CKR_BUFFER_TOO_SMALL`
- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_ENCRYPTED_DATA_INVALID`
- `CKR_ENCRYPTED_DATA_LEN_RANGE`
- `CKR_FUNCTION_CANCELED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OK`
- `CKR_OPERATION_NOT_INITIALIZED`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`
- `CKR_USER_NOT_LOGGED_IN`

#### Decrypt Update

Continues a multiple-part decryption operation, processing another encrypted data part.

The decryption operation must have been initialized with [DecryptInit](#decrypt-init). This function may be called any number of times in succession. A call to [DecryptUpdate](#decrypt-update) which results in an error other than `CKR_BUFFER_TOO_SMALL` terminates the current decryption operation.

**Request**

| Name          | Type              | Representation | Description         |
|---------------|-------------------|----------------|---------------------|
| hSession      | CK_SESSION_HANDLE | uint 8/16/32   | Session handle      |
| encryptedPart | byte array        | bin 8/16/32    | Encrypted data part |

**Response**

| Name   | Type                   | Representation | Description         |
|--------|------------------------|----------------|---------------------|
| status | [CK_RV](#return-value) | uint 8/16/32   | Return value        |
| part   | byte array             | bin 8/16/32    | Decrypted data part |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
- `CKR_BUFFER_TOO_SMALL`
- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_ENCRYPTED_DATA_INVALID`
- `CKR_ENCRYPTED_DATA_LEN_RANGE`
- `CKR_FUNCTION_CANCELED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OK`
- `CKR_OPERATION_NOT_INITIALIZED`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`
- `CKR_USER_NOT_LOGGED_IN`

#### Decrypt Final

Finishes a multiple-part decryption operation.

The decryption operation must have been initialized with [DecryptInit](#decrypt-init). A call to [DecryptFinal](#decrypt-final) always terminates the active decryption operation unless it returns `CKR_BUFFER_TOO_SMALL` or is a successful call (i.e., one which returns `CKR_OK`) to determine the length of the buffer needed to hold the plaintext.

If the input ciphertext data cannot be decrypted because it has an inappropriate length, then either `CKR_ENCRYPTED_DATA_INVALID` or `CKR_ENCRYPTED_DATA_LEN_RANGE` may be returned.

**Request**

| Name      | Type              | Representation | Description    |
|-----------|-------------------|----------------|----------------|
| hSession  | CK_SESSION_HANDLE | uint 8/16/32   | Session handle |

**Response**

| Name     | Type                   | Representation | Description              |
|----------|------------------------|----------------|--------------------------|
| status   | [CK_RV](#return-value) | uint 8/16/32   | Return value             |
| lastPart | byte array             | bin 8/16/32    | Last decrypted data part |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
- `CKR_BUFFER_TOO_SMALL`
- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_ENCRYPTED_DATA_INVALID`
- `CKR_ENCRYPTED_DATA_LEN_RANGE`
- `CKR_FUNCTION_CANCELED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OK`
- `CKR_OPERATION_NOT_INITIALIZED`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`
- `CKR_USER_NOT_LOGGED_IN`

### Message Digesting Functions

#### Digest Init

Initializes a message-digesting operation.

After calling [DigestInit](#digest-init), the application can either call [Digest](#digest) to digest data in a single part; or call [DigestUpdate](#digest-update) zero or more times, followed by [DigestFinal](#digest-final), to digest data in multiple parts.

The message-digesting operation is active until the application uses a call to [Digest](#digest) or [DigestFinal](#digest-final) to actually obtain the message digest.

To process additional data (in single or multiple parts), the application must call [DigestInit](#digest-init) again.

**Request**

| Name      | Type                                  | Representation | Description        |
|-----------|---------------------------------------|----------------|--------------------|
| hSession  | CK_SESSION_HANDLE                     | uint 8/16/32   | Session handle     |
| mechanism | [CK_MECHANISM_TYPE](#mechanism-codes) | uint 8/16/32   | Digest mechanism   |
| parameter | byte array                            | bin 8/16/32    | Optional parameter |

**Response**

| Name   | Type                   | Representation | Description  |
|--------|------------------------|----------------|--------------|
| status | [CK_RV](#return-value) | uint 8/16/32   | Return value |

**Mechanism Codes**

- `CKM_MD2`
- `CKM_MD5`
- `CKM_SHA_1`
- `CKM_RIPEMD128`
- `CKM_RIPEMD160`
- `CKM_SHA256`
- `CKM_SHA224`
- `CKM_SHA384`
- `CKM_SHA512`

**Error Codes**

- `CKR_ARGUMENTS_BAD`
- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_CANCELED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_MECHANISM_INVALID`
- `CKR_MECHANISM_PARAM_INVALID`
- `CKR_OK`
- `CKR_OPERATION_ACTIVE`
- `CKR_PIN_EXPIRED`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`
- `CKR_USER_NOT_LOGGED_IN`


#### Digest

Digests data in a single part.

The digest operation must have been initialized with [DigestInit](#digest-init).

A call to [Digest](#digest) always terminates the active digest operation unless it returns `CKR_BUFFER_TOO_SMALL` or is a successful call (i.e., one which returns `CKR_OK`) to determine the length of the buffer needed to hold the message digest.

[Digest](#digest) can not be used to terminate a multi-part operation, and must be called after [DigestInit](#digest-init) without intervening [DigestUpdate](#digest-update) calls. [Digest](#digest) is equivalent to a sequence of [DigestUpdate](#digest-update) operations followed by [DigestFinal](#digest-final).

**Request**

| Name      | Type              | Representation  | Description          |
|-----------|-------------------|-----------------|----------------------|
| hSession  | CK_SESSION_HANDLE | uint 8/16/32    | Session handle       |
| data      | octet-stream      | bin 8/16/32     | Data to be processed |

**Response**

| Name   | Type                   | Representation | Description    |
|--------|------------------------|----------------|----------------|
| status | [CK_RV](#return-value) | uint 8/16/32   | Return value   |
| digest | octet-stream           | bin 8/16       | Digest of data |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
- `CKR_BUFFER_TOO_SMALL`
- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_CANCELED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OK`
- `CKR_OPERATION_NOT_INITIALIZED`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`

#### Digest Update

Continues a multiple-part message-digesting operation, processing another data part.

The message-digesting operation must have been initialized with [DigestInit](#digest-init).

Calls to this function and [DigestKey](#digest-key) may be interspersed any number of times in any order.

A call to [DigestUpdate](#digest-update) which results in an error terminates the current digest operation.

**Request**

| Name      | Type              | Representation | Description                  |
|-----------|-------------------|----------------|------------------------------|
| hSession  | CK_SESSION_HANDLE | uint 8/16/32   | Session handle               |
| part      | octet-stream      | bin 8/16/32    | Part of data to be processed |

**Response**

| Name   | Type                   | Representation | Description    |
|--------|------------------------|----------------|----------------|
| status | [CK_RV](#return-value) | uint 8/16/32   | Return value   |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_CANCELED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OK`
- `CKR_OPERATION_NOT_INITIALIZED`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`

#### Digest Key

Continues a multiple-part message-digesting operation by digesting the value of a secret key.

The message-digesting operation must have been initialized with [DigestInit](#digest-init).

Calls to this function and [DigestUpdate](#digest-update) may be interspersed any number of times in any order.

If the value of the supplied key cannot be digested purely for some reason related to its length, [DigestKey](#digest-key) should return the error code `CKR_KEY_SIZE_RANGE`.

**Request**

| Name      | Type              | Representation  | Description    |
|-----------|-------------------|-----------------|----------------|
| hSession  | CK_SESSION_HANDLE | uint 8/16/32    | Session handle |
| hKey      | CK_OBJECT_HANDLE  | uint 8/16/32    | Key handle     |

**Response**

| Name   | Type                   | Representation | Description    |
|--------|------------------------|----------------|----------------|
| status | [CK_RV](#return-value) | uint 8/16/32   | Return value   |

**Error Codes**

- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_CANCELED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_KEY_HANDLE_INVALID`
- `CKR_KEY_INDIGESTIBLE`
- `CKR_KEY_SIZE_RANGE`
- `CKR_OK`
- `CKR_OPERATION_NOT_INITIALIZED`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`

#### Digest Final

Finishes a multiple-part message-digesting operation, returning the message digest.

The digest operation must have been initialized with [DigestInit](#digest-init).

A call to [DigestFinal](#digest-final) always terminates the active digest operation unless it returns `CKR_BUFFER_TOO_SMALL` or is a successful call (i.e., one which returns `CKR_OK`) to determine the length of the buffer needed to hold the message digest.

**Request**

| Name      | Type              | Representation | Description    |
|-----------|-------------------|----------------|----------------|
| hSession  | CK_SESSION_HANDLE | uint 8/16/32   | Session handle |

**Response**

| Name   | Type                   | Representation | Description    |
|--------|------------------------|----------------|----------------|
| status | [CK_RV](#return-value) | uint 8/16/32   | Return value   |
| digest | octet-stream           | bin 8/16       | Digest of data |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
- `CKR_BUFFER_TOO_SMALL`
- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_CANCELED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OK`
- `CKR_OPERATION_NOT_INITIALIZED`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`

### Signing and MACing Functions

#### Sign Init

#### Sign

#### Sign Update

#### Sign Final

#### Sign Recover Init

#### Sign Recover

### Functions for Verifying signatures and MACs

#### Verify Init

#### Verify

#### Verify Update

#### Verify Final

#### Verify Recover Init

#### Verify Recover

### Dual-function Cryptographic Functions

#### Digest Encrypt Update

#### Decrypt Digest Update

#### Sign Encrypt Update

#### Decrypt Verify Update

### Key Management Functions

#### Generate Key

#### Generate Key Pair

#### Wrap Key

#### Unwrap Key

#### Derive Key

### Random Number Generation Functions

#### Seed Random

Mixes additional seed material into the token's random number generator.

**Request**

| Name      | Type              | Representation | Description          |
|-----------|-------------------|----------------|----------------------|
| hSession  | CK_SESSION_HANDLE | uint 8/16/32   | Session handle       |
| seed      | octet-stream      | bin 8/16       | Random seed material |

**Response**

| Name   | Type                   | Representation | Description  |
|--------|------------------------|----------------|--------------|
| status | [CK_RV](#return-value) | uint 8/16/32   | Return value |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_CANCELED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OK`
- `CKR_OPERATION_ACTIVE`
- `CKR_RANDOM_SEED_NOT_SUPPORTED`
- `CKR_RANDOM_NO_RNG`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`
- `CKR_USER_NOT_LOGGED_IN`

#### Generate Random

Generates random or pseudo-random data.

**Request**

| Name      | Type              | Representation | Description                           |
|-----------|-------------------|----------------|---------------------------------------|
| hSession  | CK_SESSION_HANDLE | uint 8/16/32   | Session handle                        |
| length    | CK_ULONG          | uint 8/16/32   | Length of random data to be generated |


**Response**

| Name   | Type                   | Representation | Description            |
|--------|------------------------|----------------|------------------------|
| status | [CK_RV](#return-value) | uint 8/16/32   | Return value           |
| random | octet-stream           | bin 8/16       | Generated random bytes |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_CANCELED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OK`
- `CKR_OPERATION_ACTIVE`
- `CKR_RANDOM_NO_RNG`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`
- `CKR_USER_NOT_LOGGED_IN`

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

Possible Values of `CK_MECHANISM_TYPE`

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

### Attributes

Possible values of `CK_ATTRIBUTE_TYPE`

| Code                                  | Value                             |
|---------------------------------------|-----------------------------------|
| `CKA_CLASS`                           | 0x00000000                        |
| `CKA_TOKEN`                           | 0x00000001                        |
| `CKA_PRIVATE`                         | 0x00000002                        |
| `CKA_LABEL`                           | 0x00000003                        |
| `CKA_APPLICATION`                     | 0x00000010                        |
| `CKA_VALUE`                           | 0x00000011                        |
| `CKA_OBJECT_ID`                       | 0x00000012                        |
| `CKA_CERTIFICATE_TYPE`                | 0x00000080                        |
| `CKA_ISSUER`                          | 0x00000081                        |
| `CKA_SERIAL_NUMBER`                   | 0x00000082                        |
| `CKA_AC_ISSUER`                       | 0x00000083                        |
| `CKA_OWNER`                           | 0x00000084                        |
| `CKA_ATTR_TYPES`                      | 0x00000085                        |
| `CKA_TRUSTED`                         | 0x00000086                        |
| `CKA_CERTIFICATE_CATEGORY`            | 0x00000087                        |
| `CKA_JAVA_MIDP_SECURITY_DOMAIN`       | 0x00000088                        |
| `CKA_URL`                             | 0x00000089                        |
| `CKA_HASH_OF_SUBJECT_PUBLIC_KEY`      | 0x0000008A                        |
| `CKA_HASH_OF_ISSUER_PUBLIC_KEY`       | 0x0000008B                        |
| `CKA_CHECK_VALUE`                     | 0x00000090                        |
| `CKA_KEY_TYPE`                        | 0x00000100                        |
| `CKA_SUBJECT`                         | 0x00000101                        |
| `CKA_ID`                              | 0x00000102                        |
| `CKA_SENSITIVE`                       | 0x00000103                        |
| `CKA_ENCRYPT`                         | 0x00000104                        |
| `CKA_DECRYPT`                         | 0x00000105                        |
| `CKA_WRAP`                            | 0x00000106                        |
| `CKA_UNWRAP`                          | 0x00000107                        |
| `CKA_SIGN`                            | 0x00000108                        |
| `CKA_SIGN_RECOVER`                    | 0x00000109                        |
| `CKA_VERIFY`                          | 0x0000010A                        |
| `CKA_VERIFY_RECOVER`                  | 0x0000010B                        |
| `CKA_DERIVE`                          | 0x0000010C                        |
| `CKA_START_DATE`                      | 0x00000110                        |
| `CKA_END_DATE`                        | 0x00000111                        |
| `CKA_MODULUS`                         | 0x00000120                        |
| `CKA_MODULUS_BITS`                    | 0x00000121                        |
| `CKA_PUBLIC_EXPONENT`                 | 0x00000122                        |
| `CKA_PRIVATE_EXPONENT`                | 0x00000123                        |
| `CKA_PRIME_1`                         | 0x00000124                        |
| `CKA_PRIME_2`                         | 0x00000125                        |
| `CKA_EXPONENT_1`                      | 0x00000126                        |
| `CKA_EXPONENT_2`                      | 0x00000127                        |
| `CKA_COEFFICIENT`                     | 0x00000128                        |
| `CKA_PRIME`                           | 0x00000130                        |
| `CKA_SUBPRIME`                        | 0x00000131                        |
| `CKA_BASE`                            | 0x00000132                        |
| `CKA_PRIME_BITS`                      | 0x00000133                        |
| `CKA_SUBPRIME_BITS`                   | 0x00000134                        |
| `CKA_SUB_PRIME_BITS`                  | 0x00000134                        |
| `CKA_VALUE_BITS`                      | 0x00000160                        |
| `CKA_VALUE_LEN`                       | 0x00000161                        |
| `CKA_EXTRACTABLE`                     | 0x00000162                        |
| `CKA_LOCAL`                           | 0x00000163                        |
| `CKA_NEVER_EXTRACTABLE`               | 0x00000164                        |
| `CKA_ALWAYS_SENSITIVE`                | 0x00000165                        |
| `CKA_KEY_GEN_MECHANISM`               | 0x00000166                        |
| `CKA_MODIFIABLE`                      | 0x00000170                        |
| `CKA_ECDSA_PARAMS`                    | 0x00000180                        |
| `CKA_EC_PARAMS`                       | 0x00000180                        |
| `CKA_EC_POINT`                        | 0x00000181                        |
| `CKA_SECONDARY_AUTH`                  | 0x00000200                        |
| `CKA_ALWAYS_AUTHENTICATE`             | 0x00000202                        |
| `CKA_AUTH_PIN_FLAGS`                  | 0x00000201                        |
| `CKA_WRAP_WITH_TRUSTED`               | 0x00000210                        |
| `CKA_WRAP_TEMPLATE`                   | (CKF_ARRAY_ATTRIBUTE&#124;0x00000211)  |
| `CKA_UNWRAP_TEMPLATE`                 | (CKF_ARRAY_ATTRIBUTE&#124;0x00000212)  |
| `CKA_DERIVE_TEMPLATE`                 | (CKF_ARRAY_ATTRIBUTE&#124;0x00000213)  |
| `CKA_OTP_FORMAT`                      | 0x00000220                        |
| `CKA_OTP_LENGTH`                      | 0x00000221                        |
| `CKA_OTP_TIME_INTERVAL`               | 0x00000222                        |
| `CKA_OTP_USER_FRIENDLY_MODE`          | 0x00000223                        |
| `CKA_OTP_CHALLENGE_REQUIREMENT`       | 0x00000224                        |
| `CKA_OTP_TIME_REQUIREMENT`            | 0x00000225                        |
| `CKA_OTP_COUNTER_REQUIREMENT`         | 0x00000226                        |
| `CKA_OTP_PIN_REQUIREMENT`             | 0x00000227                        |
| `CKA_OTP_COUNTER`                     | 0x0000022E                        |
| `CKA_OTP_TIME`                        | 0x0000022F                        |
| `CKA_OTP_USER_IDENTIFIER`             | 0x0000022A                        |
| `CKA_OTP_SERVICE_IDENTIFIER`          | 0x0000022B                        |
| `CKA_OTP_SERVICE_LOGO`                | 0x0000022C                        |
| `CKA_OTP_SERVICE_LOGO_TYPE`           | 0x0000022D                        |
| `CKA_GOSTR3410_PARAMS`                | 0x00000250                        |
| `CKA_GOSTR3411_PARAMS`                | 0x00000251                        |
| `CKA_GOST28147_PARAMS`                | 0x00000252                        |
| `CKA_HW_FEATURE_TYPE`                 | 0x00000300                        |
| `CKA_RESET_ON_INIT`                   | 0x00000301                        |
| `CKA_HAS_RESET`                       | 0x00000302                        |
| `CKA_PIXEL_X`                         | 0x00000400                        |
| `CKA_PIXEL_Y`                         | 0x00000401                        |
| `CKA_RESOLUTION`                      | 0x00000402                        |
| `CKA_CHAR_ROWS`                       | 0x00000403                        |
| `CKA_CHAR_COLUMNS`                    | 0x00000404                        |
| `CKA_COLOR                            | 0x00000405                        |
| `CKA_BITS_PER_PIXEL`                  | 0x00000406                        |
| `CKA_CHAR_SETS`                       | 0x00000480                        |
| `CKA_ENCODING_METHODS`                | 0x00000481                        |
| `CKA_MIME_TYPES`                      | 0x00000482                        |
| `CKA_MECHANISM_TYPE`                  | 0x00000500                        |
| `CKA_REQUIRED_CMS_ATTRIBUTES`         | 0x00000501                        |
| `CKA_DEFAULT_CMS_ATTRIBUTES`          | 0x00000502                        |
| `CKA_SUPPORTED_CMS_ATTRIBUTES`        | 0x00000503                        |
| `CKA_ALLOWED_MECHANISMS`              | (CKF_ARRAY_ATTRIBUTE&#124;0x00000600)  |
| `CKA_VENDOR_DEFINED`                  | 0x80000000                        |

### Flags

Possible values of `CK_FLAGS`

| Code                     | Value      |
|--------------------------|------------|
| `CKF_HW`                 | 0x00000001 |
| `CKF_ENCRYPT`            | 0x00000100 |
| `CKF_DECRYPT`            | 0x00000200 |
| `CKF_DIGEST`             | 0x00000400 |
| `CKF_SIGN`               | 0x00000800 |
| `CKF_SIGN_RECOVER`       | 0x00001000 |
| `CKF_VERIFY`             | 0x00002000 |
| `CKF_VERIFY_RECOVER`     | 0x00004000 |
| `CKF_GENERATE`           | 0x00008000 |
| `CKF_GENERATE_KEY_PAIR`  | 0x00010000 |
| `CKF_WRAP`               | 0x00020000 |
| `CKF_UNWRAP`             | 0x00040000 |
| `CKF_DERIVE`             | 0x00080000 |
| `CKF_EC_F_P`             | 0x00100000 |
| `CKF_EC_F_2M`            | 0x00200000 |
| `CKF_EC_ECPARAMETERS`    | 0x00400000 |
| `CKF_EC_NAMEDCURVE`      | 0x00800000 |
| `CKF_EC_UNCOMPRESS`      | 0x01000000 |
| `CKF_EC_COMPRESS`        | 0x02000000 |
| `CKF_EXTENSION`          | 0x80000000 |
