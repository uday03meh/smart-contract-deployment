# Smart Contract Deployment Guide

Open a new folder in your text editor

Create another new folder inside that folder

 ```sh 
 mkdir samplefolder
 ```

 ```sh 
 cd samplefolder
  ```
  
   ```sh 
   npm init
   ```
   
   Answer (y) to all the questions asked
   
   Now you wil have a package.json file in your "samplefolder"
   
   Run this to install hardhat to our project, hardhat provides a developent environment for testing and deploying decentralised applications.
   
   ```sh
   npm install --save-dev hardhat
   ```
  
  ```sh
  npx hardhat
  ```

Select create an empty hardhat.config.js file option using your arrow keys.

This will generate a hardhat.config.js file for us, which is where we’ll specify all of the set up for our project

Create two folders "contracts" and "scripts" in the root directory of our project using

```sh
mkdir contracts
```

```sh
mkdir scripts
```

``contracts/`` is where we’ll keep our hello world smart contract code file

``scripts/`` is where we’ll keep scripts to deploy and interact with our contract

Create a new file inside our ``contracts`` folder with a .sol extension to write our smart contract say SampleContract.sol

Create a dotenv file in the root directory of our project to store our API Keys and Private Keys

```sh
npm install dotenv --save
```

And now create a ``.env`` file in the root directory of our project, Your ``.env`` should look like this:

```sh
ALCHEMY_API_KEY = "https://eth-goerli.alchemyapi.io/v2/your-api-key"
PRIVATE_KEY = "your-metamask-private-key"
```

Go to your metamask wallet, Select an account and then copy it's Private Key and paste it in ``.env`` file

Go to [Alchemy](https://www.alchemy.com/) and create a new app using Ethereum chain and Goerli Network, click on view keys and copy the API Key and paste it in ``.env`` file

Run this command to install ``Ethers.js`` for interacting with our smart contract ``SampleContract.sol`` file and read data from the blockchain.

Update your ``hardhat.config.js`` to this for updating our project with the dependencies we have added

```JavaScript
/**
* @type import('hardhat/config').HardhatUserConfig
*/

require('dotenv').config();
require("@nomiclabs/hardhat-ethers");

const { ALCHEMY_API_KEY, PRIVATE_KEY } = process.env;

module.exports = {
   solidity: "0.8.9",
   defaultNetwork: "goerli",
   networks: {
      hardhat: {},
      goerli: {
         url: ALCHEMY_API_KEY,
         accounts: [`0x${PRIVATE_KEY}`]
      }
   },
}
```

Run this commmand to compile your smart contract

```sh
npx hardhat compile
```

This will create a new folder ``artifacts`` in our project repository and in ``contracts`` folder of this folder, we will get a ``SampleContract.js" file which contains contract's ABI and Bytecode.

Go to ``scripts`` folder and create a new file ``deploy.js`` and paste this code in that file.

```JavaScript
async function main() {
   const SampleContract = await ethers.getContractFactory("SampleContract");

   // Start deployment, returning a promise that resolves to a contract object
   const sampleContract = await HelloWorld.deploy();
   await sampleContract.wait()
   console.log("Contract deployed to address:", sampleContract.address);
}

main()
  .then(() => process.exit(0))
  .catch(error => {
    console.error(error);
    process.exit(1);
  });
```

Finally to deploy your contract, run:

```sh
npx hardhat run scripts/deploy.js --network goerli
```
