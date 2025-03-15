ChainChat
A multimodal interface for managing personal finance portfolios on the blockchain, making complex financial products and blockchain interactions more accessible through natural language processing and voice commands.

Overview
This project simplifies blockchain-based personal finance management by providing an intuitive interface powered by AI agents. Users can interact through voice or text to manage their portfolio, learn about financial concepts, and execute blockchain transactions seamlessly.

Features
🎙 Multimodal Interface

Voice commands via OpenAI's GPT-4o realtime model
Text-based interaction for traditional interface users
Natural language processing for financial queries
🤖 Dual Agent Architecture

GPT-4o realtime for voice interpretation and user interaction
Coinbase AgentKit integration for blockchain operations
Parallel agent processing for handling different command types
🔗 Blockchain Integration

Base-Sepolia network integration for efficient transactions
Low-cost transaction processing
Automated portfolio management on-chain
Smart contract interaction capabilities
🔐 Secure Authentication & Wallet Management

Privy integration for streamlined wallet creation
Embedded wallets for user authentication
Secure transaction signing with Privy server wallets
Automated wallet mapping and management
Tech Stack
Next.js application
Privy integration for wallet management
Voice and text input processing
Coinbase Agentkit for blockchain operations
OpenAI GPT-4o realtime model for audio interpretation
Base-Sepolia network integration
Optional Gaia node for local processing
Agent Communication
The system employs a dual-agent architecture where:

GPT-4o realtime agent handles user interactions and voice processing
Coinbase Development Platform (CDP) agent monitors for blockchain-related context
Agents operate in parallel to process user requests
Smart Contract Architecture
Portfolio Contract
The core smart contract enables users to:

Deposit and withdraw ETH
Deposit and withdraw ERC20 tokens
Swap between supported tokens using Uniswap V3
Track portfolio value using Chainlink price feeds
Manage token approvals and price feed updates
Supported Tokens
WETH (0x4200000000000000000000000000000000000006)
USDC (0x036cBD53842C5426634e7929541eC2018491CF43)
cbETH (0x7c6b91D9Be155A6Db01f749217d76fF02A7227F2)
Core Contract Features
ETH and Token Management
function depositEth() external payable
function withdrawEth(uint256 amount) external nonReentrant
function depositToken(address token, uint256 amount) external nonReentrant
function withdrawToken(address token, uint256 amount) external nonReentrant
Portfolio Management
function swapExactInputSingle(
    address tokenIn,
    address tokenOut,
    uint256 amountIn,
    uint256 amountOutMinimum
) external nonReentrant returns (uint256 amountOut)
function getPortfolioValue(address user) external view returns (uint256 totalValueUsd)
Security Features
ReentrancyGuard implementation
Owner-controlled price feed updates
Balance and slippage protection
Price feed staleness checks
Secure token transfer handling
Prerequisites
Node.js 16+
PostgreSQL database
Privy API Key
Coinbase CDP API Key
Environment Setup
Create a .env.local file in your project root:

# Privy
PRIVY_API_KEY=your_privy_api_key

# Database
DATABASE_URL="postgresql://user:password@localhost:5432/dbname"

# Coinbase CDP
NEXT_PUBLIC_CDP_API_KEY=your_cdp_api_key

# General
NEXT_PUBLIC_ENABLE_TESTNETS=true
NEXT_PUBLIC_ALCHEMY_API_KEY=your_alchemy_api_key
Installation
Install dependencies:
npm install
Initialize Prisma:
npx prisma init
npx prisma generate
npx prisma db push
Project Structure
├── components/
│   ├── WalletAgent.tsx       # AI agent component
│   └── WalletHandler.tsx     # Wallet management component
├── contracts/
│   └── Portfolio.sol         # Main portfolio smart contract
├── lib/
│   ├── privy.ts             # Privy server wallet utilities
│   ├── wallet.ts            # Wallet management utilities
│   └── services/
│       └── walletService.ts # Wallet mapping service
├── pages/
│   ├── api/
│   │   └── wallet-actions.ts # API routes for wallet actions
│   └── index.tsx            # Main page
└── prisma/
    └── schema.prisma        # Database schema
Key Components
1. Privy Server Wallet Integration
import { PrivyClient } from '@privy-io/server-sdk';

export const PrivyWalletService = {
  async createWallet(userId: string) {
    const privy = new PrivyClient({
      apiKey: process.env.PRIVY_API_KEY!,
    });
    return privy.createWallet(userId);
  }
  // ... other wallet operations
};
2. Wallet Mapping
// Database schema for wallet mapping
model User {
  id                    String   @id @default(uuid())
  embeddedWalletAddress String   @unique
  serverWalletAddress   String   @unique
  createdAt             DateTime @default(now())
}
Usage Examples
Create a Server Wallet
const wallet = await PrivyWalletService.createWallet(userId);
Send a Transaction
const tx = await PrivyWalletService.sendTransaction(userId, {
  to: recipientAddress,
  value: ethers.utils.parseEther("0.1").toString()
});
Check Balance
const balance = await PrivyWalletService.getBalance(userId);
Example Commands for the AI Agent
The AI agent can process natural language commands like:

"Send 0.5 ETH to 0x..."
"Check my wallet balance"
"Deploy a new smart contract"
"Swap tokens on Uniswap"
Security Considerations
Smart Contract Security

Reentrancy protection
Balance verification
Price feed validation
Slippage protection
Application Security

Secure API key storage
Private key protection
Rate limiting
Transaction limits
User authentication verification
Development
Run the development server:

npm run dev
Build for production:

npm run build
Testing
npm run test
Contributing
Fork the repository
Create your feature branch
Commit your changes
Push to the branch
Create a new Pull Request
