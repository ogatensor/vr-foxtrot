## Introduction 

Here are the key points from the introduction of the Inverse Finance FiRM audit report:

- The audit was conducted to provide an independent assessment of the FiRM protocol's smart contracts, including security, code quality, and functionality.

- The FiRM protocol is a DeFi protocol that enables users to access fixed-rate loans using various assets as collateral.

- The protocol is built around the concept of Markets, where users can deposit collateral and borrow the DOLA stablecoin.

- Each Market has a unique escrow contract for each user, allowing them to participate in governance and other protocol interactions.

- The codebase is well-documented and easy to comprehend.

- However, there are instances where additional safety measures could be implemented to enhance the code's resilience.

- Many best practices are not employed, such as reentrancy guards and "safe" token function calls.

- As a result, a significant amount of responsibility is delegated to the governance process to prevent the introduction of new vulnerabilities.

## Summary 
Here is a summary of the key points from the Inverse Finance FiRM audit report:

1. The codebase is well-documented and easy to comprehend, but there are several instances where additional safety measures could be implemented to enhance the code's resilience.

2. The report identified several potential reentrancy vulnerabilities in the [[ConvexCurveEscrow]], [[Market.withdraw]], and [[INVEscrow]] functions that could allow an attacker to withdraw more collateral than allowed.

3. The report found that the [[Oracle.getFeedPrice]] function only checks that the returned price is greater than zero, but does not check for stale prices, which could lead to issues.

4. The [[DbrDistributor.onlyINVEscrow]] modifier was found to be too loose, potentially allowing escrows from unintended markets to call restricted functions.

5. The report identified several minor issues such as typos, missing error messages, missing events, and opportunities for optimization through reduced storage reads.

6. Overall, the report recommends implementing reentrancy guards, improving price feed validation, tightening access controls, and addressing other code quality and security enhancements to improve the protocol's resilience.
## Hit Functions 
The following functions were mentioned in the audit report:
1. [[ConvexCurveEscrow.pay]]
2. [[CvxCrvStakingWrapper.withdraw]]
3. [[CvxCrvStakingWrapper.getReward]]
4. [[CvxCrvStaking.getReward]]
5. [[Market.withdraw]]
6. [[INVEscrow.pay]]
7. [[xINV.redeemUnderlying]]
8. [[Oracle.getFeedPrice]]
9. [[DbrDistributor.onlyINVEscrow]]
10. [[DbrDistributor.setRewardRateConstraints]]
11. [[DbrDistributor.setRewardRate]]
12. [[Fed.expansion]]
13. [[Fed.takeProfit]]
14. [[GOhmTokenEscrow.delegate]]
15. [[GovTokenEscrow.delegate]]
16. [[INVEscrow.delegate]]
17. [[Market.repay]]
18. [[Market.liquidate]]
19. [[BorrowController.repay]]
20. [[BorrowController.onRepay]]

Hello, welcome to read the article titled "Inverse Finance FiRM Audit". The document mainly talks about the security audit findings of the [[FiRM]] protocol, potential reentrancy issues, and recommendations for improving the protocol's security. You can start asking questions like:

What potential reentrancy issues were identified in the [[ConvexCurveEscrow]] and [[INVEscrow]] contracts?->

How were the issues related to stale [[Chainlink]] answers and loose restrictions on [[onlyINVEscrow]] addressed in the protocol?->

What recommendations were provided to enhance the protocol's security?->

What potential reentrancy issues were identified in the [[ConvexCurveEscrow]] and [[INVEscrow]] contracts?

[[GPT-3.5 Turbo]]

The potential reentrancy issue identified in the [[ConvexCurveEscrow]] contract was related to the [[ConvexCurveEscrow.pay]] function calling [[CvxCrvStakingWrapper.withdraw]], which could allow a user to fully withdraw their collateral even if they have an open debt position. This issue was considered to have a medium likelihood.

In the case of the [[INVEscrow]] contract, a similar reentrancy issue was identified, although it was deemed to be much less likely compared to the one in [[ConvexCurveEscrow]]. The issue in [[INVEscrow]] was related to the [[xINV.redeemUnderlying]] function being called before transferring the escrow tokens in the [[INVEscrow.pay]] function, potentially leading to vulnerabilities if an external call to an attacker-controlled contract is introduced in the future.

What potential reentrancy issues were identified in the ConvexCurveEscrow and INVEscrow contracts?

[[GPT-3.5 Turbo]]

The potential reentrancy issue identified in the ConvexCurveEscrow contract was related to the [[ConvexCurveEscrow.pay]] function calling [[CvxCrvStakingWrapper.withdraw]], which could allow a user to fully withdraw their collateral even if they have an open debt position. This issue was considered to have a medium likelihood.

In the case of the [[INVEscrow]] contract, a similar reentrancy issue was identified, although it was deemed to be much less likely compared to the one in [[ConvexCurveEscrow]]. The issue in [[INVEscrow]] was related to the [[xINV.redeemUnderlying]] function being called before transferring the escrow tokens in the [[INVEscrow.pay]] function, potentially leading to vulnerabilities if an external call to an attacker-controlled contract is introduced in the future.

![](https://resource-cdn.sider.ai/0b6ec774f7a48fb7cb0c20546955f6ab.svg)



How were the issues related to stale Chainlink answers and loose restrictions on onlyINVEscrow addressed in the protocol?

[[GPT-3.5 Turbo]]

The issue related to stale Chainlink answers was addressed in the protocol by introducing a check in the BorrowController, which disables borrowing if the Chainlink feed has not updated the price within the configured threshold. This update was implemented as of commit e28dae4.

Regarding the loose restrictions on onlyINVEscrow, the issue was resolved by hardcoding the INV address and checking that it is equal to the market's collateral address. This update was implemented as of commit ac71fff.

![](https://resource-cdn.sider.ai/0b6ec774f7a48fb7cb0c20546955f6ab.svg)

1

2

3


can you explain this some more

[[Claude 3 Haiku]]

Sure, let me provide more details on how the issues related to stale Chainlink answers and loose restrictions on [[onlyINVEscrow]] were addressed:

1. Stale Chainlink answers:
    
    - The original issue was that the [[Oracle.getFeedPrice]] function only checked that the returned price was greater than zero, but did not check if the price was recent enough.
    - To address this, the recommendation was to always check if the price returned by the Chainlink feed is recent enough.
    - The update in [[commit e28dae4 introduced a check in the BorrowController]], which disables borrowing if the feed has not updated the price within the configured threshold. This ensures that stale prices cannot be used for borrowing.
2. Loose restrictions on [[onlyINVEscrow]]:
    
    - The original issue was that the [[DbrDistributor.onlyINVEscrow modifier]] was intended to grant access exclusively to escrows of specific markets, such as the [[INVEscrow]].
    - However, the implementation failed to effectively limit access to specific markets, and could result in escrows from unintended markets calling these functions.
    - The recommendation was to keep a [[local registry of the allowed markets in the DbrDistributor contract]], managed by its operator, to prevent unintended calls from escrows of other markets.
    - The update in [[commit ac71fff resolved this issue by hardcoding the INV address]] and checking that it is equal to the market's collateral address. This ensures that only the intended INVEscrow can call the DbrDistributor functions.

In summary, the protocol team addressed these issues by implementing more robust checks and validations to ensure the security and integrity of the system, particularly around the use of external price feeds and access control to critical protocol functions.


What recommendations were provided to enhance the protocol's security?

Claude 3 Haiku

According to the audit report, the main recommendations provided to enhance the protocol's security were:

1. Consider using reentrancy guards for all Market functions that perform external calls. This was recommended to address the potential reentrancy issues identified in the ConvexCurveEscrow and INVEscrow contracts.
    
2. Always check if the price returned by the Chainlink feed is recent enough, not just that it is greater than zero. This was recommended to address the issue of stale Chainlink answers being accepted.
    
3. Keep a local registry of the allowed markets in the DbrDistributor contract, managed by its operator. This was recommended to address the issue of the onlyINVEscrow restrictions being too loose.
    
4. Consider calling [[BorrowController.repay]] from [[Market.liquidate]] the same way it is called in the repay function. This was recommended to ensure that the BorrowController's daily limit is properly updated on liquidations.
    
5. Improve the overall code quality by addressing various typos, removing unnecessary TODO comments, improving naming conventions, and adding missing error messages and events.
    
6. Optimize the codebase by reducing the number of storage reads in certain functions.
    

The report emphasized that these recommendations aim to enhance the protocol's security by preventing issues that arise from unexpected reentrancies and adopting a more secure approach when handling external protocol integrations.

The original issue was that the Oracle.getFeedPrice function only checked that the returned price was greater than zero, but did not check if the price was recent enough. explain this mathematically

Claude 3 Haiku

Okay, let's explain the issue with the Oracle.getFeedPrice function not checking for recent enough prices mathematically.

The issue is that the function only checks that the returned price is greater than zero, but does not verify the timeliness of the price data. Mathematically, this can be represented as follows:

Let's define the following variables:

- `currentTimestamp`: The current timestamp of the blockchain.
- `priceTimestamp`: The timestamp of when the price was last updated in the Chainlink feed.
- `maxPriceAge`: The maximum acceptable age of the price data, set by the protocol.

The Oracle.getFeedPrice function should check the following condition:

```
currentTimestamp - priceTimestamp <= maxPriceAge
```

This condition ensures that the price data is not stale and is recent enough to be used for the protocol's operations.

However, the original implementation only checked:

```
price > 0
```

This means that the function would accept any price, as long as it is greater than zero, regardless of how old the price data is. This could lead to the protocol using stale price data, which could result in incorrect calculations, unexpected behavior, and potential security vulnerabilities.

The recommended solution is to add the additional check for the price data's timeliness, as shown in the first condition. This ensures that the protocol only uses recent and reliable price data from the Chainlink feed, enhancing the overall security and integrity of the system.