# Stork Oracle Network Setup Guide

This guide provides a complete walkthrough for setting up and running a Stork Oracle node with an automated validation bot.

## Prerequisites

- Chrome browser
- Ubuntu VM or WSL (Windows Subsystem for Linux)
- Node.js 14.0.0 or higher

## Step 1: Browser Extension Setup

1. Install the Chrome extension from: [Stork Verify](https://chromewebstore.google.com/detail/stork-verify/knnliglhgkmlblppdejchidfihjnockl)
2. Sign up for an account
   - You can use referral code: `J34RHYQC80` during signup
3. Confirm your email address
4. Start your node by pressing the power button in the extension

## Step 2: Set Up the Automated Validation Bot

The validation bot will help automate the verification process to earn rewards through the Stork Oracle system.

### Features

- Automatically fetches signed price data from Stork Oracle API
- Validates price data according to predefined rules
- Submits validation results back to the API
- Handles token refresh for continuous operation
- Displays validation statistics and user information
- Configurable validation interval
- Support for proxy servers to distribute requests
- Multi-threaded processing for improved performance

### Bot Installation

This is a forked repository from the original [airdropinsiders/Stork-Auto-Bot](https://github.com/airdropinsiders/Stork-Auto-Bot).

1. Clone the repository:
   ```bash
   git clone https://github.com/airdropinsiders/Stork-Auto-Bot.git
   ```

2. Navigate to the project directory:
   ```bash
   cd Stork-Auto-Bot
   ```

3. Install dependencies:
   ```bash
   npm install
   ```

### Bot Configuration

The bot uses a `config.json` file for configuration which will be generated on first run.

1. Run the bot once to generate the default config file:
   ```bash
   node index.js
   ```

2. Edit the generated `config.json` with nano:
   ```bash
   nano config.json
   ```

3. Update the file with your credentials:
   ```json
   {
     "cognito": {
       "region": "ap-northeast-1",
       "clientId": "5msns4n49hmg3dftp2tp1t2iuh",
       "userPoolId": "ap-northeast-1_M22I44OpC",
       "username": "your-email@example.com",
       "password": "your-password"
     },
     "stork": {
       "intervalSeconds": 5
     },
     "threads": {
       "maxWorkers": 1
     }
   }
   ```

4. Replace `username` and `password` with your Stork Oracle account credentials
5. Save and exit by pressing `CTRL+X`, then `Y`, then `Enter`

### Optional: Proxy Configuration

To use proxy servers for distributing requests:

1. Create a `proxies.txt` file:
   ```bash
   nano proxies.txt
   ```

2. Add one proxy per line in any of these formats:
   - HTTP proxies: `http://user:pass@host:port`
   - SOCKS proxies: `socks5://user:pass@host:port`

3. Save and exit by pressing `CTRL+X`, then `Y`, then `Enter`

## Step 3: Running the Bot

Start the automated validation bot:

```bash
node index.js
```

The bot will:
1. Authenticate using your credentials from `config.json`
2. Fetch signed price data at regular intervals
3. Validate each data point
4. Submit validation results to Stork Oracle
5. Display your current statistics

## Step 4: Setting Up Automatic Restart (Optional)

To ensure your node automatically runs after VM restart, set up a tmux session:

1. Create an auto-start script:
   ```bash
   nano start-stork.sh
   ```

2. Add the following content:
   ```bash
   #!/bin/bash
   
   # Get the server's IP address
   SERVER_IP=$(hostname -I | awk '{print $1}')
   echo "Server IP: $SERVER_IP"
   
   # Start a new tmux session and run the bot
   cd ~/Stork-Auto-Bot
   tmux new-session -d -s stork-session "node index.js"
   
   echo "Stork Oracle bot started in tmux session"
   echo "To attach to the session, use: tmux attach -t stork-session"
   ```

3. Make the script executable:
   ```bash
   chmod +x start-stork.sh
   ```

4. Add to startup using crontab:
   ```bash
   crontab -e
   ```

5. Add this line to the crontab:
   ```
   @reboot ~/start-stork.sh >> ~/stork-startup.log 2>&1
   ```

6. Save and exit

## Troubleshooting

- **Authentication errors**: Check that your username and password in `config.json` are correct
- **Bot fails to start**: Ensure your `config.json` file is properly formatted JSON
- **Token-related errors**: The `tokens.json` file may be corrupted - delete it and let the bot regenerate it
- **Connection issues**: Check your internet connection and verify the Stork Oracle API is accessible
- **Proxy issues**: Verify your `proxies.txt` is properly formatted and proxies are operational

## Advanced Configuration

In your `config.json` file, you can adjust:
- `stork.intervalSeconds`: How often the validation process runs in seconds (default: 5)
- `threads.maxWorkers`: Number of concurrent validation workers (default: 1)

## View Running Bot

To view your running bot:
```bash
tmux attach -t stork-session
```

To detach from the session without stopping it, press `CTRL+B` followed by `D`.

## Disclaimer

This bot is provided for educational purposes only. Use at your own risk. The authors are not responsible for any consequences that may arise from using this bot, including but not limited to account termination or loss of rewards.
