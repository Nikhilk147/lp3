# Blockchain Technology (BT) - Group C

## Process to Deploy the code
Yes, testing and deploying smart contracts using the methods below is completely free and requires no real money.

**Method 1: The Easiest Way (Remix VM)**
You do not even need MetaMask or an internet connection to a blockchain for this method. It runs a simulated blockchain directly inside your browser.

* In the Remix IDE **Deploy & Run Transactions** tab, click the **Environment** dropdown.


* Select **Remix VM** (this was actually the setting you had selected in your screenshot).


* Remix will instantly generate several fake accounts for you, each pre-loaded with 100 fake ETH.


* Select one of these accounts, click **Deploy**, and test your `deposit`, `withdraw`, and `showBalance` functions instantly without waiting for network confirmations or paying real gas fees.



**Method 2: The Realistic Way (Testnets + MetaMask)**
The MetaMask method explained previously is also 100% free.

* When you switch MetaMask to a "Test Network" (like Sepolia), you use "Test ETH".
* Test ETH has no real-world monetary value. You get it for free by visiting websites called "Faucets" (e.g., Sepolia Faucet), entering your wallet address, and clicking "Send me ETH".
* You use this free Test ETH to pay the gas fees for deploying and testing your contract.





## Code Explanation

# Bank Account Smart Contract Breakdown

This Solidity code acts as a digital bank where users can store and manage their Ether. Here is a line-by-line breakdown of how the smart contract operates:

*   **Setup and Versioning** (`pragma solidity ^0.8.0;`): This line tells the compiler to use version 0.8.0 of Solidity (or a newer compatible version) to ensure the code runs securely.
*   **The Contract Container** (`contract BankAccount`): This is the blueprint for your bank, functioning similarly to a class in traditional programming languages like Python or Java.
*   **State Variables**:
    *   `address public owner;`: This stores the unique Ethereum address of the person who creates (deploys) the bank.
    *   `mapping(address => uint256) private balances;`: This acts as a digital ledger or dictionary. It links a specific user's Ethereum address (`address`) to their current account balance (`uint256`, which stands for an unsigned positive integer).
*   **Events** (`event Deposited`, `event Withdrawn`): These act like transaction receipts. When money is deposited or withdrawn, these events log the action permanently on the blockchain for external applications to read.
*   **The Constructor** (`constructor()`): This special function runs only once, exactly when the contract is first deployed. It sets the `owner` variable to `msg.sender`, which represents the address of the person who clicked deploy.
*   **Deposit Function** (`function deposit() public payable`):
    *   The `payable` keyword is crucial; it allows this specific function to accept incoming Ether.
    *   `require(msg.value > 0...);`: This acts as a security guard, ensuring the user actually sent a value greater than 0.
    *   `balances[msg.sender] += msg.value;`: This identifies the person calling the function (`msg.sender`) and adds the amount of Ether they sent (`msg.value`) to their specific balance in the ledger.
*   **Withdraw Function** (`function withdraw(uint256 amount) public`):
    *   It takes an `amount` parameter, which is the amount the user wants to take out.
    *   It uses `require` statements to check two things: the withdrawal amount is greater than 0, and the user's recorded balance is greater than or equal to the requested amount.
    *   `balances[msg.sender] -= amount;`: It deducts the amount from the internal ledger *before* sending the money to prevent hackers from trying to withdraw multiple times.
    *   `(bool ok, ) = payable(msg.sender).call{value: amount}("");`: This is the code that actually transfers the real Ether out of the contract and back into the user's wallet.
*   **Show Balance Function** (`function showBalance() public view returns (uint256)`):
    *   The `view` keyword tells the blockchain that this function only looks at data and does not modify anything, making it free to run without paying gas fees.
    *   It simply looks up `msg.sender` in the `balances` mapping and returns their current saved balance.
