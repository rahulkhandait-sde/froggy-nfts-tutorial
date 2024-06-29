# 🐸 FroggyNFTs Tutorial 🐸

Welcome to the **FroggyNFTs Tutorial** repository! This tutorial will guide you through building a frog-themed decentralized application (DApp) on the Linea blockchain.

## 📖 Project Overview

In this tutorial, you'll learn how to:
- Set up your development environment 🛠️
- Write smart contracts for frog-themed NFTs 🐸
- Integrate smart contracts with a React frontend ⚛️
- Deploy your DApp on the Linea blockchain 🌐

## 🛠️ Installation

### Prerequisites
Before you start, make sure you have the following installed:
- [Node.js](https://nodejs.org/)
- [Truffle](https://www.trufflesuite.com/truffle)
- [Metamask](https://metamask.io/) browser extension

### Setup Instructions

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd froggy-nfts-tutorial
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Compile and migrate smart contracts:**
   ```bash
   truffle compile
   truffle migrate --network linea
   ```

4. **Start the frontend:**
   ```bash
   npm start
   ```

5. **Connect Metamask to Linea testnet:**
   - Import test accounts and ensure you're connected to the Linea testnet.

## 🚀 Usage

- Open your browser and navigate to [http://localhost:3000](http://localhost:3000).
- Use the DApp to mint new frog NFTs, trade them on the marketplace, and explore various features.

## 📜 Smart Contract

Here's a simple smart contract for the FroggyNFTs:

```solidity
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";

contract FrogNFT is ERC721 {
    constructor() ERC721("FroggyNFT", "FRG") {}

    function mint(address to, uint256 tokenId) public {
        _safeMint(to, tokenId);
    }
}
```

## 🌐 Frontend Integration

The React frontend interacts with the smart contract:

```javascript
import React, { useState, useEffect } from "react";
import Web3 from "web3";
import FrogNFTContract from "./contracts/FrogNFT.json";

function App() {
  const [web3, setWeb3] = useState(null);
  const [accounts, setAccounts] = useState([]);
  const [contract, setContract] = useState(null);

  useEffect(() => {
    async function init() {
      if (window.ethereum) {
        const web3Instance = new Web3(window.ethereum);
        setWeb3(web3Instance);

        try {
          await window.ethereum.request({ method: "eth_requestAccounts" });
          const accs = await web3Instance.eth.getAccounts();
          setAccounts(accs);

          const networkId = await web3Instance.eth.net.getId();
          const deployedNetwork = FrogNFTContract.networks[networkId];
          const instance = new web3Instance.eth.Contract(
            FrogNFTContract.abi,
            deployedNetwork && deployedNetwork.address
          );
          setContract(instance);
        } catch (error) {
          console.error("Error connecting to MetaMask:", error);
        }
      } else {
        console.error("MetaMask not detected!");
      }
    }

    init();
  }, []);

  async function mintNFT() {
    const tokenId = Math.floor(Math.random() * 1000000);
    try {
      await contract.methods.mint(accounts[0], tokenId).send({ from: accounts[0] });
      console.log("NFT minted successfully!");
    } catch (error) {
      console.error("Error minting NFT:", error);
    }
  }

  return (
    <div className="App">
      <header className="App-header">
        <h1>FroggyNFTs DApp</h1>
        <button onClick={mintNFT}>Mint NFT</button>
      </header>
    </div>
  );
}

export default App;
```

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🎉 Congratulations!

🎉 You've successfully built and deployed a frog-themed DApp on Linea! This tutorial covered setting up your development environment, writing and deploying smart contracts, and integrating them with a React frontend. Happy coding! 🐸✨
