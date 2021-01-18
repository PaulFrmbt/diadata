Here is my work for the bounty : https://gitcoin.co/issue/diadata-org/diadata/358/100024592

-- « MakerDAO’s Oasis lending rate » :

This is the DAI saving rate in the MakerDAO language
Smart contract that handles it : Pot
Documentation : https://docs.makerdao.com/smart-contract-modules/rates-module/pot-detailed-documentation
Github : https://github.com/makerdao/dss/blob/master/src/pot.sol
EtherScan : https://etherscan.io/address/0x19c0976f590d67707e62397c87829d896dc0f1f1#readContract

The per-second (or "instantaneous") savings rate is stored in the dsr parameter
Nowadays, dsr = 1000000000000000000000000000  which is interpreted as a ray, i.e. a 27 decimal digit fixed-point number. Thus dsr = 1
Which means that the annual saving rate = dsr^(number of seconds in a year) = 1.000000000000000000000000000^(365*24*3600) = 1
Thus the Annual Percentage Rate = 0% 
This figure might seem a little bit weird but it is explained here : https://ethereumprice.org/guides/article/dai-savings-rate-explained/


-- « MakerDAO’s Oasis borrowing rate for the different tokens » :

This is the Stability Fee in the MakerDAO language
Smart contract that handles it : Jug
Documentation : https://docs.makerdao.com/smart-contract-modules/rates-module/jug-detailed-documentation
Github : https://github.com/makerdao/dss/blob/master/src/jug.sol
EtherScan : https://etherscan.io/address/0x197e90f9fad81970ba7976f33cbd77088e5d7cf7#readContract

The per-second (or "instantaneous") stability fee is equal to base + duty.
To query the duty for the ilk named ETH-A, BAT-A, USDC-A, … we have to convert his name do bytes32
To convert you can use in your terminal : npx bytes32 ETH-A

Eg : Collateral Type = Ilk = bytes32(`ETH-A`) = 0x4554482d41000000000000000000000000000000000000000000000000000000
Nowadays base = 0 and duty = 1.000000000782997609082909351 for ETH-A
Which means that the annual saving rate = (base+duty)^(number of seconds in a year) = 1.000000000782997609082909351^(365*24*3600) = 1.02500000000000
Thus the annual stability fee for ETH-A is 2.5% as expected


-- « Locked value in MakerDAO for the different tokens » :

  - If you are looking for the amount of DAI debt from the collateral token :
  
This is the Total debt of a collateral in the MakerDAO language
Smart contract that handles it : Vat
Documentation : https://docs.makerdao.com/smart-contract-modules/core-module/vat-detailed-documentation
Github : https://github.com/makerdao/dss/blob/master/src/vat.sol
EtherScan : https://etherscan.io/address/0x35d1b3f3d7966a1dfe207aa4514c12a259a0492b#readContract

The Total debt (in Dai) is equal to ilk.Art * ilk.rate

For example, for ETH-A :
- ilks[bytes32(« ETH-A »)].Art= 685254637152972920186653993. This number is a wad so we need to divide it by 10^18 in our case : 685254637.152972920186653993
- ilks[bytes32(« ETH-A »)].Art= 1026584496485323087826827579. This number is a ray so we need to divide it by 10^27 in our case : 1.026584496485323087826827579

In the end, we have : 685254637.152972920186653993 * 1.026584496485323087826827579 = 703471786


  - If you are looking for the number of a collateral token locked in the protocol :
  
Let's take an example with BAT-A
You need to go to the ERC-20 contract of the token in our example : https://etherscan.io/address/0x0D8775F648430679A709E98d2b0Cb6250d2887EF#readContract
Then you need to have the address of the join smart contract of the token of Maker : here is a great list
https://github.com/nanexcool/daistats/blob/master/src/addresses.json : for example we have BAT :  "MCD_JOIN_BAT_A": "0x3D0B1912B66114d4096F48A8CEe3A56C231772cA"
you will then need to call balanceOf(0x3D0B1912B66114d4096F48A8CEe3A56C231772cA) to have the amount of tokens locked in the protocol here :  30,239,717.683305264361221428 (which is the value announced)

-- Additional ressources that you may find useful :

- https://makerburn.com/#/chart presents nice charts however it uses an API : https://uj17ktmqrh.execute-api.us-east-2.amazonaws.com/prod/status that is not directly on the blockchain and thus can’t be trusted forever. However I find the same results for every test I have made.

- https://mkr.tools/ but they telle there is no official API for multi-collateral DAI

