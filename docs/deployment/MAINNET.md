# Ethereum Mainnet Deployment (PoidhV3 + PoidhClaimNFT)
## Prereqs
- Funded deployer EOA on Ethereum Mainnet
- `POIDH_TREASURY` address confirmed (immutable in `PoidhV3`)
- Deployment parameters confirmed (min bounty + min contribution)
- Etherscan API key for `--verify`
## Deploy
```bash
export MAINNET_RPC_URL="https://…"
export ETHERSCAN_API_KEY="…"
export DEPLOYER_PK="0x…"
export POIDH_TREASURY="0x293e70B7e1EC808358461f5d6247cdb877B6DeD2"
export POIDH_START_CLAIM_INDEX=1
# optional
export POIDH_NFT_NAME="poidh claims v3"
export POIDH_NFT_SYMBOL="POIDH3"
forge script script/deploy/Mainnet.s.sol:DeployMainnet \
  --rpc-url "$MAINNET_RPC_URL" \
  --private-key "$DEPLOYER_PK" \
  --broadcast \
  --verify \
  --etherscan-api-key "$ETHERSCAN_API_KEY"
```
## Post-deploy checks
Fill these from the script output:
- `POIDH_V3=0xE731dFadBFf20542E10D09D26Fc71445C70d4232`
- `POIDH_NFT=0x9c5F45D5e1382e4058D334d93C6c01442012a4D9`
```bash
cast call "$POIDH_V3" "treasury()(address)" --rpc-url "$MAINNET_RPC_URL"
cast call "$POIDH_V3" "poidhNft()(address)" --rpc-url "$MAINNET_RPC_URL"
cast call "$POIDH_NFT" "poidh()(address)" --rpc-url "$MAINNET_RPC_URL"
cast call "$POIDH_V3" "MIN_BOUNTY_AMOUNT()(uint256)" --rpc-url "$MAINNET_RPC_URL"
cast call "$POIDH_V3" "MIN_CONTRIBUTION()(uint256)" --rpc-url "$MAINNET_RPC_URL"
```
Expected:
- `PoidhV3.poidhNft()` == `POIDH_NFT`
- `PoidhClaimNFT.poidh()` == `POIDH_V3`
- `PoidhV3.treasury()` == `0x293e70B7e1EC808358461f5d6247cdb877B6DeD2`
- `PoidhV3.MIN_BOUNTY_AMOUNT()` == `0.001 ether`
- `PoidhV3.MIN_CONTRIBUTION()` == `0.00001 ether`
