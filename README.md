# Prerequisties
- Visual Code or Cloud9
- Install [Remote Access Extension for Visual Code](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.vscode-remote-extensionpack)
- [Install AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/installing.html)
- Configure AWS Cli
```
aws configure //check your email for your access key id and value
```
- Download this Repo

# Install ganache-cli and truffle
- open a new terminal on visual studio code
- cd to nft-marketpalce-main if it is not there
- npm install -g ganache-cli truffle
  - install ganache-cli and truffle; run the ganache local testnet @8545 
- run ganache-cli --acctKeys .\ShareToWinRestApi\ethaccounts.json (this is to run the local testnet) to display the accounts: (note: I have to run ganache-cli twice to get the private keys; the first time it only shows 5 public keys)
```
HD Wallet
==================
Mnemonic:      poverty put today valve idle accident blast easily frown dismiss wear obey
Base HD Path:  m/44'/60'/0'/0/{account_index}
Listening on 127.0.0.1:8545
```
- install MetaMask browser extension
- record the acct password
- import ganache-cli created accts' (via private keys)
  - ensure "testnet" configuration is enabled for the MetaMask setting
  - each time ganache-cli restarts the accounts have to be re-imported 

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
- install openzeppelin
```
npm install @openzeppelin/contracts
```
- deploy smart contract
```
truffle console --network development
> compile
> migrate
```
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
- update CONTRACTADDRESS field in envExport.sh 
- copy AssetToken.json from build\contracts to 
- run 
```
source envExport.sh //update the env variable
npx nodemon --delay 1000ms index.js //start the express server and watch any changes
```

# ShareToWinWeb
- open a new terminal on visual studio code 
- cd to ShareTowinWeb
- might need:
  - npm install react-scripts
  - might need change the port if 3000 is in use
  - need create dynmao db
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
  npm start/npm run start
  ```
  
  # UI
  Buyer 1 view
  ![image](https://user-images.githubusercontent.com/73182196/201503856-030faec4-3917-4d08-bf3c-64c3cb518d81.png)
  Buyer 2 view
  ![image](https://user-images.githubusercontent.com/73182196/201503886-6b6f2ff3-b7b5-4ae0-bb6a-79464819e5da.png)
  Admin view
  ![image](https://user-images.githubusercontent.com/73182196/201503772-386acdbe-e303-4d87-937b-ab41e55e40d1.png)


