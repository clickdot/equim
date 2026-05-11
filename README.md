# equim

[![GitHub stars](https://img.shields.io/github/stars/clickdot/equim?style=social)](https://github.com/clickdot/equim)

**equim miner CLI guide**

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

- A Solana-compatible wallet (e.g., [Phantom](https://phantom.app), [Backpack](https://www.backpack.app), or CLI keypair)
- Linux (Ubuntu recommended), macOS, or Windows (with WSL for CLI)
- For VPS/headless: A basic CPU VPS works well (more cores = higher chance of winning blocks)
- Small amount of SOL for fees (~0.001 SOL per successful block or less)
- **Recommended:** [Helius RPC API key](https://www.helius.dev) (free tier available) for reliable mining and better performance

## Mining Options

### Option 1: Easiest — Browser Miner (Quick Start)

1. Go to [equium.xyz/mine](https://equium.xyz/mine)
2. Use the built-in encrypted wallet or connect your own
3. Start mining directly in the browser

**Best for:** Testing. Not ideal for 24/7 operation.

### Option 2: Desktop App (Recommended for Most Users)

1. Download from [equium.xyz/download](https://equium.xyz/download) (macOS/Windows/Linux)
2. Install and run
3. Set up an encrypted local wallet (Argon2id + AES-256-GCM)
4. Add your [Helius RPC URL](https://www.helius.dev) if desired
5. Start mining

**Best for:** User-friendly GUI and good for steady mining.

### Option 3: CLI Miner (Best for VPS/Servers)

This is the headless, efficient option.

## Install Dependencies

### 1. Install Packages

```bash
sudo apt-get update && sudo apt-get upgrade -y
sudo apt install screen curl nano -y
```

### 2. Install Rust

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

When prompted, enter `1` and wait until installation is complete.

```bash
source $HOME/.cargo/env
```

### 3. Install Solana CLI

```bash
curl --proto '=https' --tlsv1.2 -sSfL https://solana-install.solana.workers.dev | bash
```

Close and reopen your Terminal, then verify the installation:

```bash
solana --version
```

If you get `solana: command not found`, run:

```bash
echo 'export PATH="/root/.local/share/solana/install/active_release/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
solana --version
```

### 4. Set Up Helius RPC (Recommended)

Get a free [Helius RPC API key](https://www.helius.dev) for better reliability and performance. Then configure it:

```bash
solana config set --url https://mainnet.helius-rpc.com/?api-key=YOUR_HELIUS_KEY
```

**Alternatively**, use the public RPC:

```bash
solana config set --url https://api.mainnet-beta.solana.com
```

> **Note:** Helius is recommended to avoid rate limiting during mining.

## Wallet CLI Setup

### 1. Create a CLI Wallet

```bash
solana-keygen new
```

- Set a password or skip by pressing Enter
- **Save your Seed-Phrase & Public-Key**
  - **Public-Key:** Is your node's Solana wallet address

### 2. Export Private Key

**Find Keypair path:**

```bash
solana config get
```

This gives your Keypair path like: `~/.config/solana/id.json`

**Export Private Key:**

```bash
cat ~/.config/solana/id.json
```

The exported array is your Private-Key. **Save it securely.**

### 3. Import and Fund Node Wallet

1. Import your Private-Key into your Solana wallet
2. Fund it with SOL on the Solana network

## Clone and Build the Miner

```bash
git clone https://github.com/HannaPrints/equium.git
cd equium
cargo build -p equium-cli-miner --release
```

The binary will be at `./target/release/equium-miner`.

## Run the Miner

Use screen for background running:

```bash
screen -S equium
```

Then start mining with your Helius API key:

```bash
./target/release/equium-miner \
  --rpc-url https://mainnet.helius-rpc.com/?api-key=YOUR_HELIUS_KEY \
  --keypair ~/.config/solana/id.json
```

**Screen Commands:**
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

- **Get a Helius API key:** Sign up for free at [Helius](https://www.helius.dev) to avoid rate limiting
- **Monitor hashrate:** Look for `H/s` (hashes per second) in your miner logs to track performance
- **More CPU cores:** Increase your chances of winning blocks with better hardware
- **Close unnecessary apps:** Ensure you're not memory-constrained; dedicate system resources to mining
- Difficulty adjusts network-wide, so your earnings scale with CPU power and uptime

## Useful Commands & Monitoring

### Monitor Mining Activity

Check live logs from your screen session:

```bash
screen -r equium
```

Watch for messages like:
- `MINED!` — You won a block (check logs for reward amount)
- `H/s` — Your current hashrate
- `round #N` — The current mining round

### Check Balance

Use [Solscan](https://solscan.io) or your wallet to check your EQM balance (search by mint address `1MhvZzEe8gQ8Rb9CrT3Dn26Gkn9QRErzLMGkkTwveqm`).

### Miner Management

- **Stop miner:** `Ctrl+C` inside the screen session
- **List screens:** `screen -ls`
- **Kill screen:** `screen -XS equium quit`

### Update the Miner

Pull the latest version from the repository and rebuild:

```bash
cd equium
git pull
cargo build -p equium-cli-miner --release
```

Then restart your miner with the new binary.

## Troubleshooting

| Issue | Solution |
|-------|----------|
| **Rate limits** | Get a free [Helius API key](https://www.helius.dev) and use it in your miner command |
| **No blocks won** | Normal — it's probabilistic. Your earnings scale with CPU power and uptime |
| **Low hashrate** | Ensure you're not memory-constrained; close other apps and check CPU usage |
| **Transaction failures** | Ensure wallet has enough SOL for transaction fees (~0.001 SOL per block) |
| **solana: command not found** | Add Solana to PATH: `echo 'export PATH="/root/.local/share/solana/install/active_release/bin:$PATH"' >> ~/.bashrc && source ~/.bashrc` |
| **RPC connection errors** | Switch to [Helius RPC](https://www.helius.dev) or verify your RPC URL |
| **General issues** | Check official docs at [equium.xyz/docs](https://equium.xyz/docs) for RPC setup, protocol details, etc. |

## Security & Best Practices

- Never share your seed phrase or private key
- Verify all downloads and the token mint address
- Use a dedicated wallet for mining
- For production, run on a VPS with good uptime and monitor temperatures/power
- Mining profitability is not guaranteed — treat it as an experiment
- Store private keys securely and use strong passwords

## Additional Resources

- **Official Site:** [equium.xyz](https://equium.xyz)
- **Documentation:** [equium.xyz/docs](https://equium.xyz/docs)
- **GitHub:** [HannaPrints/equium](https://github.com/HannaPrints/equium)
- **X (Twitter):** [@EquiumEQM](https://twitter.com/EquiumEQM)
- **Explorer:** [Solscan](https://solscan.io) or [Solana Explorer](https://explorer.solana.com) (use the program ID for on-chain info)
- **RPC Provider:** [Helius](https://www.helius.dev) (free tier available)
