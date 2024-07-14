https://solidity-by-example.org/hacks/signature-replay/

Signing msg off-chain and have the contract verify the signature before executing is quite common (for example, gasless transaction)

Make sure the signature also signs the `nonce` so that the transaction will not be replayed (executing more than one time)
