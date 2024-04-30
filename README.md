# Provably Random Raffle Contract

## Description

This is a Solidity smart contract for creating a provably random raffle using Chainlink VRF (Verifiable Random Function) and Chainlink Automation. The contract allows users to enter the raffle by sending ETH, and after a specified time interval, a winner is randomly selected using Chainlink VRF. The winner's address is then paid out the entire contract balance.

## Features

- Users can enter the raffle by sending ETH to the `enterRaffle` function.
- The raffle automatically selects a winner after a specified time interval using Chainlink VRF.
- The winner's address is randomly selected from the list of participants.
- The entire contract balance is transferred to the winner's address.
- Implements error handling and event emissions for better monitoring and debugging.

## Requirements

- Solidity version 0.8.18 or compatible
- Access to a Chainlink VRF Coordinator contract and a funded subscription for requesting random numbers
- LINK tokens for paying the Chainlink VRF fee

## Usage

1. Deploy the contract with the required constructor parameters:
   - `entranceFee`: The amount of ETH required to enter the raffle.
   - `interval`: The time interval (in seconds) between raffle runs.
   - `vrfCoordinator`: The address of the Chainlink VRF Coordinator contract.
   - `gasLane`: The gas lane for the Chainlink VRF subscription.
   - `subscriptionId`: The ID of the Chainlink VRF subscription.
   - `callbackGasLimit`: The gas limit for the Chainlink VRF callback function.

2. Fund the contract with LINK tokens for paying the Chainlink VRF fee.

3. Users can enter the raffle by calling the `enterRaffle` function and sending the required `entranceFee` amount of ETH.

4. After the specified `interval` has passed, the Chainlink Automation nodes will call the `checkUpkeep` and `performUpkeep` functions to initiate the raffle winner selection process.

5. The `fulfillRandomWords` function is called by the Chainlink VRF Coordinator with the requested random number, which is used to select the winner from the list of participants.

6. The winner's address is stored in the `s_recentWinner` state variable, and the entire contract balance is transferred to the winner's address.

## Instructions for First-Time Users

### Hardhat

1. Clone the repository:
```
git clone https://github.com/your-username/raffle-contract.git
```

2. Install the required dependencies:
```
npm install
```

3. Compile the contract:
```
npx hardhat compile
```

4. Deploy the contract to a local or testnet environment:
```
npx hardhat run scripts/deploy.js --network
```

Replace `<network>` with the desired network (e.g., `localhost`, `goerli`).

5. Interact with the deployed contract using the provided scripts or by writing your own scripts.

6. To run tests:
```
npx hardhat test
```

### Foundry

1. Clone the repository:
```
git clone https://github.com/your-username/raffle-contract.git
```

2. Install the required dependencies:
```
forge install
```

3. Compile the contract:
```
forge build
```

4. Deploy the contract to a local or testnet environment:
```
forge create --constructor-args --private-key --rpc-url src/Raffle.sol:Raffle
```

Replace `<args>` with the required constructor arguments, `<your-private-key>` with your Ethereum account private key, and `<rpc-url>` with the RPC URL of the desired network.

5. Interact with the deployed contract using the provided scripts or by writing your own scripts.

6. To run tests:
```
forge test
```

## Functions

- `constructor`: Initializes the contract with the required parameters.
- `enterRaffle`: Allows users to enter the raffle by sending ETH.
- `checkUpkeep`: Checks if the conditions for running the raffle are met (time interval passed, raffle is open, contract has balance, and participants).
- `performUpkeep`: Requests a random number from the Chainlink VRF Coordinator and initiates the winner selection process.
- `fulfillRandomWords`: Callback function called by the Chainlink VRF Coordinator with the requested random number, selects the winner, and transfers the contract balance to the winner's address.
- Getter functions: `getEntranceFee`, `getRaffleState`, `getPlayer`, `getRecentWinner`, `getLengthOfPlayers`, `getLastTimeStamp`.

## Events

- `EnteredRaffle`: Emitted when a user enters the raffle.
- `PickedWinner`: Emitted when a winner is selected.
- `RequestedRaffleWinner`: Emitted when a random number is requested from the Chainlink VRF Coordinator.

## Errors

- `Raffle__NotEnoughETHSent`: Thrown when a user tries to enter the raffle without sending the required `entranceFee` amount of ETH.
- `Raffle__TransferFailed`: Thrown when the transfer of the contract balance to the winner's address fails.
- `Raffle__RaffleNotOpen`: Thrown when a user tries to enter the raffle while it is not in the open state.
- `Raffle__UpkeepNotNeeded`: Thrown when the conditions for running the raffle are not met during the `performUpkeep` function call.

## License

This project is licensed under the [MIT License](LICENSE).
