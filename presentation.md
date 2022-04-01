---
marp: true
theme: uncover
class: invert
---

##### Web3 Show & Tell
### How I built a Safe App from scratch
#
#
#
##### Manuel Gellfart

![h:64](assets/github.png) &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; ![h:64](assets/twitter.png)
@schmanu | @schmanu_


---
### About me

- worked in web2 the past 8 years
- tiptoed into web3 one year ago via [**CSV Airdrop**](https://github.com/bh2smith/safe-airdrop) app
- joining the Gnosis Safe frontend team in May
#
#
#
---
### Token Approval Manager
---
### Idea of new Safe App

![w:300](assets/finding_problems.gif)
![w:300](assets/finding_problems4.gif)

---
### Idea of new Safe App

![center](assets/tweet.png)

---

### Tools and Libraries

![tools](assets/tools.gif)

---
### Tools and Libraries

- Interacting with Gnosis Safe
[safe-apps-react-sdk](https://github.com/gnosis/safe-apps-sdk/tree/master/packages/safe-apps-react-sdk) & [Transaction Service API](https://safe-transaction.gnosis.io/)
- User Interface
[safe-react-components](https://github.com/gnosis/safe-react-components) & [material-ui](https://github.com/mui/material-ui) & [styled-components](https://github.com/styled-components/styled-components)
- Interacting with smart contracts
[typechain](https://github.com/dethcrypto/TypeChain) & [@openzeppelin/contracts](https://github.com/OpenZeppelin/openzeppelin-contracts) & [ethers](https://www.npmjs.com/package/ethers)
---

### Getting Started

```bash
npx create-react-app my-app 
--template @gnosis.pm/cra-template-safe-app

cd my-app
yarn start
```

![center](assets/new-app.png)

---
### Loading the safe

```tsx
<SafeProvider loader={
  <>
    <Title size="md">Waiting for Safe...</Title>
    <Loader size="md" />
  </>
}>
  <Application />
</SafeProvider>
```

---

```tsx
const SafeApp = (): React.ReactElement => {
  const { safe } = useSafeAppsSDK();
  const { safeAddress, chainId, owners, threshold } = safe;
}
```

---
#### Creating transaction data
```bash
typechain --target=ethers-v5 --out-dir src/contracts 
'./node_modules/@openzeppelin/contracts/build/contracts/ERC20.json'
```

---
```tsx
export type ApprovalEdit = {
  tokenInfo: TokenInfo;
  spenderAddress: string;
  newValue: BigNumber;
};

export const createApprovals = (approvals: ApprovalEdit[]) => {
  const erc20Interface = ERC20__factory.createInterface();
  const txList = approvals.map((approval) => ({
    to: approval.tokenInfo.address,
    value: '0',
    data: erc20Interface.encodeFunctionData('approve', [
      approval.spenderAddress,
      toWei(approval.newValue, approval.tokenInfo.decimals).toFixed(),
    ]),
  }));
  return txList;
};
```
---
#### Submitting transactions (in batch)
```tsx
const { sdk } = useSafeAppsSDK();
const txs = createApprovals(approvalEntries);
const response = await sdk.txs.send({ txs }).catch(() => undefined);
if (response?.safeTxHash) {
    setSuccess(true);
}
```
---

### Token Approval Manager

https://github.com/schmanu/token-approval-tracker

---

![w:675](assets/prototype.gif)

---

### Summary

- Batch transactions are easy within safe apps
- Building a prototype is fast
- A review process is really helpful

---

# Questions?

![](assets/goodboy.gif)