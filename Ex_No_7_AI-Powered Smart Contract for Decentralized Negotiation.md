# Experiment 7: AI-Powered Smart Contract for Decentralized Negotiation

# Aim:

To create a smart contract that integrates AI logic for automated negotiation in decentralized commerce. The contract adjusts price and conditions dynamically based on real-time market trends using an on-chain AI model.

# Algorithm:

Step 1: Item Listing: Seller lists an item with base, minimum, and maximum prices.

Step 2: Offer Submission: Buyer submits an offer along with ETH matching the offer price.

Step 3: Offer Validation: Smart contract checks if the item is still available and the payment is correct.

Step 4: AI Evaluation: dynamicPricing() simulates AI to calculate a counteroffer based on offer vs. price range.

Step 5: Decision Making: If the buyer's offer meets or exceeds the AI’s counteroffer, the sale is confirmed.

Step 6: Transaction Finalization: Seller receives payment, buyer receives confirmation, and item is marked sold.

Step 7: Counteroffer Emission: If the offer is too low, funds are refunded and a counteroffer is emitted as an event.


# Program:

### Developed by: DHARSHAN PT
### Register number: 212223230046

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract AIPoweredNegotiation {
    struct Item {
        address seller;
        uint256 minPrice;
        uint256 maxPrice;
        uint256 basePrice;
        bool sold;
    }

    mapping(uint256 => Item) public items;
    uint256 public itemCount;

    event ItemListed(uint256 itemId, uint256 basePrice);
    event OfferMade(uint256 itemId, address buyer, uint256 offer);
    event CounterOffer(uint256 itemId, uint256 counterOffer);
    event Sold(uint256 itemId, address buyer, uint256 finalPrice);

    function listItem(uint256 _basePrice, uint256 _minPrice, uint256 _maxPrice) public {
        require(_minPrice <= _basePrice && _basePrice <= _maxPrice, "Invalid price range");
        
        items[itemCount] = Item(msg.sender, _minPrice, _maxPrice, _basePrice, false);
        emit ItemListed(itemCount, _basePrice);
        itemCount++;
    }

    function makeOffer(uint256 _itemId, uint256 _offerPrice) public payable {
        Item storage item = items[_itemId];
        require(!item.sold, "Item already sold");
        require(msg.value == _offerPrice, "Incorrect offer amount");

        emit OfferMade(_itemId, msg.sender, _offerPrice);

        uint256 aiCounterOffer = dynamicPricing(item.basePrice, item.minPrice, item.maxPrice, _offerPrice);

        if (_offerPrice >= aiCounterOffer) {
            item.sold = true;
            payable(item.seller).transfer(_offerPrice);
            emit Sold(_itemId, msg.sender, _offerPrice);
        } else {
            payable(msg.sender).transfer(_offerPrice); // Refund buyer
            emit CounterOffer(_itemId, aiCounterOffer);
        }
    }

    function dynamicPricing(uint256 base, uint256 min, uint256 max, uint256 offer) private pure returns (uint256) {
        if (offer >= max) return max;
        if (offer >= base) return base;
        return (base + offer) / 2; // Simple AI-based counteroffer logic
    }
}

```

# Expected Output:

### Buyers submit offers, and the contract auto-negotiates the price.
![Screenshot 2025-05-05 133412](https://github.com/user-attachments/assets/bc8e21c0-7e7d-4f02-993d-c91e27d8abfa)

![Screenshot 2025-05-05 133600](https://github.com/user-attachments/assets/f42c2110-e44f-4511-94b3-b51897e39f03)



### If the buyer’s offer is fair, the deal is executed.
![Screenshot 2025-05-05 134853](https://github.com/user-attachments/assets/1b6a7feb-e8cc-4f38-a8ad-50b221fbe434)



### If the offer is too low, the contract suggests a counteroffer.

![Screenshot 2025-05-05 134048](https://github.com/user-attachments/assets/1abf303f-911a-4a10-aa6e-355665e665e8)



# High-Level Overview:

First-of-its-kind AI-powered pricing contract.


Mimics real-world price negotiations using dynamic on-chain pricing.


Can be extended to AI oracles for real-time market data.


Inspired by AI-enhanced commerce and eBay-like decentralized auctions.

# RESULT:

Thus, to create a smart contract that integrates AI logic for automated negotiation in decentralized commerce. The contract adjusts price and conditions dynamically based on real-time market trends using an on-chain AI model is executed successfully.



