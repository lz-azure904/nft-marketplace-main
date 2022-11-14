# Prerequisties
- Visual Code or Cloud9
- Install [Remote Access Extension for Visual Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)
- [Install AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)
- Configure AWS Cli
```
//check your email for your access key id and value
aws configure 
```
- Download this Repo

# Install ganache-cli and truffle
- open a new terminal on visual studio code
- cd to nft-marketpalce-main if it is not there
- npm install -g ganache-cli truffle
  - install ganache-cli and truffle; run the ganache local testnet @8545 
- run the following cmd to start the local testnet and save the auto-generated 10 eth-accounts into the file ethaccounts.json with the public/private keys; each account has 100 test ethers 
```
ganache-cli --acctKeys .\ShareToWinRestApi\ethaccounts.json 
```
- Below is showing the HD Wallet you will see after running gananche-cli cmd; Mnmonic is the scretes for the wallet.
```
HD Wallet
==================
Mnemonic:      poverty put today valve idle accident blast easily frown dismiss wear obey
Base HD Path:  m/44'/60'/0'/0/{account_index}
Listening on 127.0.0.1:8545
```
- install MetaMask browser extension
- record the acct password
- import ganache-cli created accts' (via private keys) if you want to see the ethers in MetaMask wallet
  - ensure "testnet" configuration is enabled for the MetaMask setting
  - each time ganache-cli restarts it will create new accounts so the new accts have to be re-imported into your MetaMask if you want to have them in your wallet 

# Smart Contract
- open a new terminal on visual studio code 
- cd to ShareTowinContract
- run truffle init
- check truffle-config.js for the local ganache-cli testnet on 8545
```
networks: {
    development: {
      host: "127.0.0.1",     // Localhost (default: none)
      port: 8545,            // Standard Ethereum port (default: none)
      network_id: "*",       // Any network (default: none)
    },
```
- install openzeppelin - the open source smart contract repo
```
npm install @openzeppelin/contracts
```
- deploy smart contract
  - there are two smart contracts in the contract folder
    - Migrations.sol is a default smart contract from truffle
    - AssetToken.sol is the smart contract for the project
```
// this cmd is to start the truffle console and connect to the network named "development" defined in truffle-config.js; once the console is ready run "compile" and "migrate" to compile and deploy the smart contracts to the "development" ethereum network
truffle console --network development
> compile
> migrate
```
//console will print out the details of deployed smart contracts
- "AssetToken" 
  - contract addrss: 0x60B107B51025f3Ad432D7f9DF34F88c67e4C67C3 
  - account: 0x2178C08137D89064a8feeF624461F7525399ac5a
- "Migration"
  - contract address: 0xB7E8f4AD4Bd7eEce1Ba6F680d1a12D250B2b1469
  - account: 0x2178C08137D89064a8feeF624461F7525399ac5a  
# ShareToWinApi
- open a new terminal on visual studio code 
- cd to ShareTowinRestApi
- might need:
  - npm install express
- update CONTRACTADDRESS field with the previous step's "AssetToken" contract address which will be different from what is shown above in envExport.sh 
- copy AssetToken.json from build\contracts to ShareToWinApi
- run 
```
//update the env variable
source envExport.sh 
//start the express server and watch any changes
npx nodemon --delay 1000ms index.js 
```

# ShareToWinWeb
- open a new terminal on visual studio code 
- cd to ShareTowinWeb
- might need:
  - npm install react-scripts
  - might need change the port if port 3000 is in use
  - need create dynamo db if there is no such dynamodb table - in our case there is a Dynamodb table created already; but here is the cmd to create the dynamo table if required
  ```
  aws dynamodb create-table --table-name ShareToWin --attribute-definitions AttributeName=AssetID,AttributeType=N --key-schema AttributeName=AssetID,KeyType=HASH --provisioned-throughput ReadCapacityUnits=1,WriteCapacityUnits=1
  {
    "TableDescription": {
        "AttributeDefinitions": [
            {
                "AttributeName": "AssetID",
                "AttributeType": "N"
            }
        ],
        "TableName": "ShareToWin",
        "KeySchema": [
            {
                "AttributeName": "AssetID",
                "KeyType": "HASH"
            }
        ],
        "TableStatus": "CREATING",
        "CreationDateTime": "2022-11-12T13:04:18.348000-06:00",
        "ProvisionedThroughput": {
            "NumberOfDecreasesToday": 0,
            "ReadCapacityUnits": 1,
            "WriteCapacityUnits": 1
        },
        "TableSizeBytes": 0,
        "ItemCount": 0,
        "TableArn": "arn:aws:dynamodb:us-east-1:020614850103:table/ShareToWin",
        "TableId": "a2383266-6eed-413f-bd76-868926c6de0e"
    }
  }
  ```
  - start the react front end
  ```
  npm start or npm run start
  ```
  
  # UI
  Buyer 1 view
  ![image](https://user-images.githubusercontent.com/73182196/201503856-030faec4-3917-4d08-bf3c-64c3cb518d81.png)
  Buyer 2 view
  ![image](https://user-images.githubusercontent.com/73182196/201503886-6b6f2ff3-b7b5-4ae0-bb6a-79464819e5da.png)
  Admin view
  ![image](https://user-images.githubusercontent.com/73182196/201503772-386acdbe-e303-4d87-937b-ab41e55e40d1.png)


