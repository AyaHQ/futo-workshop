# [2024-05-11] Session

## Steps


### Installing Scoop

- Head over to scoop.sh and follow the quickstart guide or...

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
Invoke-RestMethod -Uri https://get.scoop.sh | Invoke-Expression # select A (yes to all)
```


### Installing Dependencies

```powershell
scoop bucket add extras

scoop install git vscode nodejs
```


### Setting up Git (SSH)

```powershell
cd ~
mkdir .ssh
cd .ssh
code config
```

Add the following to the `config` file located in `~/.ssh/config`
```
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/github.com
  IdentitiesOnly yes
```

You can use this command to generate your ssh keys

```powershell
# assuming you're in the ~/.ssh folder
ssh-keygen -t ed25519 -f github.com
```

Two new files should now be generated in the `~/.ssh` folder: 
- github.com (public key, safe to share)
- github.com.pub (private key, never share)


### Setting up a Hardhat Project

Create a folder in a path that's suitable for you and open up the folder in vscode. In the terminal, run the following command:

```powershell
npx hardhat init # and go through the prompt
# be sure to select 'typescript project' (the 2nd option)
```


### Smart Contract Source Code

You can check out the source conde to the smart contract developed in a previous session using the link below 👇

https://gist.github.com/Adophilus/3de064b43aca33c0c62d5a62633114b0

Copy and paste the contents of the solidity smart contract into `contracts/Restaurant.sol`. You would need to create the file because it won't exist initially.

Then run this command

```powershell
npm add @openzeppelin/contracts
```


### Compiling The Smart Contracts

```powershell
npx hardhat compile
```


### Setup Ignition Deployment Module

Copy and paste the following code into `ignition/modules/Restaurant.ts`

```typescript
import { buildModule } from "@nomicfoundation/hardhat-ignition/modules";

const RestaurantModule = buildModule("RestaurantModule", (m) => {
  const restaurant = m.contract("Restaurant");

  return { restaurant };
});

export default RestaurantModule;
```

### Deploying The Compiled Smart Contracts (On Localhost)

```powershell
npx hardhat ignition deploy ./ignitions/modules/Restaurant.ts # you can optionally supply --network <name> to deploy to a specific network
```


### Deploying The Compiled Smart Contract (On a Testnet)

Ensure that your `hardhat.config.ts` file looks something similar to this
```typescript
import { HardhatUserConfig } from "hardhat/config";
import "@nomicfoundation/hardhat-toolbox";

const sepoliaRpcUrl = "https://ethereum-sepolia-rpc.publicnode.com"
const sepoliaPrivateKey = process.env.SEPOLIA_PRIVATE_KEY as string

const config: HardhatUserConfig = {
  solidity: "0.8.24",
  networks: {
    sepolia: {
      url: sepoliaRpcUrl,
      accounts:[sepoliaPrivateKey] 
    }
  }
};

export default config;
```

Run the following command to deploy it to the sepolia testnet 

```powershell
npx hardhat ignition deploy ./ignitions/modules/Restaurant.ts --network sepolia
```


### Useful Links

- Block Explorer
  - EtherScan: https://sepolia.etherscan.io/
- Faucets:
  - Alchemy: https://www.alchemy.com/faucets/ethereum-sepolia
  - LearnWeb3: https://learnweb3.io/faucets/sepolia/
