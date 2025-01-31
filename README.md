# [Superduper](https://elkmire.github.io/Superduper/)
click here^

# Superduper Technical Manual

## Overview
Superduper is a web-based encryption/decryption tool implementing secure text transformation with error correction.

## Security Features
- AES-256 encryption in CBC mode
- PBKDF2 key derivation (10,000 iterations)
- HMAC-SHA256 message authentication
- Ephemeral key generation
- Hamming code error detection/correction
- Salt generation for each message

## Interface Components
- Primary Seed (1-1000)
- Secondary Seed (1-1000)
- User Key input
- Mode toggle (Encode/Decode)
- Input/Output text areas
- Copy button
- Status display

## Core Functions

### Key Generation
```javascript
function deriveKey(s1, s2, alphaKey, salt, ephemeralKey) {
    const keyMaterial = `${s1 * 16807}_${s2 * 48271}_${alphaKey}_${ephemeralKey}`;
    return CryptoJS.PBKDF2(keyMaterial, salt, {
        keySize: 256/32,
        iterations: ITERATIONS,
        hasher: CryptoJS.algo.SHA256
    });
}
```

### Encryption Process
1. Generate ephemeral key
2. Create salt from seeds and user key
3. Derive encryption key
4. Encrypt data with AES-256
5. Compute HMAC
6. Apply Hamming encoding

### Error Correction
Implements Hamming code with parity checking:
- Each byte gets a parity bit
- Detects and reports data corruption
- Base64 encoding for transmission

## Usage Instructions

1. Select operating mode (Encode/Decode)
2. Enter two numeric seeds (1-1000)
3. Provide encryption key
4. Input text for processing
5. Copy output using button

## Technical Specifications
- Encryption: AES-256-CBC
- Authentication: HMAC-SHA256
- Key Derivation: PBKDF2 (10k iterations)
- Error Correction: Hamming code
- Encoding: Base64
- Dependencies: CryptoJS

## Performance
- Real-time processing with 300ms debounce
- Supports text input up to browser memory limits
- Size statistics shown in B/KB/MB

## Error Handling
- Invalid format detection
- Authentication failure alerts
- Error correction failure reporting
- Input validation for seeds

## Security Considerations
- No data persistence
- Client-side only processing
- Ephemeral key rotation
- Message authentication
- Robust key derivation
3. Limited seed range
4. Browser security context


## License
MIT License - Free to use, modify, and distribute with attribution.
