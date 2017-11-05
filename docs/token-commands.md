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
    - [Message Digesting Functions](#message-digesting-functions)
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
    - [Attribute Types](#attribute-types)
    - [Flags](#flags)
    - [States](#states)
    - [User Types](#user-types)
    - [Object Classes](#object-classes)

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

 | Name       | Type                                           | Representation | Description        |
 |------------|------------------------------------------------|----------------|--------------------|
 | status     | [CK_RV](#return-value)                         | uint 8/16/32   | Return value       |
 | mechanisms | array of [CK_MECHANISM_TYPE](#mechanism-codes) | array 16/32    | List of mechanisms |

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

Initializes a token.

If the token has not been initialized (i.e. new from the factory), then the `pin` parameter becomes the initial value of the `SO PIN`. If the token is being reinitialized, the `pin` parameter is checked against the existing `SO PIN` to authorize the initialization operation. In both cases, the `SO PIN` is the value `pin` after the function completes successfully. If the `SO PIN` is lost, then the card must be reinitialized using a mechanism outside the scope of this standard. The `CKF_TOKEN_INITIALIZED` flag in the `CK_TOKEN_INFO` structure indicates the action that will result from calling [InitToken](#init-token). If set, the token will be reinitialized, and the client must supply the existing SO password in `pin`.
When a token is initialized, all objects that can be destroyed are destroyed (i.e., all except for "indestructible" objects such as keys built into the token). Also, access by the normal user is disabled until the SO sets the normal user's PIN. Depending on the token, some "default" objects may be created, and attributes of some objects may be set to default values.

A token cannot be initialized if Cryptoki detects that any application has an open session with it; when a call to [InitToken](#init-token) is made under such circumstances, the call fails with error `CKR_SESSION_EXISTS`. Unfortunately, it may happen when [InitToken](#init-token) is called that some other application does have an open session with the token, but Cryptoki cannot detect this, because it cannot detect anything about other applications using the token. If this is the case, then the consequences of the [InitToken](#init-token) call are undefined.

**Request**

| Name  | Type         | Representation | Description            |
|-------|--------------|----------------|------------------------|
| pin   | octet-stream | bin 8/16/32    | User PIN               |
| label | octet-stream | bin 8/16/32    | Token tabel (32 chars) |

**Response**

| Name         | Type                   | Representation | Description  |
|--------------|------------------------|----------------|--------------|
| status       | [CK_RV](#return-value) | uint 8/16/32   | Return value |

**Error Codes**

- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_CANCELED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OK`
- `CKR_PIN_INCORRECT`
- `CKR_PIN_LOCKED`
- `CKR_SESSION_EXISTS`
- `CKR_SLOT_ID_INVALID`
- `CKR_TOKEN_NOT_PRESENT`
- `CKR_TOKEN_NOT_RECOGNIZED`
- `CKR_TOKEN_WRITE_PROTECTED`
- `CKR_ARGUMENTS_BAD`

#### Init PIN

Initializes the normal user's PIN.

**Request**

| Name     | Type              | Representation | Description     |
|----------|-------------------|----------------|-----------------|
| hSession | CK_SESSION_HANDLE | uint 8/16/32   | Session handle  |
| pin      | octet-stream      | bin 8/16/32    | Normal user PIN |

**Response**

| Name         | Type                   | Representation | Description  |
|--------------|------------------------|----------------|--------------|
| status       | [CK_RV](#return-value) | uint 8/16/32   | Return value |

**Error Codes**

- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_CANCELED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OK`
- `CKR_PIN_INVALID`
- `CKR_PIN_LEN_RANGE`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_READ_ONLY`
- `CKR_SESSION_HANDLE_INVALID`
- `CKR_TOKEN_WRITE_PROTECTED`
- `CKR_USER_NOT_LOGGED_IN`
- `CKR_ARGUMENTS_BAD`

#### Set PIN

Modifies the PIN of the user that is currently logged in, or the `CKU_USER PIN` if the session is not logged in.

[SetPIN](#set-pin) can only be called in the "R/W Public Session" state, "R/W SO Functions" state, or "R/W User Functions" state. An attempt to call it from a session in any other state fails with error `CKR_SESSION_READ_ONLY`.

**Request**

| Name     | Type              | Representation | Description     |
|----------|-------------------|----------------|-----------------|
| hSession | CK_SESSION_HANDLE | uint 8/16/32   | Session handle  |
| oldPin   | octet-stream      | bin 8/16/32    | User old PIN    |
| newPin   | octet-stream      | bin 8/16/32    | User new PIN    |

**Response**

| Name         | Type                   | Representation | Description  |
|--------------|------------------------|----------------|--------------|
| status       | [CK_RV](#return-value) | uint 8/16/32   | Return value |

**Error Codes**

- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_CANCELED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OK`
- `CKR_PIN_INCORRECT`
- `CKR_PIN_INVALID`
- `CKR_PIN_LEN_RANGE`
- `CKR_PIN_LOCKED`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`
- `CKR_SESSION_READ_ONLY`
- `CKR_TOKEN_WRITE_PROTECTED`
- `CKR_ARGUMENTS_BAD`

### Session Management Functions

#### Open Session

Opens a session between an application and a token in a particular slot.

When opening a session with [OpenSession](#open-session), the flags parameter consists of the logical OR of zero or more bit flags defined in the `CK_SESSION_INFO` data type. For legacy reasons, the `CKF_SERIAL_SESSION` bit must always be set; if a call to [OpenSession](#open-session) does not have this bit set, the call should return unsuccessfully with the error code `CKR_SESSION_PARALLEL_NOT_SUPPORTED`.

There may be a limit on the number of concurrent sessions an application may have with the token, which may depend on whether the session is "read-only" or "read/write". An attempt to open a session which does not succeed because there are too many existing sessions of some type should return `CKR_SESSION_COUNT`.

If the token is write-protected (as indicated in the `CK_TOKEN_INFO` structure), then only read-only sessions may be opened with it.

If the application calling [OpenSession](#open-session) already has a R/W SO session open with the token, then any attempt to open a R/O session with the token fails with error code `CKR_SESSION_READ_WRITE_SO_EXISTS`.

**Request**

| Name     | Type               | Representation | Description                |
|----------|--------------------|----------------|----------------------------|
| flags    | [CK_FLAGS](#flags) | uint 8/16/32   | Indicates type of session  |

**Response**

| Name         | Type                   | Representation | Description    |
|--------------|------------------------|----------------|----------------|
| status       | [CK_RV](#return-value) | uint 8/16/32   | Return value   |
| hSession     | CK_SESSION_HANDLE      | uint 8/16/32   | Session handle |

**Error Codes**

- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OK`
- `CKR_SESSION_COUNT`
- `CKR_SESSION_PARALLEL_NOT_SUPPORTED`
- `CKR_SESSION_READ_WRITE_SO_EXISTS`
- `CKR_SLOT_ID_INVALID`
- `CKR_TOKEN_NOT_PRESENT`
- `CKR_TOKEN_NOT_RECOGNIZED`
- `CKR_TOKEN_WRITE_PROTECTED`
- `CKR_ARGUMENTS_BAD`

#### Close Session

Closes a session between an application and a token.

When a session is closed, all session objects created by the session are destroyed automatically, even if the application has other sessions "using" the objects.

If this function is successful and it closes the last session between the application and the token, the login state of the token for the application returns to public sessions. Any new sessions to the token opened by the application will be either R/O Public or R/W Public sessions.

Despite the fact this [CloseSession](#close-session) is supposed to close a session, the return value `CKR_SESSION_CLOSED` is an error return. It actually indicates the (probably somewhat unlikely) event that while this function call was executing, another call was made to [CloseSession](#close-session) to close this particular session, and that call finished executing first. Such uses of sessions are a bad idea, and Cryptoki makes little promise of what will occur in general if an application indulges in this sort of behavior.

**Request**

| Name     | Type               | Representation | Description    |
|----------|--------------------|----------------|----------------|
| hSession | CK_SESSION_HANDLE  | uint 8/16/32   | Session handle |

**Response**

| Name         | Type                   | Representation | Description    |
|--------------|------------------------|----------------|----------------|
| status       | [CK_RV](#return-value) | uint 8/16/32   | Return value   |

**Errror Codes**

- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OK`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`

#### Close All Sessions

closes all sessions an application has with a token.

When a session is closed, all session objects created by the session are destroyed automatically.

After successful execution of this function, the login state of the token for the application returns to public sessions. Any new sessions to the token opened by the application will be either R/O Public or R/W Public sessions.

**Request**

None

**Response**

| Name         | Type                   | Representation | Description    |
|--------------|------------------------|----------------|----------------|
| status       | [CK_RV](#return-value) | uint 8/16/32   | Return value   |

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

#### Get Session Info

Obtains information about a session.

**Request**

| Name     | Type               | Representation | Description    |
|----------|--------------------|----------------|----------------|
| hSession | CK_SESSION_HANDLE  | uint 8/16/32   | Session handle |

**Response**

| Name   | Type                   | Representation | Description    |
|--------|------------------------|----------------|----------------|
| status | [CK_RV](#return-value) | uint 8/16/32   | Return value   |
| state  | [CK_STATE](#states)    | uint 8/16/32   | Session state  |
| flags  | [CK_FLAGS](#flags)     | uint 8/16/32   | Bit flags that define the type of session |

**Error Codes**

- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OK`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`
- `CKR_ARGUMENTS_BAD`

#### Get Operation State

Obtains a copy of the cryptographic operations state of a session, encoded as a string of bytes. State serialization is mechanism specific. Refer to [Token State Serialization](token-state-serialization.md) for more detail.

**Request**

| Name     | Type               | Representation | Description    |
|----------|--------------------|----------------|----------------|
| hSession | CK_SESSION_HANDLE  | uint 8/16/32   | Session handle |

**Response**

| Name           | Type                   | Representation | Description                |
|----------------|------------------------|----------------|----------------------------|
| status         | [CK_RV](#return-value) | uint 8/16/32   | Return value               |
| operationState | octet-stream           | bin 8/16/32    | Serialized operation state |

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
- `CKR_OPERATION_NOT_INITIALIZED`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`
- `CKR_STATE_UNSAVEABLE`
- `CKR_ARGUMENTS_BAD`

#### Set Operation State

Restores the cryptographic operations state of a session from a string of bytes obtained with [GetOperationState](#get-operation-state). Refer to [Token State Serialization](token-state-serialization.md) for more detail.

If [SetOperationState](#set-operation-state) is supplied with alleged saved cryptographic operations state which it can determine is not valid saved state (or is cryptographic operations state from a session with a different session state, or is cryptographic operations state from a different token), it fails with the error `CKR_SAVED_STATE_INVALID`.

Saved state obtained from calls to [GetOperationState](#get-operation-state) may or may not contain information about keys in use for ongoing cryptographic operations. If a saved cryptographic operations state has an ongoing encryption or decryption operation, and the key in use for the operation is not saved in the state, then it must be supplied to C_SetOperationState in the hEncryptionKey argument. If it is not, then [SetOperationState](#set-operation-state) will fail and return the error `CKR_KEY_NEEDED`. If the key in use for the operation is saved in the state, then it can be supplied in the hEncryptionKey argument, but this is not required.

Similarly, if a saved cryptographic operations state has an ongoing signature, MACing, or verification operation, and the key in use for the operation is not saved in the state, then it must be supplied to [SetOperationState](#set-operation-state) in the `hAuthenticationKey` argument. If it is not, then [SetOperationState](#set-operation-state) will fail with the error `CKR_KEY_NEEDED`. If the key in use for the operation is saved in the state, then it can be supplied in the `hAuthenticationKey` argument, but this is not required.

If an irrelevant key is supplied to [SetOperationState](#set-operation-state) call (e.g., a nonzero key handle is submitted in the hEncryptionKey argument, but the saved cryptographic operations state supplied does not have an ongoing encryption or decryption operation, then [SetOperationState](#set-operation-state) fails with the error `CKR_KEY_NOT_NEEDED`.

If a key is supplied as an argument to [SetOperationState](#set-operation-state), and [SetOperationState](#set-operation-state) can somehow detect that this key was not the key being used in the source session for the supplied cryptographic operations state (it may be able to detect this if the key or a hash of the key is present in the saved state, for example), then [SetOperationState](#set-operation-state) fails with the error `CKR_KEY_CHANGED`.

An application can look at the `CKF_RESTORE_KEY_NOT_NEEDED` flag in the flags field of the `CK_TOKEN_INFO` field for a token to determine whether or not it needs to supply key handles to [SetOperationState](#set-operation-state) calls. If this flag is true, then a call to [SetOperationState](#set-operation-state) never needs a key handle to be supplied to it. If this flag is false, then at least some of the time, [SetOperationState](#set-operation-state) requires a key handle, and so the application should probably always pass in any relevant key handles when restoring cryptographic operations state to a session.

[SetOperationState](#set-operation-state) can successfully restore cryptographic operations state to a session even if that session has active cryptographic or object search operations when [SetOperationState](#set-operation-state) is called (the ongoing operations are abruptly cancelled).

**Request**

| Name               | Type              | Representation | Description                                                        |
|--------------------|-------------------|----------------|--------------------------------------------------------------------|
| hSession           | CK_SESSION_HANDLE | uint 8/16/32   | Session handle                                                     |
| operationState     | octet-stream      | bin 8/16/32    | Serialized operation state                                         |
| hEncryptionKey     | CK_OBJECT_HANDLE  | uint 8/16/32   | Encryption key handle (for encryption/decryption related state)    |
| hAuthenticationKey | CK_OBJECT_HANDLE  | uint 8/16/32   | Authentication key handle (for signing/verification related state) |

**Response**

| Name           | Type                   | Representation | Description                |
|----------------|------------------------|----------------|----------------------------|
| status         | [CK_RV](#return-value) | uint 8/16/32   | Return value               |

**Error Codes**

- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_KEY_CHANGED`
- `CKR_KEY_NEEDED`
- `CKR_KEY_NOT_NEEDED`
- `CKR_OK, CKR_SAVED_STATE_INVALID`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`
- `CKR_ARGUMENTS_BAD`

#### Login

Logs a user into a token.

When the user type is either `CKU_SO` or `CKU_USER`, if the call succeeds, each of the application's sessions will enter either the "R/W SO Functions" state, the "R/W User Functions" state, or the "R/O User Functions" state. If the user type is `CKU_CONTEXT_SPECIFIC` , the behavior of [Login](#login) depends on the context in which it is called. Improper use of this user type will result in a return value `CKR_OPERATION_NOT_INITIALIZED`.

If there are any active cryptographic or object finding operations in an application's session, and then [Login](#login) is successfully executed by that application, it may or may not be the case that those operations are still active. Therefore, before logging in, any active operations should be finished.

If the application calling [Login](#login) has a R/O session open with the token, then it will be unable to log the SO into a session. An attempt to do this will result in the error code `CKR_SESSION_READ_ONLY_EXISTS`.

[Login](#login) may be called repeatedly, without intervening [Logout](#logout) calls, if (and only if) a key with the `CKA_ALWAYS_AUTHENTICATE` attribute set to `CK_TRUE` exists, and the user needs to do cryptographic operation on this key.

**Request**

| Name     | Type                        | Representation | Description    |
|----------|-----------------------------|----------------|----------------|
| hSession | CK_SESSION_HANDLE           | uint 8/16/32   | Session handle |
| userType | [CK_USER_TYPE](#user-types) | uint 8/16/32   | User type      |
| pin      | octet-stream                | bin 8/16/32    | User PIN       |

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
- `CKR_OPERATION_NOT_INITIALIZED`
- `CKR_PIN_INCORRECT`
- `CKR_PIN_LOCKED`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`
- `CKR_SESSION_READ_ONLY_EXISTS`
- `CKR_USER_ALREADY_LOGGED_IN`
- `CKR_USER_ANOTHER_ALREADY_LOGGED_IN`
- `CKR_USER_PIN_NOT_INITIALIZED`
- `CKR_USER_TOO_MANY_TYPES`
- `CKR_USER_TYPE_INVALID`

#### Logout

Logs a user out from a token.

Depending on the current user type, if the call succeeds, each of the application's sessions will enter either the "R/W Public Session" state or the "R/O Public Session" state.
When [Logout](#logout) successfully executes, any of the application's handles to private objects become invalid (even if a user is later logged back into the token, those handles remain invalid). In addition, all private session objects from sessions belonging to the application are destroyed.

If there are any active cryptographic or object-finding operations in an application's session, and then [Logout](#logout) is successfully executed by that application, it may or may not be the case that those operations are still active. Therefore, before logging out, any active operations should be finished.

**Request**

| Name     | Type              | Representation | Description    |
|----------|-------------------|----------------|----------------|
| hSession | CK_SESSION_HANDLE | uint 8/16/32   | Session handle |

**Response**

| Name   | Type                   | Representation | Description  |
|--------|------------------------|----------------|--------------|
| status | [CK_RV](#return-value) | uint 8/16/32   | Return value |

**Error Codes**

- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OK`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`
- `CKR_USER_NOT_LOGGED_IN`

### Object Management Functions

#### Create Object

Creates a new object.

If a call to [CreateObject](#create-object) cannot support the precise template supplied to it, it will fail and return without creating any object.

If [CreateObject](#create-object) is used to create a key object, the key object will have its `CKA_LOCAL` attribute set to `CK_FALSE`. If that key object is a secret or private key then the new key will have the `CKA_ALWAYS_SENSITIVE` attribute set to `CK_FALSE`, and the `CKA_NEVER_EXTRACTABLE` attribute set to `CK_FALSE`.

Only session objects can be created during a read-only session. Only public objects can be created unless the normal user is logged in.

**Request**

| Name      | Type                                                                             | Representation | Description                       |
|-----------|----------------------------------------------------------------------------------|----------------|-----------------------------------|
| hSession  | CK_SESSION_HANDLE                                                                | uint 8/16/32   | Session handle                    |
| template  | map of [CK_ATTRIBUTE_TYPE](#attribute-types) and octet-stream of attribute value | map 16/32      | Map of attribute and their values |

**Response**

| Name    | Type                   | Representation | Description           |
|---------|------------------------|----------------|-----------------------|
| status  | [CK_RV](#return-value) | uint 8/16/32   | Return value          |
| hObject | CK_OBJECT_HANDLE       | uint 8/16/32   | Created object handle |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
- `CKR_ATTRIBUTE_READ_ONLY`
- `CKR_ATTRIBUTE_TYPE_INVALID`
- `CKR_ATTRIBUTE_VALUE_INVALID`
- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_DOMAIN_PARAMS_INVALID`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OK`
- `CKR_PIN_EXPIRED`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`
- `CKR_SESSION_READ_ONLY`
- `CKR_TEMPLATE_INCOMPLETE`
- `CKR_TEMPLATE_INCONSISTENT`
- `CKR_TOKEN_WRITE_PROTECTED`
- `CKR_USER_NOT_LOGGED_IN`

#### Copy Object

Copies an object, creating a new object for the copy.

The template may specify new values for any attributes of the object that can ordinarily be modified (e.g., in the course of copying a secret key, a key's `CKA_EXTRACTABLE` attribute may be changed from `CK_TRUE` to `CK_FALSE`, but not the other way around. If this change is made, the new key's `CKA_NEVER_EXTRACTABLE` attribute will have the value `CK_FALSE`. Similarly, the template may specify that the new key's `CKA_SENSITIVE` attribute be CK_TRUE; the new key will have the same value for its `CKA_ALWAYS_SENSITIVE` attribute as the original key). It may also specify new values of the `CKA_TOKEN` and `CKA_PRIVATE` attributes (e.g., to copy a session object to a token object). If the template specifies a value of an attribute which is incompatible with other existing attributes of the object, the call fails with the return code `CKR_TEMPLATE_INCONSISTENT`.

If a call to C_CopyObject cannot support the precise template supplied to it, it will fail and return without creating any object. If the object indicated by hObject has its CKA_COPYABLE attribute set to `CK_FALSE`, C_CopyObject will return `CKR_COPY_PROHIBITED`.

Only session objects can be created during a read-only session. Only public objects can be created unless the normal user is logged in.

**Request**

| Name      | Type                                                                             | Representation | Description                       |
|-----------|----------------------------------------------------------------------------------|----------------|-----------------------------------|
| hSession  | CK_SESSION_HANDLE                                                                | uint 8/16/32   | Session handle                    |
| hObject   | CK_OBJECT_HANDLE                                                                 | uint 8/16/32   | Handle of object to be copied     |
| template  | map of [CK_ATTRIBUTE_TYPE](#attribute-types) and octet-stream of attribute value | map 16/32      | Map of attribute and their values |

**Response**

| Name       | Type                   | Representation | Description             |
|------------|------------------------|----------------|-------------------------|
| status     | [CK_RV](#return-value) | uint 8/16/32   | Return value            |
| hNewObject | CK_OBJECT_HANDLE       | uint 8/16/32   | Handle of cloned object |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
- `CKR_ATTRIBUTE_READ_ONLY`
- `CKR_ATTRIBUTE_TYPE_INVALID`
- `CKR_ATTRIBUTE_VALUE_INVALID`
- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OBJECT_HANDLE_INVALID`
- `CKR_OK`
- `CKR_PIN_EXPIRED`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`
- `CKR_SESSION_READ_ONLY`
- `CKR_TEMPLATE_INCONSISTENT`
- `CKR_TOKEN_WRITE_PROTECTED`
- `CKR_USER_NOT_LOGGED_IN`
- `CKR_COPY_PROHIBITED`

#### Destroy Object

Destroys an object.

Only session objects can be destroyed during a read-only session. Only public objects can be destroyed unless the normal user is logged in.

**Request**

| Name      | Type              | Representation | Description                      |
|-----------|-------------------|----------------|----------------------------------|
| hSession  | CK_SESSION_HANDLE | uint 8/16/32   | Session handle                   |
| hObject   | CK_OBJECT_HANDLE  | uint 8/16/32   | Handle of object to be destroyed |

**Response**

| Name   | Type                   | Representation | Description  |
|--------|------------------------|----------------|--------------|
| status | [CK_RV](#return-value) | uint 8/16/32   | Return value |

**Error Codes**

- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
- `CKR_OBJECT_HANDLE_INVALID`
- `CKR_OK, CKR_PIN_EXPIRED`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`
- `CKR_SESSION_READ_ONLY`
- `CKR_TOKEN_WRITE_PROTECTED`

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
| parameter | octet-stream                          | bin 8/16/32    | Optional parameter |
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
| data      | octet-stream      | bin 8/16/32    | Data to be encrypted |

**Response**

| Name          | Type                   | Representation | Description    |
|---------------|------------------------|----------------|----------------|
| status        | [CK_RV](#return-value) | uint 8/16/32   | Return value   |
| encryptedData | octet-stream           | bin 8/16/32    | Encrypted data |

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
| part      | octet-stream      | bin 8/16/32    | Data part to be encrypted |

**Response**

| Name          | Type                   | Representation | Description            |
|---------------|------------------------|----------------|------------------------|
| status        | [CK_RV](#return-value) | uint 8/16/32   | Return value           |
| encryptedPart | octet-stream           | bin 8/16/32    | Encrypted part of data |

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
| lastEncryptedPart | octet-stream           | bin 8/16/32    | Last encrypted part of data |

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
| parameter | octet-stream                          | bin 8/16/32    | Optional parameter |
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
| encryptedData | octet-stream      | bin 8/16/32    | Data to be decrypted |

**Response**

| Name          | Type                   | Representation | Description          |
|---------------|------------------------|----------------|----------------------|
| status        | [CK_RV](#return-value) | uint 8/16/32   | Return value         |
| data          | octet-stream           | bin 8/16/32    | Decrypted data       |

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
| encryptedPart | octet-stream      | bin 8/16/32    | Encrypted data part |

**Response**

| Name   | Type                   | Representation | Description         |
|--------|------------------------|----------------|---------------------|
| status | [CK_RV](#return-value) | uint 8/16/32   | Return value        |
| part   | octet-stream           | bin 8/16/32    | Decrypted data part |

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
| lastPart | octet-stream           | bin 8/16/32    | Last decrypted data part |

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
| parameter | octet-stream                          | bin 8/16/32    | Optional parameter |

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

Initializes a signature operation, where the signature is an appendix to the data.

The `CKA_SIGN` attribute of the signature key, which indicates whether the key supports signatures with appendix, must be `CK_TRUE`.
After calling [SignInit](#sign-init), the application can either call [Sign](#sign) to sign in a single part; or call [SignUpdate](#sign-update) one or more times, followed by [SignFinal](#sign-final), to sign data in multiple parts. The signature operation is active until the application uses a call to [Sign](#sign) or [SignFinal](#sign-final) to actually obtain the signature. To process additional data (in single or multiple parts), the application must call [SignInit](#sign-init) again.

**Request**

| Name      | Type                                  | Representation | Description        |
|-----------|---------------------------------------|----------------|--------------------|
| hSession  | CK_SESSION_HANDLE                     | uint 8/16/32   | Session handle     |
| mechanism | [CK_MECHANISM_TYPE](#mechanism-codes) | uint 8/16/32   | Signing mechanism   |
| parameter | octet-stream                          | bin 8/16/32    | Optional parameter |
| hKey      | CK_OBJECT_HANDLE                      | uint 8/16/32   | Key Handle         |

**Response**

| Name   | Type                   | Representation | Description  |
|--------|------------------------|----------------|--------------|
| status | [CK_RV](#return-value) | uint 8/16/32   | Return value |

**Mechanism Codes**

- `CKM_RSA_9796`
- `CKM_MD2_RSA_PKCS`
- `CKM_MD5_RSA_PKCS`
- `CKM_SHA1_RSA_PKCS`
- `CKM_RIPEMD128_RSA_PKCS`
- `CKM_RIPEMD160_RSA_PKCS`
- `CKM_RSA_X9_31`
- `CKM_SHA1_RSA_X9_31`
- `CKM_RSA_PKCS_PSS`
- `CKM_SHA1_RSA_PKCS_PSS`
- `CKM_DSA`
- `CKM_DSA_SHA1`
- `CKM_DSA_SHA224`
- `CKM_DSA_SHA256`
- `CKM_DSA_SHA384`
- `CKM_DSA_SHA512`
- `CKM_SHA256_RSA_PKCS`
- `CKM_SHA384_RSA_PKCS`
- `CKM_SHA512_RSA_PKCS`
- `CKM_SHA256_RSA_PKCS_PSS`
- `CKM_SHA384_RSA_PKCS_PSS`
- `CKM_SHA512_RSA_PKCS_PSS`
- `CKM_SHA224_RSA_PKCS`
- `CKM_SHA224_RSA_PKCS_PSS`
- `CKM_RC2_MAC`
- `CKM_RC2_MAC_GENERAL`
- `CKM_DES_MAC`
- `CKM_DES_MAC_GENERAL`
- `CKM_DES3_MAC`
- `CKM_DES3_MAC_GENERAL`
- `CKM_DES3_CMAC_GENERAL`
- `CKM_DES3_CMAC`
- `CKM_CDMF_MAC`
- `CKM_CDMF_MAC_GENERAL`
- `CKM_MD2_HMAC`
- `CKM_MD2_HMAC_GENERAL`
- `CKM_MD5_HMAC`
- `CKM_MD5_HMAC_GENERAL`
- `CKM_SHA_1_HMAC`
- `CKM_SHA_1_HMAC_GENERAL`
- `CKM_RIPEMD128_HMAC`
- `CKM_RIPEMD128_HMAC_GENERAL`
- `CKM_RIPEMD160_HMAC`
- `CKM_RIPEMD160_HMAC_GENERAL`
- `CKM_SHA256_HMAC`
- `CKM_SHA256_HMAC_GENERAL`
- `CKM_SHA224_HMAC`
- `CKM_SHA224_HMAC_GENERAL`
- `CKM_SHA384_HMAC`
- `CKM_SHA384_HMAC_GENERAL`
- `CKM_SHA512_HMAC`
- `CKM_SHA512_HMAC_GENERAL`
- `CKM_CAST_MAC`
- `CKM_CAST_MAC_GENERAL`
- `CKM_CAST3_MAC`
- `CKM_CAST3_MAC_GENERAL`
- `CKM_CAST5_MAC`
- `CKM_CAST128_MAC`
- `CKM_CAST5_MAC_GENERAL`
- `CKM_CAST128_MAC_GENERAL`
- `CKM_RC5_MAC`
- `CKM_RC5_MAC_GENERAL`
- `CKM_IDEA_MAC`
- `CKM_IDEA_MAC_GENERAL`
- `CKM_CAMELLIA_MAC`
- `CKM_CAMELLIA_MAC_GENERAL`
- `CKM_ARIA_MAC`
- `CKM_ARIA_MAC_GENERAL`
- `CKM_SEED_MAC`
- `CKM_SEED_MAC_GENERAL`
- `CKM_ECDSA`
- `CKM_ECDSA_SHA1`
- `CKM_ECDSA_SHA224`
- `CKM_ECDSA_SHA256`
- `CKM_ECDSA_SHA384`
- `CKM_ECDSA_SHA512`
- `CKM_AES_MAC`
- `CKM_AES_MAC_GENERAL`
- `CKM_AES_CMAC`
- `CKM_AES_CMAC_GENERAL`
- `CKM_GOSTR3411_HMAC`
- `CKM_GOST28147_MAC`

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

#### Sign

Signs data in a single part, where the signature is an appendix to the data.

The signing operation must have been initialized with [SignInit](#sign-init). A call to [Sign](#sign) always terminates the active signing operation unless it returns `CKR_BUFFER_TOO_SMALL` or is a successful call (i.e., one which returns `CKR_OK`) to determine the length of the buffer needed to hold the signature.

[Sign](#sign) can not be used to terminate a multi-part operation, and must be called after [SignInit](#sign-init) without intervening [SignUpdate](#sign-update) calls.

For most mechanisms, [Sign](#sign) is equivalent to a sequence of [SignUpdate](#sign-update) operations followed by [SignFinal](#sign-final).

**Request**

| Name      | Type              | Representation | Description       |
|-----------|-------------------|----------------|-------------------|
| hSession  | CK_SESSION_HANDLE | uint 8/16/32   | Session handle    |
| data      | octet-stream      | bin 8/16/32    | Data to be signed |

**Response**

| Name      | Type                   | Representation | Description    |
|-----------|------------------------|----------------|----------------|
| status    | [CK_RV](#return-value) | uint 8/16/32   | Return value   |
| signature | octet-stream           | bin 8/16/32    | Data signature |

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
- `CKR_USER_NOT_LOGGED_IN`
- `CKR_FUNCTION_REJECTED`

#### Sign Update

continues a multiple-part signature operation, processing another data part.

The signature operation must have been initialized with [SignInit](#sign-init). This function may be called any number of times in succession. A call to [SignUpdate](#sign-update) which results in an error terminates the current signature operation.

**Request**

| Name      | Type              | Representation | Description            |
|-----------|-------------------|----------------|------------------------|
| hSession  | CK_SESSION_HANDLE | uint 8/16/32   | Session handle         |
| part      | octet-stream      | bin 8/16/32    | Data part to be signed |

**Response**

| Name      | Type                   | Representation | Description    |
|-----------|------------------------|----------------|----------------|
| status    | [CK_RV](#return-value) | uint 8/16/32   | Return value   |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
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
- `CKR_USER_NOT_LOGGED_IN`

#### Sign Final

Finishes a multiple-part signature operation, returning the signature.

The signing operation must have been initialized with [SignInit](#sign-init). A call to [SignFinal](#sign-final) always terminates the active signing operation unless it returns `CKR_BUFFER_TOO_SMALL` or is a successful call (i.e., one which returns `CKR_OK`) to determine the length of the buffer needed to hold the signature.

**Request**

| Name      | Type              | Representation | Description       |
|-----------|-------------------|----------------|-------------------|
| hSession  | CK_SESSION_HANDLE | uint 8/16/32   | Session handle    |

**Response**

| Name      | Type                   | Representation | Description    |
|-----------|------------------------|----------------|----------------|
| status    | [CK_RV](#return-value) | uint 8/16/32   | Return value   |
| signature | octet-stream           | bin 8/16/32    | Data signature |

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
- `CKR_USER_NOT_LOGGED_IN`
- `CKR_FUNCTION_REJECTED`

#### Sign Recover Init

Initializes a signature operation, where the data can be recovered from the signature.

The `CKA_SIGN_RECOVER` attribute of the signature key, which indicates whether the key supports signatures where the data can be recovered from the signature, must be `CK_TRUE`.
After calling [SignRecoverInit](#sign-recover-init), the application may call [SignRecover](#sign-recover) to sign in a single part. The signature operation is active until the application uses a call to [SignRecover](#sign-recover) to actually obtain the signature. To process additional data in a single part, the application must call [SignRecoverInit](#sign-recover-init) again.

**Request**

| Name      | Type                                  | Representation | Description        |
|-----------|---------------------------------------|----------------|--------------------|
| hSession  | CK_SESSION_HANDLE                     | uint 8/16/32   | Session handle     |
| mechanism | [CK_MECHANISM_TYPE](#mechanism-codes) | uint 8/16/32   | Signing mechanism   |
| parameter | octet-stream                          | bin 8/16/32    | Optional parameter |
| hKey      | CK_OBJECT_HANDLE                      | uint 8/16/32   | Key Handle         |

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

#### Sign Recover

Signs data in a single operation, where the data can be recovered from the signature.

The signing operation must have been initialized with [SignRecoverInit](#sign-recover-init). A call to [SignRecover](#sign-recover) always terminates the active signing operation unless it returns `CKR_BUFFER_TOO_SMALL` or is a successful call (i.e., one which returns `CKR_OK`) to determine the length of the buffer needed to hold the signature.

**Request**

| Name      | Type              | Representation | Description       |
|-----------|-------------------|----------------|-------------------|
| hSession  | CK_SESSION_HANDLE | uint 8/16/32   | Session handle    |
| data      | octet-stream      | bin 8/16/32    | Data to be signed |

**Response**

| Name      | Type                   | Representation | Description    |
|-----------|------------------------|----------------|----------------|
| status    | [CK_RV](#return-value) | uint 8/16/32   | Return value   |
| signature | octet-stream           | bin 8/16/32    | Data signature |

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
- `CKR_USER_NOT_LOGGED_IN`

### Functions for Verifying signatures and MACs

#### Verify Init

Initializes a verification operation, where the signature is an appendix to the data.

The `CKA_VERIFY` attribute of the verification key, which indicates whether the key supports verification where the signature is an appendix to the data, must be `CK_TRUE`.
After calling [VerifyInit](#verify-init), the application can either call [Verify](#verify) to verify a signature on data in a single part; or call [VerifyUpdate](#verify-update) one or more times, followed by [VerifyFinal](#verify-final), to verify a signature on data in multiple parts. The verification operation is active until the application calls [Verify](#verify) or [VerifyFinal](#verify-final). To process additional data (in single or multiple parts), the application must call [VerifyInit](#verify-init) again.

**Request**

| Name      | Type                                  | Representation | Description                      |
|-----------|---------------------------------------|----------------|----------------------------------|
| hSession  | CK_SESSION_HANDLE                     | uint 8/16/32   | Session handle                   |
| mechanism | [CK_MECHANISM_TYPE](#mechanism-codes) | uint 8/16/32   | Signature Verification mechanism |
| parameter | octet-stream                          | bin 8/16/32    | Optional parameter               |
| hKey      | CK_OBJECT_HANDLE                      | uint 8/16/32   | Key Handle                       |

**Response**

| Name   | Type                   | Representation | Description  |
|--------|------------------------|----------------|--------------|
| status | [CK_RV](#return-value) | uint 8/16/32   | Return value |

**Mechanism Codes**

- `CKM_RSA_9796`
- `CKM_MD2_RSA_PKCS`
- `CKM_MD5_RSA_PKCS`
- `CKM_SHA1_RSA_PKCS`
- `CKM_RIPEMD128_RSA_PKCS`
- `CKM_RIPEMD160_RSA_PKCS`
- `CKM_RSA_X9_31`
- `CKM_SHA1_RSA_X9_31`
- `CKM_RSA_PKCS_PSS`
- `CKM_SHA1_RSA_PKCS_PSS`
- `CKM_DSA`
- `CKM_DSA_SHA1`
- `CKM_DSA_SHA224`
- `CKM_DSA_SHA256`
- `CKM_DSA_SHA384`
- `CKM_DSA_SHA512`
- `CKM_SHA256_RSA_PKCS`
- `CKM_SHA384_RSA_PKCS`
- `CKM_SHA512_RSA_PKCS`
- `CKM_SHA256_RSA_PKCS_PSS`
- `CKM_SHA384_RSA_PKCS_PSS`
- `CKM_SHA512_RSA_PKCS_PSS`
- `CKM_SHA224_RSA_PKCS`
- `CKM_SHA224_RSA_PKCS_PSS`
- `CKM_RC2_MAC`
- `CKM_RC2_MAC_GENERAL`
- `CKM_DES_MAC`
- `CKM_DES_MAC_GENERAL`
- `CKM_DES3_MAC`
- `CKM_DES3_MAC_GENERAL`
- `CKM_DES3_CMAC_GENERAL`
- `CKM_DES3_CMAC`
- `CKM_CDMF_MAC`
- `CKM_CDMF_MAC_GENERAL`
- `CKM_MD2_HMAC`
- `CKM_MD2_HMAC_GENERAL`
- `CKM_MD5_HMAC`
- `CKM_MD5_HMAC_GENERAL`
- `CKM_SHA_1_HMAC`
- `CKM_SHA_1_HMAC_GENERAL`
- `CKM_RIPEMD128_HMAC`
- `CKM_RIPEMD128_HMAC_GENERAL`
- `CKM_RIPEMD160_HMAC`
- `CKM_RIPEMD160_HMAC_GENERAL`
- `CKM_SHA256_HMAC`
- `CKM_SHA256_HMAC_GENERAL`
- `CKM_SHA224_HMAC`
- `CKM_SHA224_HMAC_GENERAL`
- `CKM_SHA384_HMAC`
- `CKM_SHA384_HMAC_GENERAL`
- `CKM_SHA512_HMAC`
- `CKM_SHA512_HMAC_GENERAL`
- `CKM_CAST_MAC`
- `CKM_CAST_MAC_GENERAL`
- `CKM_CAST3_MAC`
- `CKM_CAST3_MAC_GENERAL`
- `CKM_CAST5_MAC`
- `CKM_CAST128_MAC`
- `CKM_CAST5_MAC_GENERAL`
- `CKM_CAST128_MAC_GENERAL`
- `CKM_RC5_MAC`
- `CKM_RC5_MAC_GENERAL`
- `CKM_IDEA_MAC`
- `CKM_IDEA_MAC_GENERAL`
- `CKM_CAMELLIA_MAC`
- `CKM_CAMELLIA_MAC_GENERAL`
- `CKM_ARIA_MAC`
- `CKM_ARIA_MAC_GENERAL`
- `CKM_SEED_MAC`
- `CKM_SEED_MAC_GENERAL`
- `CKM_ECDSA`
- `CKM_ECDSA_SHA1`
- `CKM_ECDSA_SHA224`
- `CKM_ECDSA_SHA256`
- `CKM_ECDSA_SHA384`
- `CKM_ECDSA_SHA512`
- `CKM_AES_MAC`
- `CKM_AES_MAC_GENERAL`
- `CKM_AES_CMAC`
- `CKM_AES_CMAC_GENERAL`
- `CKM_GOSTR3411_HMAC`
- `CKM_GOST28147_MAC`

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

#### Verify

Verifies a signature in a single-part operation, where the signature is an appendix to the data.

The verification operation must have been initialized with [VerifyInit](#verify-init). A call to [Verify](#verify) always terminates the active verification operation.

A successful call to [Verify](#verify) should return either the value `CKR_OK` (indicating that the supplied signature is valid) or `CKR_SIGNATURE_INVALID` (indicating that the supplied signature is invalid). If the signature can be seen to be invalid purely on the basis of its length, then `CKR_SIGNATURE_LEN_RANGE` should be returned. In any of these cases, the active signing operation is terminated.

[Verify](#verify) can not be used to terminate a multi-part operation, and must be called after [VerifyInit](#verify-init) without intervening [VerifyUpdate](#verify-update) calls.

For most mechanisms, [Verify](#verify) is equivalent to a sequence of [VerifyUpdate](#verify-update) operations followed by [VerifyFinal](#verify_final).

**Request**

| Name      | Type              | Representation | Description       |
|-----------|-------------------|----------------|-------------------|
| hSession  | CK_SESSION_HANDLE | uint 8/16/32   | Session handle    |
| data      | octet-stream      | bin 8/16/32    | Data to be signed |

**Response**

| Name      | Type                   | Representation | Description    |
|-----------|------------------------|----------------|----------------|
| status    | [CK_RV](#return-value) | uint 8/16/32   | Return value   |
| signature | octet-stream           | bin 8/16/32    | Data signature |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
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
- `CKR_SIGNATURE_INVALID`
- `CKR_SIGNATURE_LEN_RANGE`

#### Verify Update

Continues a multiple-part verification operation, processing another data part.

The verification operation must have been initialized with [VerifyInit](#verify-init). This function may be called any number of times in succession. A call to [VerifyUpdate](#verify-update) which results in an error terminates the current verification operation.

**Request**

| Name      | Type              | Representation | Description              |
|-----------|-------------------|----------------|--------------------------|
| hSession  | CK_SESSION_HANDLE | uint 8/16/32   | Session handle           |
| part      | octet-stream      | bin 8/16/32    | Data part to be verified |

**Response**

| Name   | Type                   | Representation | Description  |
|--------|------------------------|----------------|--------------|
| status | [CK_RV](#return-value) | uint 8/16/32   | Return value |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
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

#### Verify Final

Finishes a multiple-part verification operation, checking the signature.

The verification operation must have been initialized with [VerifyInit](#verify-init). A call to [VerifyFinal](#verify-final) always terminates the active verification operation.

A successful call to [VerifyFinal](#verify-final) should return either the value `CKR_OK` (indicating that the supplied signature is valid) or `CKR_SIGNATURE_INVALID` (indicating that the supplied signature is invalid). If the signature can be seen to be invalid purely on the basis of its length, then `CKR_SIGNATURE_LEN_RANGE` should be returned. In any of these cases, the active verifying operation is terminated.

**Request**

| Name      | Type              | Representation | Description    |
|-----------|-------------------|----------------|----------------|
| hSession  | CK_SESSION_HANDLE | uint 8/16/32   | Session handle |
| signature | octet-stream      | bin 8/16/32    | Data signature |

**Response**

| Name   | Type                   | Representation | Description  |
|--------|------------------------|----------------|--------------|
| status | [CK_RV](#return-value) | uint 8/16/32   | Return value |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
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
- `CKR_SIGNATURE_INVALID`
- `CKR_SIGNATURE_LEN_RANGE`

#### Verify Recover Init

Initializes a signature verification operation, where the data is recovered from the signature.

The `CKA_VERIFY_RECOVER` attribute of the verification key, which indicates whether the key supports verification where the data is recovered from the signature, must be `CK_TRUE`.

After calling [VerifyRecoverInit](#verify-recover-init), the application may call [VerifyRecover](#verify-recover) to verify a signature on data in a single part. The verification operation is active until the application uses a call to [VerifyRecover](#verify-recover) to actually obtain the recovered message.

**Request**

| Name      | Type                                  | Representation | Description                      |
|-----------|---------------------------------------|----------------|----------------------------------|
| hSession  | CK_SESSION_HANDLE                     | uint 8/16/32   | Session handle                   |
| mechanism | [CK_MECHANISM_TYPE](#mechanism-codes) | uint 8/16/32   | Signature Verification mechanism |
| parameter | octet-stream                          | bin 8/16/32    | Optional parameter               |
| hKey      | CK_OBJECT_HANDLE                      | uint 8/16/32   | Key Handle                       |

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

#### Verify Recover

Verifies a signature in a single-part operation, where the data is recovered from the signature.

The verification operation must have been initialized with [VerifyRecoverInit](#verify-recover-init). A call to [VerifyRecover](#verify-recover) always terminates the active verification operation unless it returns `CKR_BUFFER_TOO_SMALL` or is a successful call (i.e., one which returns `CKR_OK`) to determine the length of the buffer needed to hold the recovered data.

A successful call to [VerifyRecover](#verify-recover) should return either the value `CKR_OK` (indicating that the supplied signature is valid) or `CKR_SIGNATURE_INVALID` (indicating that the supplied signature is invalid). If the signature can be seen to be invalid purely on the basis of its length, then `CKR_SIGNATURE_LEN_RANGE` should be returned. The return codes `CKR_SIGNATURE_INVALID` and `CKR_SIGNATURE_LEN_RANGE` have a higher priority than the return code `CKR_BUFFER_TOO_SMALL`, i.e., if [VerifyRecover](#verify-recover) is supplied with an invalid signature, it will never return `CKR_BUFFER_TOO_SMALL`.

**Request**

| Name      | Type              | Representation | Description    |
|-----------|-------------------|----------------|----------------|
| hSession  | CK_SESSION_HANDLE | uint 8/16/32   | Session handle |
| signature | octet-stream      | bin 8/16/32    | Data signature |

**Response**

| Name   | Type                   | Representation | Description    |
|--------|------------------------|----------------|----------------|
| status | [CK_RV](#return-value) | uint 8/16/32   | Return value   |
| data   | octet-stream           | bin 8/16/32     | Data recovered |

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
- `CKR_SIGNATURE_LEN_RANGE`
- `CKR_SIGNATURE_INVALID`

### Dual-function Cryptographic Functions

#### Digest Encrypt Update

Continues multiple-part digest and encryption operations, processing another data part.

If a [DigestEncryptUpdate](#digest-encrypt-update) call does not produce encrypted output (because an error occurs, or because `encryptedPart` has the value NULL_PTR, then no plaintext is passed to the active digest operation.

Digest and encryption operations must both be active (they must have been initialized with [DigestInit](#digest-init) and [EncryptInit](#encrypt-init), respectively). This function may be called any number of times in succession, and may be interspersed with [DigestUpdate](#digest-update), [DigestKey](#digest-key), and [EncryptUpdate](#encrypt-update) calls (it would be somewhat unusual to intersperse calls to [DigestEncryptUpdate](#digest-encrypt-update) with calls to [DigestKey](#digest-key), however).

**Request**

| Name      | Type              | Representation | Description               |
|-----------|-------------------|----------------|---------------------------|
| hSession  | CK_SESSION_HANDLE | uint 8/16/32   | Session handle            |
| part      | octet-stream      | bin 8/16/32    | Data part to be encrypted |

**Response**

| Name          | Type                   | Representation | Description            |
|---------------|------------------------|----------------|------------------------|
| status        | [CK_RV](#return-value) | uint 8/16/32   | Return value           |
| encryptedPart | octet-stream           | bin 8/16/32    | Encrypted part of data |

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

#### Decrypt Digest Update

Continues a multiple-part combined decryption and digest operation, processing another data part.

If a [DecryptDigestUpdate](#decrypt-digest-update) call does not produce decrypted output (because an error occurs, or because `part` has the value `NULL_PTR`), then no plaintext is passed to the active digest operation.

Decryption and digesting operations must both be active (they must have been initialized with [DecryptInit](#decrypt-init) and [DigestInit](#digest-init), respectively). This function may be called any number of times in succession, and may be interspersed with [DecryptUpdate](#decrypt-update), [DigestUpdate](#digest-update), and [DigestKey](#digest-key) calls (it would be somewhat unusual to intersperse calls to [DecryptDigestUpdate](#decrypt-digest-update) with calls to [DigestKey](#digest-key), however).

Use of [DecryptDigestUpdate](#decrypt-digest-update) involves a pipelining issue that does not arise when using [DigestEncryptUpdate](#digest-encrypt-update), the "inverse function" of [DecryptDigestUpdate](#decrypt-digest-update). This is because when [DigestEncryptUpdate](#digest-encrypt-update) is called, precisely the same input is passed to both the active digesting operation and the active encryption operation; however, when [DecryptDigestUpdate](#decrypt-digest-update) is called, the input passed to the active digesting operation is the output of the active decryption operation. This issue comes up only when the mechanism used for decryption performs padding.

In particular, envision a 24-byte ciphertext which was obtained by encrypting an 18-byte plaintext with DES in CBC mode with PKCS padding. Consider an application which will simultaneously decrypt this ciphertext and digest the original plaintext thereby obtained.

After initializing decryption and digesting operations, the application passes the 24-byte ciphertext (3 DES blocks) into [DecryptDigestUpdate](#decrypt-digest-update). [DecryptDigestUpdate](#decrypt-digest-update) returns exactly 16 bytes of plaintext, since at this point, Cryptoki doesn't know if there's more ciphertext coming, or if the last block of ciphertext held any padding. These 16 bytes of plaintext are passed into the active digesting operation.

Since there is no more ciphertext, the application calls [DecryptFinal](#decrypt-final). This tells Cryptoki that there's no more ciphertext coming, and the call returns the last 2 bytes of plaintext. However, since the active decryption and digesting operations are linked only through the [DecryptDigestUpdate](#decrypt-digest-update) call, these 2 bytes of plaintext are not passed on to be digested.

A call to [DigestFinal](#digest-final), therefore, would compute the message digest of the first 16 bytes of the plaintext, not the message digest of the entire plaintext. It is crucial that, before [DigestFinal](#digest-final) is called, the last 2 bytes of plaintext get passed into the active digesting operation via a [DigestUpdate](#digest-update) call.

Because of this, it is critical that when an application uses a padded decryption mechanism with [DecryptDigestUpdate](#decrypt-digest-update), it knows exactly how much plaintext has been passed into the active digesting operation. Extreme caution is warranted when using a padded decryption mechanism with [DecryptDigestUpdate](#decrypt-digest-update).

**Request**

| Name          | Type              | Representation | Description         |
|---------------|-------------------|----------------|---------------------|
| hSession      | CK_SESSION_HANDLE | uint 8/16/32   | Session handle      |
| encryptedPart | octet-stream      | bin 8/16/32    | Encrypted data part |

**Response**

| Name   | Type                   | Representation | Description         |
|--------|------------------------|----------------|---------------------|
| status | [CK_RV](#return-value) | uint 8/16/32   | Return value        |
| part   | octet-stream           | bin 8/16/32    | Decrypted data part |

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

#### Sign Encrypt Update

Continues a multiple-part combined signature and encryption operation, processing another data part.

If a [SignEncryptUpdate](#sign-encrypt-update) call does not produce encrypted output (because an error occurs, or because encryptedPart has the value `NULL_PTR`), then no plaintext is passed to the active signing operation.

Signature and encryption operations must both be active (they must have been initialized with [SignInit](#sign-init) and [EncryptInit](#encrypt-init), respectively). This function may be called any number of times in succession, and may be interspersed with [SignUpdate](#sign-update) and [EncryptUpdate](#encrypt-update) calls.

**Request**

| Name      | Type              | Representation | Description               |
|-----------|-------------------|----------------|---------------------------|
| hSession  | CK_SESSION_HANDLE | uint 8/16/32   | Session handle            |
| part      | octet-stream      | bin 8/16/32    | Data part to be encrypted |

**Response**

| Name          | Type                   | Representation | Description            |
|---------------|------------------------|----------------|------------------------|
| status        | [CK_RV](#return-value) | uint 8/16/32   | Return value           |
| encryptedPart | octet-stream           | bin 8/16/32    | Encrypted part of data |

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
- `CKR_USER_NOT_LOGGED_IN`

#### Decrypt Verify Update

Continues a multiple-part combined decryption and verification operation, processing another data part.

If a [DecryptVerifyUpdate](#decrypt-verify-update) call does not produce decrypted output (because an error occurs, or because pPart has the value NULL_PTR, or because pulPartLen is too small to hold the entire encrypted part output), then no plaintext is passed to the active verification operation.

Decryption and signature operations must both be active (they must have been initialized with [DecryptInit](#decrypt-init) and [VerifyInit](#verify-init), respectively). This function may be called any number of times in succession, and may be interspersed with [DecryptUpdate](#decrypt-update) and [VerifyUpdate](#verify-update) calls.

Use of [DecryptVerifyUpdate](#decrypt-verify-update) involves a pipelining issue that does not arise when using [SignEncryptUpdate](#sign-encrypt-update), the "inverse function" of [DecryptVerifyUpdate](#decrypt-verify-update). This is because when [SignEncryptUpdate](#sign-encrypt-update) is called, precisely the same input is passed to both the active signing operation and the active encryption operation; however, when [DecryptVerifyUpdate](#decrypt-verify-update) is called, the input passed to the active verifying operation is the output of the active decryption operation. This issue comes up only when the mechanism used for decryption performs padding.

In particular, envision a 24-byte ciphertext which was obtained by encrypting an 18-byte plaintext with DES in CBC mode with PKCS padding. Consider an application which will simultaneously decrypt this ciphertext and verify a signature on the original plaintext thereby obtained.

After initializing decryption and verification operations, the application passes the 24-byte ciphertext (3 DES blocks) into [DecryptVerifyUpdate](#decrypt-verify-update). [DecryptVerifyUpdate](#decrypt-verify-update) returns exactly 16 bytes of plaintext, since at this point, Cryptoki doesn't know if there's more ciphertext coming, or if the last block of ciphertext held any padding. These 16 bytes of plaintext are passed into the active verification operation.

Since there is no more ciphertext, the application calls [DecryptFinal](#decrypt-final). This tells Cryptoki that there's no more ciphertext coming, and the call returns the last 2 bytes of plaintext. However, since the active decryption and verification operations are linked only through the [DecryptVerifyUpdate](#decrypt-verify-update) call, these 2 bytes of plaintext are not passed on to the verification mechanism.

A call to [VerifyFinal](#verify-final), therefore, would verify whether or not the signature supplied is a valid signature on the first 16 bytes of the plaintext, not on the entire plaintext. It is crucial that, before [VerifyFinal](#verify-final) is called, the last 2 bytes of plaintext get passed into the active verification operation via a [VerifyUpdate](#verify-update) call.

Because of this, it is critical that when an application uses a padded decryption mechanism with [DecryptVerifyUpdate](#decrypt-verify-update), it knows exactly how much plaintext has been passed into the active verification operation. Extreme caution is warranted when using a padded decryption mechanism with [DecryptVerifyUpdate](#decrypt-verify-update).

**Request**

| Name          | Type              | Representation | Description         |
|---------------|-------------------|----------------|---------------------|
| hSession      | CK_SESSION_HANDLE | uint 8/16/32   | Session handle      |
| encryptedPart | octet-stream      | bin 8/16/32    | Encrypted data part |

**Response**

| Name   | Type                   | Representation | Description         |
|--------|------------------------|----------------|---------------------|
| status | [CK_RV](#return-value) | uint 8/16/32   | Return value        |
| part   | octet-stream           | bin 8/16/32    | Decrypted data part |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
- `CKR_BUFFER_TOO_SMALL`
- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DATA_LEN_RANGE`
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

### Key Management Functions

#### Generate Key

Generates a secret key or set of domain parameters, creating a new object.

If the generation mechanism is for domain parameter generation, the `CKA_CLASS` attribute will have the value `CKO_DOMAIN_PARAMETERS`; otherwise, it will have the value `CKO_SECRET_KEY`.

Since the type of key or domain parameters to be generated is implicit in the generation mechanism, the template does not need to supply a key type. If it does supply a key type which is inconsistent with the generation mechanism, [GenerateKey](#generate-key) fails and returns the error code `CKR_TEMPLATE_INCONSISTENT`. The `CKA_CLASS` attribute is treated similarly.

If a call to [GenerateKey](#generate-key) cannot support the precise template supplied to it, it will fail and return without creating an object.

The object created by a successful call to [GenerateKey](#generate-key) will have its `CKA_LOCAL` attribute set to `CK_TRUE`.

**Request**

| Name      | Type                                                                             | Representation | Description                       |
|-----------|----------------------------------------------------------------------------------|----------------|-----------------------------------|
| hSession  | CK_SESSION_HANDLE                                                                | uint 8/16/32   | Session handle                    |
| mechanism | [CK_MECHANISM_TYPE](#mechanism-codes)                                            | uint 8/16/32   | Cipher mechanism                  |
| parameter | octet-stream                                                                     | bin 8/16/32    | Optional parameter                |
| templates | map of [CK_ATTRIBUTE_TYPE](#attribute-types) and octet-stream of attribute value | map 16/32      | Map of attribute and their values |

**Response**

| Name   | Type                   | Representation | Description        |
|--------|------------------------|----------------|--------------------|
| status | [CK_RV](#return-value) | uint 8/16/32   | Return value       |
| hKey   | CK_OBJECT_HANDLE        uint 8/16/32    | Created Key Handle |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
- `CKR_ATTRIBUTE_READ_ONLY`
- `CKR_ATTRIBUTE_TYPE_INVALID`
- `CKR_ATTRIBUTE_VALUE_INVALID`
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
- `CKR_SESSION_READ_ONLY`
- `CKR_TEMPLATE_INCOMPLETE`
- `CKR_TEMPLATE_INCONSISTENT`
- `CKR_TOKEN_WRITE_PROTECTED`
- `CKR_USER_NOT_LOGGED_IN`

#### Generate Key Pair

Generates a public/private key pair, creating new key objects.

Since the types of keys to be generated are implicit in the key pair generation mechanism, the templates do not need to supply key types. If one of the templates does supply a key type which is inconsistent with the key generation mechanism, [GenerateKeyPair](#generate-key-pair) fails and returns the error code `CKR_TEMPLATE_INCONSISTENT`. The `CKA_CLASS` attribute is treated similarly.

If a call to [GenerateKeyPair](#generate-key-pair) cannot support the precise templates supplied to it, it will fail and return without creating any key objects.

A call to [GenerateKeyPair](#generate-key-pair) will never create just one key and return. A call can fail, and create no keys; or it can succeed, and create a matching public/private key pair.

The key objects created by a successful call to [GenerateKeyPair](#generate-key-pair) will have their `CKA_LOCAL` attributes set to `CK_TRUE`.

**Request**

| Name                | Type                                                                             | Representation | Description                                      |
|---------------------|----------------------------------------------------------------------------------|----------------|--------------------------------------------------|
| hSession            | CK_SESSION_HANDLE                                                                | uint 8/16/32   | Session handle                                   |
| mechanism           | [CK_MECHANISM_TYPE](#mechanism-codes)                                            | uint 8/16/32   | Cipher mechanism                                 |
| parameter           | octet-stream                                                                     | bin 8/16/32    | Optional parameter                               |
| publicKeyTemplates  | map of [CK_ATTRIBUTE_TYPE](#attribute-types) and octet-stream of attribute value | map 16/32      | Map of attribute and their values of public key  |
| privateKeyTemplates | map of [CK_ATTRIBUTE_TYPE](#attribute-types) and octet-stream of attribute value | map 16/32      | Map of attribute and their values of private key |

**Response**

| Name        | Type                   | Representation | Description               |
|-------------|------------------------|----------------|---------------------------|
| status      | [CK_RV](#return-value) | uint 8/16/32   | Return value              |
| hPublicKey  | CK_OBJECT_HANDLE       | uint 8/16/32   | Created Public Key Handle |
| hPrivateKey | CK_OBJECT_HANDLE       | uint 8/16/32   | Created Priate Key Handle |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
- `CKR_ATTRIBUTE_READ_ONLY`
- `CKR_ATTRIBUTE_TYPE_INVALID`
- `CKR_ATTRIBUTE_VALUE_INVALID`
- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_DOMAIN_PARAMS_INVALID`
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
- `CKR_SESSION_READ_ONLY`
- `CKR_TEMPLATE_INCOMPLETE`
- `CKR_TEMPLATE_INCONSISTENT`
- `CKR_TOKEN_WRITE_PROTECTED`
- `CKR_USER_NOT_LOGGED_IN`

#### Wrap Key

Wraps (i.e., encrypts) a private or secret key.

The CKA_WRAP attribute of the wrapping key, which indicates whether the key supports wrapping, must be `CK_TRUE`. The `CKA_EXTRACTABLE` attribute of the key to be wrapped must also be `CK_TRUE`.

If the key to be wrapped cannot be wrapped for some token-specific reason, despite its having its `CKA_EXTRACTABLE` attribute set to `CK_TRUE`, then [WrapKey](#wrap-key) fails with error code `CKR_KEY_NOT_WRAPPABLE`. If it cannot be wrapped with the specified wrapping key and mechanism solely because of its length, then [WrapKey](#wrap-key) fails with error code `CKR_KEY_SIZE_RANGE`.

[WrapKey](#wrap-key) can be used in the following situations:

To wrap any secret key with a public key that supports encryption and decryption.

To wrap any secret key with any other secret key. Consideration must be given to key size and mechanism strength or the token may not allow the operation.

To wrap a private key with any secret key. Of course, tokens vary in which types of keys can actually be wrapped with which mechanisms.

To partition the wrapping keys so they can only wrap a subset of extractable keys the attribute `CKA_WRAP_TEMPLATE` can be used on the wrapping key to specify an attribute set that will be compared against the attributes of the key to be wrapped. If all attributes match according to the [FindObjects](#find-objects) rules of attribute matching then the wrap will proceed. The value of this attribute is an attribute template and the size is the number of items in the template times the size of CK_ATTRIBUTE. If this attribute is not supplied then any template is acceptable. Attributes not present are not checked. If any attribute mismatch occurs on an attempt to wrap a key then the function shall return `CKR_KEY_HANDLE_INVALID`.

**Request**

| Name         | Type                                  | Representation | Description            |
|--------------|---------------------------------------|----------------|------------------------|
| hSession     | CK_SESSION_HANDLE                     | uint 8/16/32   | Session handle         |
| mechanism    | [CK_MECHANISM_TYPE](#mechanism-codes) | uint 8/16/32   | Key wrapping mechanism |
| parameter    | octet-stream                          | bin 8/16/32    | Optional parameter     |
| hWrappingKey | CK_OBJECT_HANDLE                      | uint 8/16/32   | Wrapping key handle    |
| hKey         | CK_OBJECT_HANDLE                      | uint 8/16/32   | Wrapped key handle     |

**Response**

| Name       | Type                   | Representation | Description                |
|------------|------------------------|----------------|----------------------------|
| status     | [CK_RV](#return-value) | uint 8/16/32   | Return value               |
| wrappedKey | octet-stream           | bin 8/16/32    | Serialized and wrapped key |

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
- `CKR_KEY_HANDLE_INVALID`
- `CKR_KEY_NOT_WRAPPABLE`
- `CKR_KEY_SIZE_RANGE`
- `CKR_KEY_UNEXTRACTABLE`
- `CKR_MECHANISM_INVALID`
- `CKR_MECHANISM_PARAM_INVALID`
- `CKR_OK`
- `CKR_OPERATION_ACTIVE`
- `CKR_PIN_EXPIRED`
- `CKR_SESSION_CLOSED`
- `CKR_SESSION_HANDLE_INVALID`
- `CKR_USER_NOT_LOGGED_IN`
- `CKR_WRAPPING_KEY_HANDLE_INVALID`
- `CKR_WRAPPING_KEY_SIZE_RANGE`
- `CKR_WRAPPING_KEY_TYPE_INCONSISTENT`

#### Unwrap Key

Unwraps (i.e. decrypts) a wrapped key, creating a new private key or secret key object.

The `CKA_UNWRAP` attribute of the unwrapping key, which indicates whether the key supports unwrapping, must be `CK_TRUE`.

The new key will have the `CKA_ALWAYS_SENSITIVE` attribute set to `CK_FALSE`, and the `CKA_NEVER_EXTRACTABLE` attribute set to `CK_FALSE`. The `CKA_EXTRACTABLE` attribute is by default set to `CK_TRUE`.

Some mechanisms may modify, or attempt to modify. the contents of the pMechanism structure at the same time that the key is unwrapped.

If a call to [UnwrapKey](#unwrap-key) cannot support the precise template supplied to it, it will fail and return without creating any key object.

The key object created by a successful call to [UnwrapKey](#unwrap-key) will have its `CKA_LOCAL` attribute set to `CK_FALSE`.

To partition the unwrapping keys so they can only unwrap a subset of keys the attribute `CKA_UNWRAP_TEMPLATE` can be used on the unwrapping key to specify an attribute set that will be added to attributes of the key to be unwrapped. If the attributes do not conflict with the user supplied attribute template, in 'template', then the unwrap will proceed. The value of this attribute is an attribute template and the size is the number of items in the template times the size of `CK_ATTRIBUTE`. If this attribute is not present on the unwrapping key then no additional attributes will be added. If any attribute conflict occurs on an attempt to unwrap a key then the function shall return `CKR_TEMPLATE_INCONSISTENT`.

**Request**

| Name           | Type                                  | Representation | Description                |
|----------------|---------------------------------------|----------------|----------------------------|
| hSession       | CK_SESSION_HANDLE                     | uint 8/16/32   | Session handle             |
| mechanism      | [CK_MECHANISM_TYPE](#mechanism-codes) | uint 8/16/32   | Key unwrapping mechanism   |
| parameter      | octet-stream                          | bin 8/16/32    | Optional parameter         |
| hUnwrappingKey | CK_OBJECT_HANDLE                      | uint 8/16/32   | Unwrapping key handle      |
| wrappedKey     | octet-stream                          | bin 8/16/32    | Serialized and wrapped key |

**Response**

| Name        | Type                                                                             | Representation | Description                       |
|-------------|----------------------------------------------------------------------------------|----------------|-----------------------------------|
| status      | [CK_RV](#return-value)                                                           | uint 8/16/32   | Return value                      |
| template    | map of [CK_ATTRIBUTE_TYPE](#attribute-types) and octet-stream of attribute value | map 16/32      | Map of attribute and their values |
| hKey        | CK_OBJECT_HANDLE                                                                 | uint 8/16/32   | Unwrapped key handle              |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
- `CKR_ATTRIBUTE_READ_ONLY`
- `CKR_ATTRIBUTE_TYPE_INVALID`
- `CKR_ATTRIBUTE_VALUE_INVALID`
- `CKR_BUFFER_TOO_SMALL`
- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_DOMAIN_PARAMS_INVALID`
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
- `CKR_SESSION_READ_ONLY`
- `CKR_TEMPLATE_INCOMPLETE`
- `CKR_TEMPLATE_INCONSISTENT`
- `CKR_TOKEN_WRITE_PROTECTED`
- `CKR_UNWRAPPING_KEY_HANDLE_INVALID`
- `CKR_UNWRAPPING_KEY_SIZE_RANGE`
- `CKR_UNWRAPPING_KEY_TYPE_INCONSISTENT`
- `CKR_USER_NOT_LOGGED_IN`
- `CKR_WRAPPED_KEY_INVALID`
- `CKR_WRAPPED_KEY_LEN_RANGE`

#### Derive Key

Derives a key from a base key, creating a new key object.

The values of the `CK_SENSITIVE`, `CK_ALWAYS_SENSITIVE`, `CK_EXTRACTABLE`, and `CK_NEVER_EXTRACTABLE` attributes for the base key affect the values that these attributes can hold for the newly-derived key. If a call to [DeriveKey](#derive-key) cannot support the precise template supplied to it, it will fail and return without creating any key object.

The key object created by a successful call to [DeriveKey](#derive-key) will have its `CKA_LOCAL` attribute set to `CK_FALSE`.

**Request**

| Name      | Type                                                                             | Representation | Description                       |
|-----------|----------------------------------------------------------------------------------|----------------|-----------------------------------|
| hSession  | CK_SESSION_HANDLE                                                                | uint 8/16/32   | Session handle                    |
| mechanism | [CK_MECHANISM_TYPE](#mechanism-codes)                                            | uint 8/16/32   | Key unwrapping mechanism          |
| parameter | octet-stream                                                                     | bin 8/16/32    | Optional parameter                |
| hBaseKey  | CK_OBJECT_HANDLE                                                                 | uint 8/16/32   | Base key handle                   |
| template  | map of [CK_ATTRIBUTE_TYPE](#attribute-types) and octet-stream of attribute value | map 16/32      | Map of attribute and their values |

**Response**

| Name        | Type                                                                             | Representation | Description        |
|-------------|----------------------------------------------------------------------------------|----------------|--------------------|
| status      | [CK_RV](#return-value)                                                           | uint 8/16/32   | Return value       |
| hKey        | CK_OBJECT_HANDLE                                                                 | uint 8/16/32   | Derived key handle |

**Error Codes**

- `CKR_ARGUMENTS_BAD`
- `CKR_ATTRIBUTE_READ_ONLY`
- `CKR_ATTRIBUTE_TYPE_INVALID`
- `CKR_ATTRIBUTE_VALUE_INVALID`
- `CKR_CRYPTOKI_NOT_INITIALIZED`
- `CKR_DEVICE_ERROR`
- `CKR_DEVICE_MEMORY`
- `CKR_DEVICE_REMOVED`
- `CKR_DOMAIN_PARAMS_INVALID`
- `CKR_FUNCTION_CANCELED`
- `CKR_FUNCTION_FAILED`
- `CKR_GENERAL_ERROR`
- `CKR_HOST_MEMORY`
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
- `CKR_SESSION_READ_ONLY`
- `CKR_TEMPLATE_INCOMPLETE`
- `CKR_TEMPLATE_INCONSISTENT`
- `CKR_TOKEN_WRITE_PROTECTED`
- `CKR_USER_NOT_LOGGED_IN`


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

### Attribute Types

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

### States

Possible values of `CK_STATE`

| Code                     | Value |
|--------------------------|-------|
| `CKS_RO_PUBLIC_SESSION`  | 0     |
| `CKS_RO_USER_FUNCTIONS`  | 1     |
| `CKS_RW_PUBLIC_SESSION`  | 2     |
| `CKS_RW_USER_FUNCTIONS`  | 3     |
| `CKS_RW_SO_FUNCTIONS`    | 4     |

### User Types

Possible value of `CK_USER_TYPE`

| Code                 | Value |
|----------------------|-------|
| CKU_SO               | 0     |
| CKU_USER             | 1     |
| CKU_CONTEXT_SPECIFIC | 2     |

### Object Classes

Possible values of `CK_OBJECT_CLASS`

| Code                    | Value      |
|-------------------------|------------|
| `CKO_DATA`              | 0x00000000 |
| `CKO_CERTIFICATE`       | 0x00000001 |
| `CKO_PUBLIC_KEY`        | 0x00000002 |
| `CKO_PRIVATE_KEY`       | 0x00000003 |
| `CKO_SECRET_KEY`        | 0x00000004 |
| `CKO_HW_FEATURE`        | 0x00000005 |
| `CKO_DOMAIN_PARAMETERS` | 0x00000006 |
| `CKO_MECHANISM`         | 0x00000007 |
| `CKO_OTP_KEY`           | 0x00000008 |
| `CKO_VENDOR_DEFINED`    | 0x80000000 |
