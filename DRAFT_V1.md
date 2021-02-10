<div align="center">

# Yearn ARC

</div>

> Aave Request for Comment

<br />
<br />

- [Yearn ARC](#yearn-arc)
  - [Overview](#overview)
    - [Rationale](#rationale)
  * [YFI as a non-loanable collateral](#yfi-as-a-non-loanable-collateral)
    - [Aave Documentation](#aave-documentation)
  * [Disabled Depositing reduces risk](#disabled-depositing-reduces-risk)
    - [Will Move the following Equations to an appendix](#will-move-the-following-equations-to-an-appendix)
  * [Liquidation Threshold](#liquidation-threshold)
  * [Health Factor](#health-factor)
  * [Utilisaation Rate](#utilisaation-rate)
  * [Historical Utilisation Rate](#historical-utilisation-rate)
  * [Interest Rate Model](#interest-rate-model)
  * [Valuation Risk](#valuation-risk)
  * [Comparisons](#comparisons)
  * [Calculation Specifics](#calculation-specifics)
    - [Acknowledgments](#acknowledgments)

### Overview

YFI tokens from Yearn’s treasury are deposited into {Aave} for the
purpose of borrowing stablecoins {Aave} against it. The {aave}
governance commits to a stabe and preffered interest rate {aave} for a
period of five years. Yearn governance on the other hand commits to
maintaining an LTV of >=50% throughout the period. Implementation

Yearn Finance deposits >=1000 YFI and borrows stablecoins against it,
such that a minimum of LTV of 50% is maintained for at least five years.
In return, Yearn Finance is asking the Aave for a stable 3% interest
rate (aave) for that duration.

#### Rationale

Yearn’s treasury controls high-value capital that warrants special
treatment given the high interest rate that will be paid during the five
year commitment.

- This collaboration is a win-win for the token holders of both
  communities and further strengthens their cooperation.

- The {Maker, Aave} lock-in a guaranteed and large yield from the large
  line of credit.

- Yearn lock-in a predetermined interest rate thereby decreasing
  uncertainties around operational costs.

## YFI as a non-loanable collateral

### Aave Documentation

[disabling lending on asset](https://docs.aave.com/developers/v/1.0/developing-on-aave/the-protocol/lendingpool#reserveusedascollateraldisabled)
[source, LendingPool.sol](https://github.com/aave/aave-protocol/blob/9cf250b22d63b0c6a2accd9e0fe64b0b045557d8/contracts/lendingpool/LendingPool.sol#L142)

## Disabled Depositing reduces risk

Currencies only enabled for depositing and borrowing (not usable as
collaterals) present lower risk for the protocol. Collaterals are the
assets of the protocol.

To remain solvent, these assets must remain greater than the
liabilities, the loans. Currencies which can only be used for borrowing
should always be excessively backed by other currencies as the
collaterals.

The volatility of price can negatively affect the collateral which
safeguards the solvency of the protocol and must cover the loan
liabilities. The risk of the collateral falling below the loan amounts
can be mitigated through the level of coverage required, the
Loan-To-Value.

It also affects the liquidation process as the margin for liquidators
needs to allow for profit. The less volatile currencies are the
stablecoins followed by ETH, they have the highest LTV at 75%, and the
highest liquidation threshold at 80%.

The most volatile currencies REP and LEND have the lowest LTV at 35% and
40%. The liquidations thresholds are set at 65% to protect Aave users
from a sharp drop in price which could lead to undercollaterisation
followed by liquidation.

### Will Move the following Equations to an appendix

## Liquidation Threshold

<!--
$$
\text { Liquidation Threshold }=\frac{\sum \text { Collateral }_{i} \text { in } E T H \times \text { Liquidation } \text { Threshold }_{i}}{\text { Total Collateral in ETH }}
$$
-->
<img src="https://render.githubusercontent.com/render/math?math=%5Ctext%20%7B%20Liquidation%20Threshold%20%7D%3D%5Cfrac%7B%5Csum%20%5Ctext%20%7B%20Collateral%20%7D_%7Bi%7D%20%5Ctext%20%7B%20in%20%7D%20E%20T%20H%20%5Ctimes%20%5Ctext%20%7B%20Liquidation%20%7D%20%5Ctext%20%7B%20Threshold%20%7D_%7Bi%7D%7D%7B%5Ctext%20%7B%20Total%20Collateral%20in%20ETH%20%7D%7D">

## Health Factor

<!--
$$
H_{f}=\frac{\sum \text { Collateral }_{i} \text { in } E T H \times \text { Liquidation } \text { Threshold }_{i}}{\text { Total Borrows in } E T H+\text { Total Fees in } E T H}
$$
-->
<img src="https://render.githubusercontent.com/render/math?math=H_%7Bf%7D%3D%5Cfrac%7B%5Csum%20%5Ctext%20%7B%20Collateral%20%7D_%7Bi%7D%20%5Ctext%20%7B%20in%20%7D%20E%20T%20H%20%5Ctimes%20%5Ctext%20%7B%20Liquidation%20%7D%20%5Ctext%20%7B%20Threshold%20%7D_%7Bi%7D%7D%7B%5Ctext%20%7B%20Total%20Borrows%20in%20%7D%20E%20T%20H%2B%5Ctext%20%7B%20Total%20Fees%20in%20%7D%20E%20T%20H%7D">

Market risks have the most direct impact on the risk parameters:

## Utilisaation Rate

<!--
$$
U=\text { TotalBorrows / TotalLiquidity }
$$
-->
<img src="https://render.githubusercontent.com/render/math?math=U%3D%5Ctext%20%7B%20TotalBorrows%20%2F%20TotalLiquidity%20%7D">

## Historical Utilisation Rate

Since inception, across the assets of the Aave Market, full utilisation
was reached only 1% of days since inception. The table below shows the
Aave Market statistics on the maximum daily utilisation as at the 24th
of November 2020.

Statistic $\%$ of time
$\begin{array}{ll}=100 \% & 1 \% \\ >95 \% & 2.8 \% \\ >90 \% & 4.6 \% \\ >80 \% & 11.4 \% \\ >50 \% & 26 \% \\ <25 \% & 52.6 \% \\ <10 \% & 38.5 \% \\ <5 \% & 28.7 \%\end{array}$

[\*Historical Utilisation Rate, source Aave Documentation](https://docs.aave.com/risk/liquidity-risk/historical-utilization)

## Interest Rate Model

<!--
$$
\begin{array}{ll}
\text { if } U<U_{\text {optimal }}: & R_{t}=R_{0}+\frac{U_{t}}{U_{\text {optimal }}} R_{\text {slope1 }} \\
\text { if } U \geq U_{\text {optimal }}: & R_{t}=R_{0}+R_{\text {slope } 1}+\frac{U_{t}-U_{\text {optimal }}}{1-U_{\text {optimal }}} R_{\text {slope2 }}
\end{array}
$$
-->

<img src="https://render.githubusercontent.com/render/math?math=%5Cbegin%7Barray%7D%7Bll%7D%0A%5Ctext%20%7B%20if%20%7D%20U%3CU_%7B%5Ctext%20%7Boptimal%20%7D%7D%3A%20%26%20R_%7Bt%7D%3DR_%7B0%7D%2B%5Cfrac%7BU_%7Bt%7D%7D%7BU_%7B%5Ctext%20%7Boptimal%20%7D%7D%7D%20R_%7B%5Ctext%20%7Bslope1%20%7D%7D%20%5C%5C%0A%5Ctext%20%7B%20if%20%7D%20U%20%5Cgeq%20U_%7B%5Ctext%20%7Boptimal%20%7D%7D%3A%20%26%20R_%7Bt%7D%3DR_%7B0%7D%2BR_%7B%5Ctext%20%7Bslope%20%7D%201%7D%2B%5Cfrac%7BU_%7Bt%7D-U_%7B%5Ctext%20%7Boptimal%20%7D%7D%7D%7B1-U_%7B%5Ctext%20%7Boptimal%20%7D%7D%7D%20R_%7B%5Ctext%20%7Bslope2%20%7D%7D%0A%5Cend%7Barray%7D">

## Valuation Risk

> source: aave documentation

​Yearn is an ecosystem of financial products governed by the YFI token.
The smart platform offers different optimised strategies based on DeFi
primitives such as Aave, gaining immense traction and attracting nearly
a billion dollars of AUM in just over a month.

The governance token YFI is distributed to users who provide liquidity
for the various products. The token holders discuss strategies and vote
on policies in the governance forum while the technical side is led by
Andrey Cronje.

YFI Smart contract Risk: B-

The YFI smart contract was deployed on the 17th of July yet it already
holds more transactions and holders than some other assets of portfolio.
The simple ERC20 implementation with permissions only to the governance
contract controlled by the YFI holders leads to a reduced technical risk

YFI Counterparty Risk: B

YFI is fully decentralized, its distribution to Yearn liquidity
providers is among the fairest and most transparent. The community is
already strong of 6,000 holders.

YFI Market Risk: B-

## Comparisons

Yearn.finance allocated its total supply of tokens within a week, which
means that its market-cap-to-FDV ratio is equal to 100%.

Curve, on the other hand, distributes 2 million tokens each day via
liquidity mining, which will gradually inflate its supply to a maximum
of 3.03 billion CRV — resulting in an extremely low market-cap-to-FDV
ratio of 2.64%.

| **Curve**        | **YFI**          |
| ---------------- | ---------------- |
| Volatility (30D) | Volatility (30D) |
| 2.96             | 1.41             |
| Volatility (90D) | Volatility (90D) |
| 2.21             | 1.6              |
| Volatility (1Y)  | Volatility (1Y)  |
| 2.44             | 2.35             |
| Sharpe (30D)     | Sharpe (30D)     |
| 7.34             | 0.414            |
| Sharpe (90D)     | Sharpe (90D)     |
| 3.96             | 2.49             |
| Sharpe (1Y)      | Sharpe (1Y)      |
| 0.511            | 3.24             |

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

### Acknowledgments

- Wot_Is Goin_On
- Daniel L.
- Ali Atiia
