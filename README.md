# Lelantos Smart Contract
A Blockchain-based Anonymous Physical Delivery System
## Test Case
### Deployment Lelantos smart contract Using [Remix-Solidity IDE](https://remix.ethereum.org/#version=soljsonv0.4.11+commit.68ef5810.js&optimize=true)
* Remix-Solidity IDE  is a browser-based Solidity compiler and IDE. It also creates private blockchian for testing purpose.
* Upload Lelantos smart contract to Remix-Solidity IDE
* From the right side of Remix-Solidity IDE, select "Contract". You will find textbox to enter _**max_hop_fee**_  ![](https://raw.githubusercontent.com/mhgharieb/test/master/images/deploy.png)

     Assuming that _**max_hop_fee**_ is 1 ETH (= 1000000000000000000 Wei)

     _**Deployment Input**_: `"1000000000000000000"`

   After Deployment step (Pressing _Create_), we can get the address of Lelantos Smart Contract L<sub>sc</sub> on the blockchain

### Scenario
A customer *C* wants to purchase a product (product ID `123456789`) from a merchant *M*. The *price* of the product is `2 ETH`. The customer wants to receive the product through **3** delivery companies.

#### Customer *App<sub>c<sub>* Calculation

* Choose 3 random tracking number *tn<sub>i<sub>* and calculate thier corresponding SHA3()
   
|i|*tn<sub>i<sub>*       |*trackingComm* = SHA3(*tn<sub>i<sub>*)|
|-|----------|----------|
|1|4174566344|`0x380687b0191d9315c524d1e1df1162bc752a05d19bb9c2ed1fe4ec2f464cd396`|
|2|2921603003|`0x4280d5baa22abc41a7d89e8460b69abcdda662e4a4984b5d9ff8063602e4a80a`|
|3|159340662|`0x089fd6732cb8459da7f217c37545e8d60294e778cb5c7b7d347add0455667750`|

* Choose 3 delivary companies, get their long term public key _pk<sub>i</sub>_ and caluclate _**c<sub>i</sub>**_ based on the equation  
_**c<sub>i</sub> = Enc<sub>pk<sub>i</sub></sub>(tn<sub>i</sub> , L<sub>sc</sub> , add<sub>next<sub>dc</sub></sub> ⊕ mask<sub>i</sub>)**_ where
     * add<sub>next<sub>dc</sub></sub>: the address of delivary company _i+1_ on route
     * mask<sub>i</sub>: random masking value

|i|mask<sub>i</sub>|c<sub>i</sub>|
|-|-----------|--------|
|1|`0x2897da2f52e3e517169e9485c67d5420c05096a3ef3616d21248c1418bd4f0152bf5639f43cf9e7fa7a49c0b1f6b0bd2431f863b3b3b0863bc3daf4cf2574337`|`0x91efe942812585d3d921015f2c8bfb7f2e0287ebe8a74d7a86d65b92b2e122b3a7af8a907b5593d32aaa720c0d47c7e0249e7c02bf22c248be3ddd66d16892aa91efe942812585d3d921015f2c8bfb7f2e0287ebe8a74d7a86d65b92b2e122b3`|
|2|`0x42d8d6b282371ea4bc081585733c794d3d365800627e831a5c6077a91afbfef1b074a570fa9b71c335b1442d371abde026dea6fbf7c9be064516b786e18e642b`|`0xe786c64ee96c4568235a615a1a0dd6f0a7eaa790e8030cc091a57b0271a47830a26b68404722430639a98bd51a9a72cd2727fa6900bcfe7f3cde6e477c8e8505e786c64ee96c4568235a615a1a0dd6f0a7eaa790e8030cc091a57b0271a47830`|
|3|`0xf8364a7f0e0b468f43c068e8c65a0bd82a080ec1d2e947e7a6d15fd32bbbc9f5abbb187fd7a0510079447e14cfde1c4c5dba2cc7ce9313c7a0bbdc8eda8a3b3d`|`0xefaa4e7851ce2dfbb04c59bb699e8c7c721156218b05e6f217adf24cb5edc171a04afb73361464192823bcd71aa8512f623cca70670cfe15f1b36d4be22a2622efaa4e7851ce2dfbb04c59bb699e8c7c721156218b05e6f217adf24cb5edc171`|

* Choose a _**secret**_ and calcuate its corresponding _**commitment**_ = SHA3(_**secret**_)

   _**secret**_ = `MySecret12345678`
   
   _**commitment**_ = `0x81fc8bce1439eeee7515cbd9973c49dfb50bef47e3c0284cc8c5ab5ca867bd5c`
 
* Calculate _**mc<sub>0</sub>**_ which is the result of encrypting the address of the first delivary company probabilistically by the public key of the merchant _**pk<sub>m</sub>**_
```
0x9c22ff5f21f0b81b113e63f7db6da94fedef11b2119b4088b89664fb9a3cb658956d747595e5eb44e1992fb86ec4944ce9d71f74ddb3e0387101a293b07dd86aa4815d8ef41e10e84c7eeb5015ce8afde3a8fbff54682b26837a840c5ff6439d
```

### Test Using [Remix-Solidity IDE](https://remix.ethereum.org/#version=soljsonv0.4.11+commit.68ef5810.js&optimize=true)

Remix-Solidity IDE offers 5 diffrent addresses. We will use one for customer, one for merchant, and three for delivary companies (dc1, dc2, dc3)

```
customer = '0xca35b7d915458ef540ade6068dfe2f44e8fa733c'
merchant = '0x14723a09acff6d2a60dcdf7aa4aff308fddc160c'
dc1 = '0x4b0897b0513fdc7c541b6d9d7e929c4e5364d2db'
dc2 = '0x583031d1113ad414f02576bd6afabfb302140225'
dc3 = '0xdd870fa1b7c4700f2bd7f44238821c26f7392148'
```

**1. Create**
* _From Address_: `customer`
* _Value(ETH)_: `0`
* _Input Format_:`(bytes32 _commitment, bytes32[] _trackingComm)`
   * `_commitment`: _**commitment**_
   * `_trackingComm`: Array of _**trackingComm**_ from 1 to 3 
```
["0x81","0xfc","0x8b","0xce","0x14","0x39","0xee","0xee","0x75","0x15","0xcb","0xd9","0x97","0x3c","0x49","0xdf","0xb5","0x0b","0xef","0x47","0xe3","0xc0","0x28","0x4c","0xc8","0xc5","0xab","0x5c","0xa8","0x67","0xbd","0x5c"],["0x380687b0191d9315c524d1e1df1162bc752a05d19bb9c2ed1fe4ec2f464cd396","0x4280d5baa22abc41a7d89e8460b69abcdda662e4a4984b5d9ff8063602e4a80a","0x089fd6732cb8459da7f217c37545e8d60294e778cb5c7b7d347add0455667750"]
```

**2. Order**
* _From Address_: `customer`
* _Value(ETH)_: `price + 3 * max_hop_fee = 5`
* _Input Format_:`(address _merchant, uint _pID, uint _price, bytes label0, bytes _nextLabel, uint validityTime)`
   * `_merchant`: Merchant Address
   * `_pID`: product ID
   * `_price`: price in Wei
   * `label0`: mc<sub>0</sub>
   * `_nextLabel`: c<sub>1</sub>
   * `validityTime`: Validity Time of the order in Second (Ex: 1 Hour = 3600 Seconds)
```
"0x14723a09acff6d2a60dcdf7aa4aff308fddc160c",123456789,"2000000000000000000",["0x9c","0x22","0xff","0x5f","0x21","0xf0","0xb8","0x1b","0x11","0x3e","0x63","0xf7","0xdb","0x6d","0xa9","0x4f","0xed","0xef","0x11","0xb2","0x11","0x9b","0x40","0x88","0xb8","0x96","0x64","0xfb","0x9a","0x3c","0xb6","0x58","0x95","0x6d","0x74","0x75","0x95","0xe5","0xeb","0x44","0xe1","0x99","0x2f","0xb8","0x6e","0xc4","0x94","0x4c","0xe9","0xd7","0x1f","0x74","0xdd","0xb3","0xe0","0x38","0x71","0x01","0xa2","0x93","0xb0","0x7d","0xd8","0x6a","0xa4","0x81","0x5d","0x8e","0xf4","0x1e","0x10","0xe8","0x4c","0x7e","0xeb","0x50","0x15","0xce","0x8a","0xfd","0xe3","0xa8","0xfb","0xff","0x54","0x68","0x2b","0x26","0x83","0x7a","0x84","0x0c","0x5f","0xf6","0x43","0x9d"],["0x91","0xef","0xe9","0x42","0x81","0x25","0x85","0xd3","0xd9","0x21","0x01","0x5f","0x2c","0x8b","0xfb","0x7f","0x2e","0x02","0x87","0xeb","0xe8","0xa7","0x4d","0x7a","0x86","0xd6","0x5b","0x92","0xb2","0xe1","0x22","0xb3","0xa7","0xaf","0x8a","0x90","0x7b","0x55","0x93","0xd3","0x2a","0xaa","0x72","0x0c","0x0d","0x47","0xc7","0xe0","0x24","0x9e","0x7c","0x02","0xbf","0x22","0xc2","0x48","0xbe","0x3d","0xdd","0x66","0xd1","0x68","0x92","0xaa","0x91","0xef","0xe9","0x42","0x81","0x25","0x85","0xd3","0xd9","0x21","0x01","0x5f","0x2c","0x8b","0xfb","0x7f","0x2e","0x02","0x87","0xeb","0xe8","0xa7","0x4d","0x7a","0x86","0xd6","0x5b","0x92","0xb2","0xe1","0x22","0xb3"],3600
```

**3. Accept**
* _From Address_: `merchant`
* _Value(ETH)_: `0`
* _Input Format_:`(uint _pID)`
   * `_pID`: product ID
```
123456789
```

**4. Receive 1**
* _From Address_: `dc1`
* _Value(ETH)_: `0`
* _Input Format_:`(bytes trackingNum, uint _fee)`
   * `trackingNum`: tn<sub>1</sub> in hexadecimal
   * `_fee`: Fee of delivary com. 1 (0.2 ETH) in Wei 
```
["0xf8","0xd2","0xd3","0xc8"],"200000000000000000"
```

**5. Next 1**
* _From Address_: `customer`
* _Value(ETH)_: `0`
* _Input Format_:`(bytes _mask, bytes _nextLabel)`
   * `_mask`: Mask<sub>1</sub>
   * `_nextLabel`: c<sub>2</sub>
```
["0x28","0x97","0xda","0x2f","0x52","0xe3","0xe5","0x17","0x16","0x9e","0x94","0x85","0xc6","0x7d","0x54","0x20","0xc0","0x50","0x96","0xa3","0xef","0x36","0x16","0xd2","0x12","0x48","0xc1","0x41","0x8b","0xd4","0xf0","0x15","0x2b","0xf5","0x63","0x9f","0x43","0xcf","0x9e","0x7f","0xa7","0xa4","0x9c","0x0b","0x1f","0x6b","0x0b","0xd2","0x43","0x1f","0x86","0x3b","0x3b","0x3b","0x08","0x63","0xbc","0x3d","0xaf","0x4c","0xf2","0x57","0x43","0x37"],["0xe7","0x86","0xc6","0x4e","0xe9","0x6c","0x45","0x68","0x23","0x5a","0x61","0x5a","0x1a","0x0d","0xd6","0xf0","0xa7","0xea","0xa7","0x90","0xe8","0x03","0x0c","0xc0","0x91","0xa5","0x7b","0x02","0x71","0xa4","0x78","0x30","0xa2","0x6b","0x68","0x40","0x47","0x22","0x43","0x06","0x39","0xa9","0x8b","0xd5","0x1a","0x9a","0x72","0xcd","0x27","0x27","0xfa","0x69","0x00","0xbc","0xfe","0x7f","0x3c","0xde","0x6e","0x47","0x7c","0x8e","0x85","0x05","0xe7","0x86","0xc6","0x4e","0xe9","0x6c","0x45","0x68","0x23","0x5a","0x61","0x5a","0x1a","0x0d","0xd6","0xf0","0xa7","0xea","0xa7","0x90","0xe8","0x03","0x0c","0xc0","0x91","0xa5","0x7b","0x02","0x71","0xa4","0x78","0x30"]
```

**6. Receive 2**
* _From Address_: `dc2`
* _Value(ETH)_: `0`
* _Input Format_:`(bytes trackingNum, uint _fee)`
   * `trackingNum`: tn<sub>2</sub> in hexadecimal
   * `_fee`: Fee of delivary com. 2 (0.3 ETH) in Wei 
```
["0xae","0x24","0x1f","0xbb"],"300000000000000000"
```

**7. Next 2**
* _From Address_: `customer`
* _Value(ETH)_: `0`
* _Input Format_:`(bytes _mask, bytes _nextLabel)`
   * `_mask`: Mask<sub>2</sub>
   * `_nextLabel`: c<sub>3</sub>
```
["0x42","0xd8","0xd6","0xb2","0x82","0x37","0x1e","0xa4","0xbc","0x08","0x15","0x85","0x73","0x3c","0x79","0x4d","0x3d","0x36","0x58","0x00","0x62","0x7e","0x83","0x1a","0x5c","0x60","0x77","0xa9","0x1a","0xfb","0xfe","0xf1","0xb0","0x74","0xa5","0x70","0xfa","0x9b","0x71","0xc3","0x35","0xb1","0x44","0x2d","0x37","0x1a","0xbd","0xe0","0x26","0xde","0xa6","0xfb","0xf7","0xc9","0xbe","0x06","0x45","0x16","0xb7","0x86","0xe1","0x8e","0x64","0x2b"],["0xef","0xaa","0x4e","0x78","0x51","0xce","0x2d","0xfb","0xb0","0x4c","0x59","0xbb","0x69","0x9e","0x8c","0x7c","0x72","0x11","0x56","0x21","0x8b","0x05","0xe6","0xf2","0x17","0xad","0xf2","0x4c","0xb5","0xed","0xc1","0x71","0xa0","0x4a","0xfb","0x73","0x36","0x14","0x64","0x19","0x28","0x23","0xbc","0xd7","0x1a","0xa8","0x51","0x2f","0x62","0x3c","0xca","0x70","0x67","0x0c","0xfe","0x15","0xf1","0xb3","0x6d","0x4b","0xe2","0x2a","0x26","0x22","0xef","0xaa","0x4e","0x78","0x51","0xce","0x2d","0xfb","0xb0","0x4c","0x59","0xbb","0x69","0x9e","0x8c","0x7c","0x72","0x11","0x56","0x21","0x8b","0x05","0xe6","0xf2","0x17","0xad","0xf2","0x4c","0xb5","0xed","0xc1","0x71"]
```

**8. Receive 3**
* _From Address_: `dc3`
* _Value(ETH)_: `0`
* _Input Format_:`(bytes trackingNum, uint _fee)`
   * `trackingNum`: tn<sub>3</sub> in hexadecimal
   * `_fee`: Fee of delivary com. 3 (0.4 ETH) in Wei 
```
["0x09","0x7f","0x58","0x76"],"400000000000000000"
```

**9. Pickup**
* _From Address_: `dc3`
* _Value(ETH)_: `0`
* _Input Format_:`(bytes secret)`
   * `secret`: _**secret**_
```
"MySecret12345678"
```
