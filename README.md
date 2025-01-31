# [Superduper]([https://elkmire.github.io/Crystal/](https://elkmire.github.io/Superduper/))
click here^


## System Overview
Superduper is a browser-based cryptographic system that combines multiple security inputs (two numeric seeds and an alphanumeric key) with AES encryption for secure text transformation. The system provides deterministic encryption with enhanced security and memory optimization.

## Security Implementation

### Key Components

#### 1. Input Parameters
- Primary Seed (1-1000)
- Secondary Seed (1-1000)
- Alphanumeric Key (unlimited length)

#### 2. Key Derivation (PBKDF2)
```javascript
function deriveKey(s1, s2, alphaKey, salt) {
    const keyMaterial = `${s1 * 16807}_${s2 * 48271}_${alphaKey}`;
    return CryptoJS.PBKDF2(keyMaterial, salt, {
        keySize: 256/32,
        iterations: 10000,
        hasher: CryptoJS.algo.SHA256
    });
}
```

#### 3. Salt Generation
```javascript
function generateSalt(s1, s2, alphaKey) {
    return CryptoJS.SHA256(`salt_${s1}_${s2}_${alphaKey}`).toString().slice(0, 32);
}
```

#### 4. Encryption (AES-CBC)
```javascript
function encrypt(text, key, salt) {
    const iv = CryptoJS.enc.Hex.parse(salt.slice(0, 32));
    const textBytes = CryptoJS.enc.Utf8.parse(text);
    return CryptoJS.AES.encrypt(textBytes, key, {
        mode: CryptoJS.mode.CBC,
        padding: CryptoJS.pad.Pkcs7,
        iv: iv
    }).toString();
}
```

## Memory Optimization

### Chunked Processing
- Chunk size: 1MB (1024 * 1024 bytes)
- Efficient memory usage for large inputs
- Automatic garbage collection optimization

### Memory Management
- Minimized temporary object creation
- Efficient string handling
- Optimized buffer usage
- Memory-conscious data structures

## Security Properties

### Encryption Characteristics
- Key Space: 1000 × 1000 × alphanumeric combinations
- Key Derivation: PBKDF2 with 10,000 iterations
- Encryption: AES-256 in CBC mode
- Salt: Unique per key combination
- IV: Derived from salt

### Security Features
1. Multi-Factor Security
   - Two numeric seeds
   - Alphanumeric key
   - Combined entropy pool

2. Enhanced Key Derivation
   - PBKDF2 with high iteration count
   - Multiple input factors
   - Salt-based strengthening

3. Output Security
   - Deterministic but secure
   - Salt inclusion for verification
   - Format preservation

## Implementation Details

### Data Flow
1. Input Collection
   - Validate numeric seeds
   - Process alphanumeric key
   - Prepare input text

2. Key Generation
   - Generate salt from all inputs
   - Derive key using PBKDF2
   - Generate IV from salt

3. Encryption/Decryption
   - Process in chunks if needed
   - Apply AES encryption
   - Format output with salt

### Output Format
```
[32-byte-salt-hex]:[base64-encrypted-data]
```

## Interface Features

### Copy to Clipboard
- One-click copy functionality
- Multiple clipboard APIs supported
- Fallback mechanisms:
  1. Modern Clipboard API
  2. execCommand fallback
  3. Selection-based backup

### Visual Feedback
- Success/failure indicators
- Copy confirmation
- Error messages
- Size statistics

## Technical Specifications

### Input Constraints
- Primary Seed: 1-1000 (integer)
- Secondary Seed: 1-1000 (integer)
- Alphanumeric Key: Unlimited length
- Text: UTF-8 encoded

### Output Properties
- Format: Salt:Ciphertext
- Encoding: Base64
- Size Ratio: ~1.4x input + salt
- Character Set: [A-Za-z0-9+/=]

## Performance Characteristics

### Time Complexity
- Key Derivation: O(iterations)
- Encryption/Decryption: O(n)
- Memory Usage: O(chunk_size)
- Overall: O(n) where n is input size

### Space Requirements
- Runtime Memory: ≤ 1MB per chunk
- Temporary Storage: ~2x chunk size
- Key Material: 256 bits
- Salt: 128 bits

## Error Handling

### Common Errors
1. Encryption Failures
   - Invalid input text
   - Memory limitations
   - Encoding issues

2. Decryption Failures
   - Invalid format
   - Incorrect keys
   - Corrupted data

3. Clipboard Errors
   - Permission denied
   - API unavailable
   - Buffer limitations

## Best Practices

### Usage Guidelines
1. Use strong alphanumeric keys
2. Change seeds regularly
3. Verify output after encoding
4. Keep all three keys secure
5. Test with small inputs first

### Security Recommendations
1. Use unique key combinations
2. Avoid predictable keys
3. Keep keys private
4. Verify decoded output
5. Monitor memory usage

## Browser Compatibility

### Minimum Requirements
- Modern JavaScript support
- Web Crypto API
- Clipboard API (optional)
- UTF-8 encoding support

### Tested Browsers
- Chrome 80+
- Firefox 75+
- Safari 13+
- Edge 80+

## Known Limitations

### Technical Constraints
1. Browser memory limits
2. Large file handling
3. Key space constraints
4. Synchronous processing

### Security Boundaries
1. No perfect forward secrecy
2. Deterministic output
3. Limited seed range
4. Browser security context


## License
MIT License - Free to use, modify, and distribute with attribution.
