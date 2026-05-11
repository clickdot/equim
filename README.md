# equim

equim miner CLI guide

## What is Equium?

Equium is a fair-launched, CPU-mineable token on Solana with Bitcoin-style economics: 21M total supply cap, halvings, ~1-minute blocks, and no pre-mine for most of the supply. It uses Equihash (96,5), which is memory-hard and favors CPUs over GPUs/ASICS.

**Key Features:**
- **Total supply:** 21,000,000 EQM (capped forever)
- **Mineable:** ~90% (18.9M EQM)
- **Block reward:** Starts at 25 EQM, halves every ~8.6 months
- **PoW:** Equihash (memory-bound — CPUs are competitive)
- **Network:** Solana mainnet (low fees)
- **Mint:** 1MhvZzEe8gQ8Rb9CrT3Dn26Gkn9QRErzLMGkkTwveqm (always verify)

Mined EQM lands automatically in your wallet's associated token account. You need a tiny bit of SOL for transaction fees.

## Prerequisites

- A Solana-compatible wallet (e.g., Phantom, Backpack, or CLI keypair)
- Linux (Ubuntu recommended), macOS, or Windows (with WSL for CLI)
- For VPS/headless: A basic CPU VPS works well (more cores = higher chance of winning blocks)
- Small amount of SOL for fees (~0.001 SOL per successful block or less)
- Optional but recommended: Helius RPC API key (free tier available) for reliable mining

## Mining Options

### Option 1: Easiest — Browser Miner (Quick Start)

1. Go to equium.xyz/mine
2. Use the built-in encrypted wallet or connect your own
3. Start mining directly in the browser

**Best for:** Testing. Not ideal for 24/7 operation.

### Option 2: Desktop App (Recommended for Most Users)

1. Download from equium.xyz/download (macOS/Windows/Linux)
2. Install and run
3. Set up an encrypted local wallet (Argon2id + AES-256-GCM)
4. Add your Helius RPC URL if desired
5. Start mining

**Best for:** User-friendly GUI and good for steady mining.

### Option 3: CLI Miner (Best for VPS/Servers)

This is the headless, efficient option.

#### Install Dependencies (Ubuntu/Debian example)

```bash
sudo apt-get update && sudo apt-get upgrade -y
sudo apt install screen curl build-essential pkg-config libssl-dev -y
```

#### Install Rust

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
```

#### Clone and Build the Miner

```bash
git clone https://github.com/HannaPrints/equium.git
cd equium
cargo build -p equium-cli-miner --release
```

The binary will be at `./target/release/equium-miner`.

#### Create/Fund a Solana Wallet (CLI)

```bash
solana-keygen new
# Save your seed phrase and public key (wallet address)
```

Export private key if needed (for import into other wallets):

```bash
solana config get  # Note the keypair path, usually ~/.config/solana/id.json
cat ~/.config/solana/id.json
```

Fund the wallet with a small amount of SOL on Solana mainnet.

#### Run the Miner

Use screen for background running:

```bash
screen -S equium
```

Then start mining:

```bash
./target/release/equium-miner \
  --rpc-url https://mainnet.helius-rpc.com/?api-key=YOUR_HELIUS_KEY \
  --keypair ~/.config/solana/id.json
```

- Omit `--max-blocks N` to run indefinitely
- Use `Ctrl+A+D` to detach the screen
- Re-attach: `screen -r equium`
- For help: `./target/release/equium-miner --help`

**Example output:**
```
round #42   reward 25 EQM   target 0x10ffff…
    try #3   ✓ MINED!   +25 EQM
    sig 4xZ9Ks…2pH8Yt
```

## Tips for Better Performance

- Use a good RPC (Helius free key recommended to avoid rate limits)
- More CPU cores generally improve your share of blocks (difficulty adjusts network-wide)
- Monitor hashrate in the logs (e.g., H/s)

## Useful Commands & Monitoring

- **Check balance:** Use Solscan or your wallet (search by mint address)
- **Stop miner:** `Ctrl+C` inside the screen session
- **List screens:** `screen -ls`
- **Kill screen:** `screen -XS equium quit`
- **Update miner:** Pull latest from repo and rebuild

## Troubleshooting

| Issue | Solution |
|-------|----------|
| **Rate limits** | Switch to a private RPC like Helius |
| **No blocks won** | Normal — it's probabilistic. Your earnings scale with CPU power and uptime |
| **Low hashrate** | Ensure you're not memory-constrained; close other apps |
| **Transaction failures** | Ensure wallet has SOL for fees |
| **General issues** | Check official docs at equium.xyz/docs for RPC setup, protocol details, etc. |

## Security & Best Practices

- Never share your seed phrase or private key
- Verify all downloads and the token mint address
- Use a dedicated wallet for mining
- For production, run on a VPS with good uptime and monitor temperatures/power
- Mining profitability is not guaranteed — treat it as an experiment

## Additional Resources

- **Official Site:** [equium.xyz](https://equium.xyz)
- **GitHub:** [HannaPrints/equium](https://github.com/HannaPrints/equium)
- **X (Twitter):** [@EquiumEQM](https://twitter.com/EquiumEQM)
- **Explorer:** [Solscan](https://solscan.io) or [Solana Explorer](https://explorer.solana.com) (use the program ID for on-chain info)
