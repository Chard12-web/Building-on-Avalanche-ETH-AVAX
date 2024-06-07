# Building on Avalanche ETH-AVAX

## Description

Avalanche ETH-AVAX is a bridge that connects the Ethereum network with the Avalanche network, enabling seamless asset transfers between the two ecosystems. 
This README provides an overview of the bridge's functionality and how to get started with it.

## Getting Started

To get started with the Avalanche ETH-AVAX bridge, you can follow these steps:

1. Set up a wallet that supports both Ethereum and Avalanche networks.
2. Connect your wallet to a decentralized exchange or liquidity pool that supports asset swapping between Ethereum and Avalanche.
3. Initiate asset transfers between Ethereum and Avalanche using the bridge interface provided by the decentralized exchange or liquidity pool.

## Executing Program (Remix - an online Solidity IDE)

To execute the provided Solidity code, you can use Remix, an online Solidity Integrated Development Environment (IDE). Follow these steps:

1. Go to the Remix website: [Remix IDE](https://remix.ethereum.org/).
2. Create a new Solidity file.
3. Copy and paste the provided Solidity code into the file.
4. Compile the Solidity code by clicking on the "Solidity Compiler" tab and selecting the appropriate compiler version.
5. Deploy the contract by clicking on the "Deploy & Run Transactions" tab.
6. Interact with the deployed contract by calling its functions.

## Code

```javascript
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract DegenToken is ERC20, Ownable {
  event TokensMinted(address indexed receiver, uint amount);
  enum SellItem {
    DEGEN_REFRIGERATOR,
    DEGEN_AIRCON,
    DEGEN_OVEN,
    DEGEN_MICROWAVE,
    DEGEN_RICECOOKER
  }
  mapping(SellItem => uint) public RichardShopPrices;

  constructor() ERC20("Richard", "RCHRD") Ownable(msg.sender) {
    _mint(msg.sender, 10000 * 10**decimals());

    RichardShopPrices[SellItem.DEGEN_REFRIGERATOR] = 2000;
    RichardShopPrices[SellItem.DEGEN_AIRCON] = 5000;
    RichardShopPrices[SellItem.DEGEN_OVEN] = 4000;
    RichardShopPrices[SellItem.DEGEN_MICROWAVE] = 3500;
    RichardShopPrices[SellItem.DEGEN_RICECOOKER] = 3000;
  }

  function mint(address _seller, uint _amount) public onlyOwner {
    _mint(_seller, _amount);
    emit TokensMinted(_seller, _amount);
  }

  function richardTokens(address _seller, uint _amount) public {
    _transfer(msg.sender, _seller, _amount);
  }

  function redeemTokens(SellItem _item) public {
    uint256 itemPrice = RichardShopPrices[_item];
    require(balanceOf(msg.sender) >= itemPrice, "You have insufficient balance, try again");

    _burn(msg.sender, itemPrice);
  }

  function degenTokenBalance(address _buyer) public view returns (uint) {
    return balanceOf(_buyer);
  }

  function burnTokens(uint _amount) public {
    require(balanceOf(msg.sender) >= _amount, "You have insufficient balance, try again");
    _burn(msg.sender, _amount);
  }
}
````
# Explanation:

Contract Inheritance: The contract DegenToken inherits from two other contracts: ERC20 and Ownable. ERC20 provides standard functionality for a fungible token, while Ownable ensures that only the contract owner can execute certain functions.

Events: The contract defines an event TokensMinted which is emitted when tokens are minted.

Enum: An enum SellItem is defined to represent different items that can be sold in a hypothetical shop. The items include DEGEN_REFRIGERATOR, DEGEN_AIRCON, DEGEN_OVEN, DEGEN_MICROWAVE, and DEGEN_RICECOOKER.

Mapping: The contract defines a mapping RichardShopPrices that maps each SellItem to its respective price in tokens.

Constructor: The constructor function is called when the contract is deployed. It mints an initial supply of tokens to the deployer and initializes the prices for each item in the shop.

## Functions:

mint: Allows the contract owner to mint tokens and transfer them to a specified address.
richardTokens: Allows anyone to transfer tokens from the contract owner's address to another address.
redeemTokens: Allows users to redeem an item from the shop by burning the required amount of tokens.
degenTokenBalance: Allows users to check their token balance.
burnTokens: Allows users to burn a specified amount of their tokens.

## Authors

Richard Pelarios BSIT 
@Chard-web

# License

Copyright (c) 2024 Chard-web


