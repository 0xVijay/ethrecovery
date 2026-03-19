# SimplGen - Unified BIP39 Toolkit

A single, unified binary for BIP39 mnemonic generation, extraction, and validation with automatic CPU/GPU detection.

## 🚀 Features

- **Single Binary**: All functionality in one executable
- **Auto-Detection**: Automatically detects and uses CUDA GPU when available
- **No Hardcoded Values**: Fully configurable via command line arguments
- **Complete BIP39 Support**: Generation, validation, extraction, and search
- **Production Ready**: Clean CMake build system

## 🏗️ Build Instructions

### Prerequisites
- **C++17 compiler** (GCC 7+, Clang 5+, MSVC 2017+)
- **CMake 3.18+**
- **OpenSSL** (for cryptographic functions)
- **CUDA Toolkit** (optional - auto-detected for GPU acceleration)

### macOS
```bash
# Install dependencies
brew install cmake openssl

# Optional: Install CUDA for GPU acceleration
# Download from: https://developer.nvidia.com/cuda-downloads
```

### Ubuntu/Debian
```bash
# Install dependencies
sudo apt update
sudo apt install build-essential cmake libssl-dev

# Optional: Install CUDA
sudo apt install nvidia-cuda-toolkit
```

### Build
```bash
# Clone and build
git clone <your-repo>
cd simplegen
mkdir build && cd build
cmake .. && make -j$(nproc)
```

The build system automatically:
- ✅ Detects CUDA availability
- ✅ Configures GPU acceleration if available
- ✅ Falls back to CPU-only if CUDA not found
- ✅ Links OpenSSL libraries

## 📖 Usage

### Basic Commands

```bash
# Generate binary combinations from config
./simplegen generate <config_file> <output_file> [wordlist]

# Validate and search for target address
./simplegen validate <binary_file> <target_address> [derivation_path]

# Extract combinations to text file
./simplegen extract <config_file> <binary_file> <output_file>

# Show binary file information
./simplegen info <config_file> <binary_file>

# Search for specific mnemonic
./simplegen search <config_file> <binary_file> "word1 word2 ..."
```

### Examples

```bash
# Generate combinations
./simplegen generate data/config.txt data/output.bin

# Find specific Ethereum address
./simplegen validate data/output.bin 0xb6716976A3ebe8D39aCEB04372f22Ff8e6802D7A m/44'/60'/0'/0/2

# Extract all combinations to text
./simplegen extract data/config.txt data/output.bin results.txt

# Show file statistics
./simplegen info data/config.txt data/output.bin

# Search for known mnemonic
./simplegen search data/config.txt data/output.bin "abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon abandon about"
```

## 📁 Configuration Structure

### Config File Format

Create a configuration file with word choices for each of the 12 mnemonic positions:

```
# config.txt - Each line represents choices for one position
abandon,ability,able,about,above
abandon
abandon,abstract,absurd,abuse
abandon,ability
abandon
abandon
abandon
abandon
abandon
abandon
abandon,absurd,abuse,ability
abandon,ability,about,above,absent
```

**Rules:**
- Each line = one mnemonic position (12 lines total)
- Comma-separated word choices per position
- Single word = fixed position
- Multiple words = will generate all combinations
- Lines starting with `#` are comments (ignored)
- Empty lines are ignored

### BIP44 Derivation Paths

Common Ethereum derivation paths:
- `m/44'/60'/0'/0/0` - First address (most common)
- `m/44'/60'/0'/0/1` - Second address
- `m/44'/60'/0'/0/2` - Third address
- `m/44'/60'/1'/0/0` - Second account, first address

## 🔧 Hardware Detection

The application automatically detects your hardware at runtime:

### With CUDA GPU:
```
🚀 GPU Detected: NVIDIA GeForce RTX 4090
   CUDA Devices: 1
   Compute: 8.9
   Memory: 24564 MB
   CPU Cores: 16
```

### CPU Only:
```
💻 CPU-only mode (CUDA not available)
   CPU Cores: 8
```

**No configuration required** - the optimal execution path is chosen automatically.

## 📊 File Formats

### Binary Output Format

Generated binary files contain:
```
Header (12 bytes):
- Total combinations (4 bytes)
- Valid combinations (4 bytes) 
- Bytes per combination (4 bytes)

Data:
- Binary-packed mnemonic combinations
- Only BIP39-valid combinations stored
```

### Directory Structure
```
simplegen/
├── CMakeLists.txt          # Build configuration
├── simplegen.cpp           # Main application
├── cuda/
│   └── unified_cuda.cu     # GPU acceleration
├── src/crypto/             # Cryptographic library
│   ├── sha512.cpp
│   ├── hmac_sha512.cpp
│   ├── keccak.cpp
│   ├── bip32.cpp
│   └── *.h
└── data/                   # Example configs
    ├── wordlist.txt        # BIP39 wordlist
    ├── config.txt          # Example config
    └── testing.txt         # Test config
```

## 🧪 Testing

Test with provided example data:
```bash
# Generate test data
./simplegen generate data/testing.txt data/test_output.bin

# Show test results
./simplegen info data/testing.txt data/test_output.bin

# Search for known test mnemonic
./simplegen validate data/test_output.bin 0xb6716976A3ebe8D39aCEB04372f22Ff8e6802D7A m/44'/60'/0'/0/2
```

## ⚡ Performance

- **CPU Mode**: Multi-threaded processing using all available cores
- **GPU Mode**: CUDA-accelerated parallel processing (10-100x faster)
- **Memory Efficient**: Streams large datasets without loading entirely into RAM
- **Optimized**: BIP39 validation with pre-computed lookup tables

## 🔐 Security

- **Real Crypto**: Uses OpenSSL for all cryptographic operations
- **BIP Standards**: Full BIP32/BIP39/BIP44 compliance
- **No Mock Data**: All generated addresses are cryptographically valid
- **Deterministic**: Same input always produces same output

## 📄 License

MIT License - see LICENSE file for details.

## 🤝 Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 🐛 Issues

Report bugs and feature requests through GitHub Issues.

---

**Ready for production use** - Single binary, auto-detection, zero configuration required!



./build/simplegen generate data/testing.txt testing_combinations.bin

./build/simplegen validate data/testing.txt testing_combinations.bin 0x59394DB1d80E78bF271B7693b22FD522C6CA572a "m/44'/60'/0'/0/2"
