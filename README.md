 // fallback function can be used to buy tokens
    function () inActivePeriod payable {
        buyTokens(msg.sender);
    }


    function buyTokens(address _sender) inActivePeriod payable {
        require(_sender != 0x0);
        require(msg.value >= minTransactionAmount);

        uint256 weiAmount = msg.value;

        raisedForEther = raisedForEther.add(weiAmount);

        // calculate token amount to be created
        uint256 tokens = weiAmount.mul(rate);
        tokens += getBonus(tokens);

        tokenReserve(_sender, tokens);

        forwardFunds();
    }

    // send ether to the fund collection wallet
    // override to create custom fund forwarding mechanisms
    function forwardFunds() internal {
        wallet.transfer(msg.value);
    }t
