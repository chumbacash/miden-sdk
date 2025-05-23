# Miden Python SDK

A Python SDK for interacting with the Miden blockchain, providing a Pythonic interface to Miden's core features without requiring Rust expertise.

![Miden](https://img.shields.io/badge/blockchain-Polygon%20Miden-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-Alpha-orange)

## Overview

Miden is a zkRollup secured by Ethereum and the AggLayer, offering privacy, client-side proving, parallel execution, and Turing-complete smart contracts. This SDK provides a Pythonic interface to Miden's core features:

- **Node Communication**: Interact with Miden nodes via gRPC
- **Wallet & Account Management**: Create and manage private/public and mutable/immutable accounts
- **Transaction Building**: Construct and submit transactions
- **STARK Proof Generation**: Generate proofs using the Miden CLI
- **Note Management**: Create, import, and export private or public notes

## Installation

1. Install the Miden client:
```bash
cargo install miden-client
```

2. Install the Python SDK:
```bash
pip install miden-sdk
```

3. Download and place the WASM module:
```bash
# Install @demox-labs/miden-sdk
npm install @demox-labs/miden-sdk

# Copy WASM module to SDK directory
cp node_modules/@demox-labs/miden-sdk/dist/miden_sdk.wasm miden_sdk/wasm/
```

## Quick Start

### Create a Wallet

```python
from miden_sdk import MidenClient, Wallet

# Initialize client with testnet endpoint
client = MidenClient(rpc_endpoint="http://18.203.155.106:57291")

# Create a private, mutable wallet
wallet = client.new_wallet(storage_mode="private", mutable=True)

# Save wallet to JSON
wallet.save("wallet.json")
print(f"Wallet created: {wallet.account_id}")
```

### Send a Payment

```python
from miden_sdk import MidenClient, Wallet, Transaction, NoteType

# Initialize client
client = MidenClient(rpc_endpoint="http://18.203.155.106:57291")

# Load wallet
wallet = Wallet.load("wallet.json", client=client)

# Create a payment transaction
tx = Transaction.pay_to_id(
    sender=wallet,
    recipient_address="0xABC1234567890...",
    amount=100,
    faucet_id="0xDEF4567890...",
    note_type=NoteType.PRIVATE
)

# Generate STARK proof
tx.generate_proof()

# Submit transaction
tx_id = client.send_transaction(tx)
print(f"Transaction submitted: {tx_id}")
```

### Import a Note

```python
from miden_sdk import MidenClient, Note

# Initialize client
client = MidenClient(rpc_endpoint="http://18.203.155.106:57291")

# Import note from JSON
note = Note.import_note("note.json")

# Add to client's note store
client.add_note(note)
print(f"Note imported: {note.id}")
```

## Project Structure

```
miden-sdk/
├── miden_sdk/
│   ├── __init__.py
│   ├── client.py        # gRPC and WASM integration
│   ├── wallet.py        # Keypair and account management
│   ├── transaction.py   # Transaction construction
│   ├── note.py          # Note creation and querying
│   ├── config.py        # Constants and configurations
│   ├── utils.py         # Utility functions
├── tests/               # Unit and integration tests
├── examples/            # Example scripts
```

## Requirements

- Python 3.7+
- miden-client (Rust)
- @demox-labs/miden-sdk WASM module

## Development

1. Clone the repository:
```bash
git clone https://github.com/chumbacash/miden-sdk.git
cd miden-sdk
```

2. Install development dependencies:
```bash
pip install -e ".[dev]"
```

3. Run tests:
```bash
pytest
```

## Features

- **Intuitive API Design**: User-friendly interface following Python idioms
- **Clear Error Handling**: Custom exceptions with informative messages
- **Type Hinting**: Comprehensive type hints for IDE support
- **Modular Architecture**: Clean separation of concerns

## Status

This SDK is in alpha stage and compatible with Miden Testnet v6 (released February 2025). It will be updated as Miden evolves toward mainnet launch in late 2025.

## Resources

- [Miden Documentation](https://0xmiden.github.io/miden-node/index.html)
- [Miden GitHub](https://github.com/0xMiden)
- [Miden Client](https://github.com/0xMiden/miden-client)
- [npm SDK](https://www.npmjs.com/package/@demox-labs/miden-sdk)

## License

MIT License

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request. 
