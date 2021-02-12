---

title: Yearn Finance ARC description: draft ARC version: 2021.02.11 URL:
{link_to_post} tags:

---

# Yearn Finance ARC

### Abstract

YFI tokens from Yearn’s treasury are deposited into Aave for the purpose
of borrowing stable coins {Aave} against it. The Aave governance commits
to a stable and preferred interest rate towards Yearn for a period that
can be defined in the the implementation (referenced to aa the 'credit
facility')

### Implementation

Yearn Finance deposits >=1000 YFI and borrows stable coins against it,
such that a minimum of LTV of 50% is maintained for a period of time.
${time_d}.$ In return, Yearn Finance is asking the Aave for a stable 3%
interest rate Aave for that duration.

1. **Disable borrowing of YFI**

2. **3% interest rate on the deposited YFI, along with meeting the
   proposed parameters below**

3. **Creation of a Revolving Credit Facility for the purposes of
   facilitating reconciliation between accounts**

| Key                   | Value                 | Type     |
| --------------------- | --------------------- | -------- |
| Reserve Factor        | >5% \$RFACTOR <10%    | Percent  |
| Liquidation Threshold | 85%                   | Percent  |
| Liquidation Bonus     | 5%                    | Percent  |
| LTV_minimum           | 50%                   | Percent  |
| LoanAmount            | 1mm > \$AMOUNT < 10mm | Range    |
| Basis points          | 375                   | Decimals |
| Time_d                | Months                | Time     |

## Rationale

Yearn’s treasury controls high-value capital that warrants special
treatment given the high interest rate that will be paid during the
course of the credit terms.

- This collaboration is a win-win for the token holders of both
  communities and further strengthens their cooperation.
- The lock-in is a guaranteed as well as large yield for the large line
  of credit.
- Yearn lock-in a predetermined interest rate thereby decreasing
  uncertainties around operational costs.
- Through the Yearn Ecosystem, Aave will be able to draw additional loan
  originators, while also reducing default risk.

## Disabled Borrowing reduces risk

Assets that are only enabled for depositing and borrowing (not usable as
collaterals) present lower risk for the protocol. Collaterals are the
assets of the protocol.

For Aave to remain solvent, these assets must remain greater than the
liabilities, the loans. Assets that can only be used for borrowing
typically are over collateralized in order to offer a 'buffer' to
mitigate against default risk.

The volatility of price can negatively affect the collateral which
safeguards the solvency of the protocol (Aave) and must cover the loan
liabilities. The risk of the collateral falling below the loan amounts
can be mitigated through the level of coverage required, the
Loan-To-Value.

It also affects the liquidation process as the margin for liquidators
needs to allow for profit. The less volatile currencies are the stable
coins followed by ETH. They have the highest LTV at 75%, and the highest
liquidation threshold at 80%.

## Asset Risk Comparison

Yearn.finance allocated its total supply of tokens within a week, which
means that its market-cap-to-FDV ratio is equal to 100%.

Curve, on the other hand, distributes 2 million tokens each day via
liquidity mining, which will gradually inflate its supply to a maximum
of 3.03 billion CRV — resulting in an extremely low market-cap-to-FDV
ratio of 2.64%.

| **Key**          | **Curve** | **YFI** |
| ---------------- | --------- | ------- |
| Volatility (30D) | 2.96      | 1.41    |
| Volatility (90D) | 2.21      | 1.6     |
| Volatility (1Y)  | 2.44      | 2.35    |
| Sharpe (30D)     | 7.34      | 0.414   |
| Sharpe (90D)     | 3.96      | 2.49    |
| Sharpe (1Y)      | 0.511     | 3.24    |

\*This is the asset's volatility calculated over the past 30 days of
daily returns.

\*Volatility is defined as the annualized standard-deviation of daily
returns.

## Calculation Specifics

To calculate correct historically archived deposit rates we use the
index based rate claculation.
[source contract link](https://github.com/aave/aave-js/blob/6c74c6df3c9d86a652b3adbf9e285a00f8497f0c/src/helpers/pool-math.ts#L124)

```solidity
export function calculateAverageRate(andyk, 4 months ago: • initial implementation
  index0: string,
  index1: string,
  timestamp0: number,
  timestamp1: number
): string {
  return valueToBigNumber(index1)
    .dividedBy(index0)
    .minus('1')
    .dividedBy(timestamp1 - timestamp0)
    .multipliedBy(SECONDS_PER_YEAR)
    .toString();
}
```

Rate: The rate is based on a utilization metric which represents the
current rate at this point of time. _(POSIX Unix Epoch Time)_

Index: The index keeps track of "growth" also incorporating things like
the 'flash premium'.

## Comparison to other assets

| **Name**    | **Symbol** | **Collateral** | **Loan To Value** | **Liquidation Threshold** | **Liquidation Bonus** | **Reserve Factor** |
| ----------- | ---------- | -------------- | ----------------- | ------------------------- | --------------------- | ------------------ |
| Binance USD | BUSD       | no             | -                 | -                         | -                     | 10%                |
| DAI         | DAI        | yes            | 75%               | 80%                       | 5%                    | 10%                |
| Balancer    | BAL        | yes            | 55%               | 65%                       | 10%                   | 20%                |
| Curve DAO   | CRV        | yes            | 40%               | 55%                       | 15%                   | 0%                 |
| Enjin       | ENJ        | yes            | 55%               | 60%                       | 10%                   | 20%                |
| Ethereum    | ETH        | yes            | 80%               | 82.50%                    | 5%                    | 10%                |
| Uniswap     | UNI        | yes            | 60%               | 65%                       | 10%                   | 20%                |
| Wrapped BTC | WBTC       | yes            | 70%               | 75%                       | 10%                   | 20%                |
| Wrapped ETH | WETH       | yes            | 80%               | 82.50%                    | 5%                    | 10%                |
| Yearn YFI   | YFI        | yes            | 40%               | 55%                       | 15%                   | 20%                |
| 0x          | ZRX        | yes            | 60%               | 65%                       | 10%                   | 20%                |

| **Statistic** | **BAT** | **ETH** | **KNC** | **LEND** | **LINK** | **MANA** | **MKR** | **REN** | **REP** | **SNX** | **WBTC** | **YFI** | **ZRX** |
| ------------- | ------- | ------- | ------- | -------- | -------- | -------- | ------- | ------- | ------- | ------- | -------- | ------- | ------- |
| Max           | 100%    | 80.40%  | 100%    | 17.60%   | 43.40%   | 82.60%   | 100%    | 45.70%  | 57.30%  | 100%    | 100%     | 65.70%  | 64.90%  |
| 1             | 0.30%   | 0%      | 3.10%   | 0%       | 0%       | 0%       | 0.30%   | 0%      | 0%      | 9.30%   | 0.30%    | 0%      | 0%      |
| >95%          | 0.30%   | 0%      | 5.60%   | 0%       | 0%       | 0%       | 1.20%   | 0%      | 0%      | 13.70%  | 0.30%    | 0%      | 0%      |
| >90%          | 1.60%   | 0%      | 6.80%   | 0%       | 0%       | 0%       | 1.20%   | 0%      | 0%      | 15.20%  | 0.30%    | 0%      | 0%      |
| >80%          | 2.20%   | 0.30%   | 8.10%   | 0%       | 0%       | 0.60%    | 1.20%   | 0%      | 0%      | 19.60%  | 0.60%    | 0%      | 0%      |
| >50%          | 9.60%   | 4%      | 10.60%  | 0%       | 0%       | 9.90%    | 2.20%   | 0%      | 2.20%   | 54.70%  | 1.60%    | 0.30%   | 1.90%   |
| <25%          | 81.40%  | 77.60%  | 81.10%  | 100%     | 95%      | 76.40%   | 85.40%  | 32.30%  | 86.30%  | 11.50%  | 97.20%   | 19.60%  | 81.10%  |
| <10%          | 55.30%  | 52.80%  | 72%     | 98.10%   | 84.50%   | 67.10%   | 72.40%  | 25.80%  | 64.20%  | 0.30%   | 89.80%   | 12.10%  | 49.40%  |
| <5%           | 26.70%  | 11.80%  | 52.80%  | 96.30%   | 14.60%   | 51.20%   | 14.60%  | 10.20%  | 57.10%  | 0%      | 64.90%   | 10.60%  | 38.80%  |

### Acknowlegments

- Wot_Is Goin_On
- Daniel L.
- Ali Atiia
