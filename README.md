# Create An Ethereum Dapp with Ethersjs

 This is a step-by-step tutorial on how to create a front end, deploy a Solidity smart contract, and connect them together.
 We will use [Metamask](https://metamask.io), [Remix IDE](https://remix.ethereum.org) and [Ethersjs](https://github.com/ethers-io/ethers.js/). 
 
 By the end of this tutorial you will be able to create a simple HTML front end with buttons that can interact with smart contract functions. The tutorial takes place in 3 stages
 
 - Create a basic html web page
 - Create a basic solidity smart contract
 - Connect the web page with the smart contracts using Ethersjs.  
 
### Preparation
 
 - Download and Install [MetaMask](https://metamask.io)
   - Never used Metamask? Watch [this explainer video](https://youtu.be/wlm4QcA8c4Q?t=66)
    - (the important bits at the moment are in the video from 1:06 to 4:14)
   - Change to the Ropsten Tesnet:
<p align="middle">
<img src="https://btcgeek.com/wp-content/uploads/2018/11/Metamask-Ropsten-Network-1a.png" alt="Metamask-Ropsten-Network-1a.png" width="200">
</p>
   - Copy an account's wallet public address
   - Request some Ropsten Tesnet Ether from a *faucet* loaded into your Metamask Wallet. 
   - Never done that? Watch [this explainer video](https://youtu.be/wlm4QcA8c4Q?t=66)
   - [Faucet blog explaining how to use it](https://blog.b9lab.com/when-we-first-built-our-faucet-we-deployed-it-on-the-morden-testnet-70bfbf4e317e)
   - [faucet link to request funds](https://ipfs.io/ipfs/QmVAwVKys271P5EQyEfVSxm7BJDKWt42A2gHvNmxLjZMps/)
   
- Install a http server. Use any you like, but we recommend a simple python instance for beginners:
  - Install NPM ([Download and Instructions](https://www.npmjs.com/)) 
  - Install python (version 3) (with NPM in a terminal/command prompt): 
    ```bash
    npm install python -g
    ```
 
 
### Create a basic Front End

The first step is to create a basic html page.
  
 - Create a new directory in your terminal using `mkdir <directory name>`
 - Create a new file called `index.html` by : `touch index.html`
 - In your favourite code editor, open the file (ex: `atom index.html' )
 - Open <html> and <body> tags inside the html page and add some buttons.
 
 --- 
 ```html
<!DOCTYPE html>
<html>
<head>
<title>Ethersjs Demo</title>
<style>
body {
    background-color: black;
    text-align: center;
    color: white;
    font-family: Arial, Helvetica, sans-serif;
}
</style>
</head>
<body>

<h1>This is my Ethersjs Demo!</h1>
<p> Here we can set or get the mood: </p>

<button onclick="getMood()"> get Mood </button>
<button onclick = "setMood()"> set Mood</button>

</body>
</html>
```
 ---
 - Serve the webpage via terminal/command prompt from the directory that has `index.html` in it: 
   ```bash
   python -m http.server 1337
   ```
 - Go to [http://127.0.0.1:1337/](http://127.0.0.1:1337/) in your browser to see your page!

### Create a basic Smart Contract

 - Create a smart contract on [remix.ethereum.org]
 - You can use your own smart contract with a minimum of a set and get function, something like this:
 
```solidity
pragma solidity ^0.4.24;

contract MoodDiary{
    
    string mood;
    
    function setMood(string _mood) public{
        mood = _mood;
    }
    
    function getMood() public view returns(string){
        return mood;
    }
}
```
 - deploy the contract on the Ropsten Testnet. 
 - test it on the Remix "Run" tab to make sure it works


### Connect Front End to Smart Contract

- import the ethers source into your html page

```html
<script charset="utf-8"
        src="https://cdn.ethers.io/scripts/ethers-v4.min.js"
        type="text/javascript">
</script>
```

- Inside script tags, add the following code to your html page.

- define a new ethers provider

```javascript
var provider = new ethers.providers.Web3Provider(web3.currentProvider,'rinkeby');
```

- Import the ABI and contract address, and create a contract object and a signer
```javascript
  var MoodContractAddress = "<contract address>";
  var MoodContractABI = <contract ABI>
  var MoodContract
  var signer
```
- Connect the signer to your metamask account, and define the contract object using your contract address, ABI, and signer.
```javascript
provider.listAccounts().then(function(accounts) {
      signer = provider.getSigner(accounts[0]);
      MoodContract = new ethers.Contract(MoodContractAddress, MoodContractABI, signer);
    })
```

- create asynchronous functions to call your smart contract functions

```javascript
  async function getMood(){
    getMoodPromise = MoodContract.getMood();
    var Mood = await getMoodPromise;
    console.log(Mood);
  }

  async function setMood(){
    setMoodPromise = MoodContract.setMood("patient");
    await setMoodPromise;
  }
```

- connect your functions to your html buttons

```html

<button onclick="getMood()"> get Mood </button>
<button onclick = "setMood()"> set Mood</button>
```



### Test it

- See if it works!
 
 
 


 
 
