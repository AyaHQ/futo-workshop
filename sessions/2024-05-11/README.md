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


### Setting up a Hardhat Project

```powershell
npx hardhat init # and go through the prompt
```


### Compiling The Smart Contracts

```powershell
npx hardhat compile # and go through the prompt
```


### Deploying The Compiled Smart Contracts

```powershell
npx hardhat ignitions deploy ./ignitions/modules/*.ts # you can optionally supply --network <name> to deploy to a specific network
```

