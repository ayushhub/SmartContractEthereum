Setting up private Ethereum node on MacOS

FYI Helpful Commands> https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console

Step 1 - Create a private working directory

mkdir -p ~/ChainSkills/private
cd ~/ChainSkills/private

Step 2 - Define a private Geth instance - Genesis Block (First Block in the Blockchain)

Use puppeth to ensure you setup the right version of the Genesis Block

![DASHBOARD](https://github.com/ayushhub/SmartContractEthereum/blob/master/screenshots/setting%20up%20genesis%20block%20via%20puppeth.png)

![DASHBOARD](https://github.com/ayushhub/SmartContractEthereum/blob/master/screenshots/ChainSkills-JSON.png)

Step 3 - Initialize the private instance. Create the node and pass the directory it will store its data

geth --datadir ~/ChainSkills/private init ChainSkills.json

This will create two directories
geth - blockchain instance data
keystore - all the accounts

![DASHBOARD](https://github.com/ayushhub/SmartContractEthereum/blob/master/screenshots/types%20of%20network%20IDs.png)

Step 4 - Prepare for mining and usage. Create 3 accounts

First account (do same thrice)
Private keys will be stored under the /keystore directory
geth --datadir . account new

enter password "pass1234"

![DASHBOARD](https://github.com/ayushhub/SmartContractEthereum/blob/master/screenshots/prepared%203%20instances.png)

To list the accounts in the order Use
geth --datadir . account list

Step 5 - Start the private node. Move the script to the /private directory startnode.sh. Make it executable

echo "pass1234" > /Users/ayushkumar/ChainSkills/private/password.sec
chmod +x startnode.sh
./startnode.sh
After epoch 0...1 it will start mining

![DASHBOARD](https://github.com/ayushhub/SmartContractEthereum/blob/master/screenshots/running%20startnode-sh.png)

![DASHBOARD](https://github.com/ayushhub/SmartContractEthereum/blob/master/screenshots/mining%20first%20block.png)

This is mining not just transactions but also generating ethers. This is why it will keep on generating new blocks
at regular intervals of times creating new quantities of ethers sent to the coinbase account.

Step 6 - Open new Terminal to connect to the above running node

![DASHBOARD](https://github.com/ayushhub/SmartContractEthereum/blob/master/screenshots/geth%20attach.png)

geth attach (will refer to the mining library)

Match the coinbase account and the data directory

> eth.accounts
["0x89f4f73ddb2e49f54366a0726952fedbe56ca1c0", "0x9b101773ac46d9dcdee2090bd49711af1559df28", "0xdae6bfff40141bc53250c45230445121105ccae0"]

> eth.coinbase
"0x89f4f73ddb2e49f54366a0726952fedbe56ca1c0"

> eth.getBalance(eth.accounts[1])
0

> eth.getBalance(eth.accounts[0]) -- this is receiving the mining reward represents the weight
345000000000000000000

Step 7 - Convert above to ether

> web3.fromWei(eth.getBalance(eth.coinbase), "ether")
378

> miner.stop()
true

INFO [03-11|13:45:01] Commit new mining work                   number=127 txs=0 uncles=0 elapsed=584.138µs

> miner.start(2) -- start on 2 threads
null

> net.version -- network identifier
"4224"

Unlock an account - make the private keys available for signing transactions. this is required
else the signing will require passphrase. Default is 10 minutes

personal.unlockAccount(eth.accounts[1], "pass1234",300) -- for 5 mins or 300 seconds

Step 8 - Transfer Ethers from the coinbase account "0" to the other two accounts

![DASHBOARD](https://github.com/ayushhub/SmartContractEthereum/blob/master/screenshots/Transfering%20the%20ethers.png)

eth.sendTransaction({from: eth.accounts[0], to:eth.accounts[1], value:web3.toWei(10, "ether")})

![DASHBOARD](https://github.com/ayushhub/SmartContractEthereum/blob/master/screenshots/mined%20transaction%20for%20transfer%20of%20ethers.png)

> web3.fromWei(eth.getBalance(eth.accounts[1]), "ether")
10

> web3.fromWei(eth.getBalance(eth.accounts[2]), "ether")
0

![DASHBOARD](https://github.com/ayushhub/SmartContractEthereum/blob/master/screenshots/Shutdown%20with%20control%20C.png)
