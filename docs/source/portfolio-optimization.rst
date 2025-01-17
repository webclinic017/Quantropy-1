Portfolio Optimization
**********************

The aim of **portfolio optimization** is to construct a portfolio by determining what proportions of our portfolio that each
asset should hold, according to quantifiable characteristics such as risk, return, covariance matrices, as well as
mathematical methodologies such as optimization (i.e. CLA) and machine learning techniques (i.e. hierarchical clustering).

Modern Portfolio Theory
=======================

In a 1952 essay, economist Harry Markowitz introduced **Modern Portfolio Theory** (MPT). Also termed *mean-variance analysis*,
it is a mathematical framework for to determine the proportion of an asset in the portfolio, so that the expected return is maximized
for a given level of risk. It formalizes the idea of *diversification*, i.e. that owning different types of financial assets is less
risky than owning only one. Essentially, an asset's risk and return should not be assessed by itself, but by how it
contributes to the risk and overall return of a portfolio. This framework has then been improved by other economists
and mathematicians who went on to account for its limitations.

Markowitz Mean-Variance Framework (1952)
------------------------------------------

.. autoclass:: portfolio_management.portfolio_optimization.ModernPortfolioTheory

Treynor-Black Model (1973)
--------------------------

Unlike the **Markowitz's** approach for portfolio allocation, the **Treynor-Black model** is a type of *active* portfolio
management.

.. autoclass:: portfolio_management.portfolio_optimization.TreynorBlackModel

Black-Litterman Model (1990)
----------------------------

Post Modern Portfolio Theory (1991)
===================================

Two major limitations of MPT are in measuring *risk* and *return* in a way that doesn't represent the realities of the
investment markets. Specifically, it assumes the following:

*   **Risk proxy:** The variance of portfolio returns is the correct measure of investment risk. Using the variance (or its square root,
    the standard deviation) implies that uncertainty about better-than-expected returns is equally averred as uncertainty
    about returns that are worse than expected. It has long been recognized that investors typically do not view as
    risky those returns above the minimum they must earn in order to achieve their investment objectives.
    They believe that risk has to do with the bad outcomes (i.e., returns below a required target), not the good
    outcomes (i.e., returns in excess of the target) and that losses weigh more heavily than gains.

*   **Statistical distribution:** The investment returns of all securities and portfolios can be adequately represented by a joint elliptical
    distribution, such as the **normal distribution**. The assumption of a normal distribution is a major practical limitation,
    because it is symmetrical. Using the normal distribution to model the pattern of investment returns makes investment results with
    more upside than downside returns appear more risky than they really are.

Recent advances in portfolio and financial theory, coupled with increased computing power, have overcome these limitations.
The resulting expanded risk/return paradigm is known as Post-Modern Portfolio Theory, or PMPT.
Thus, MPT becomes nothing more than a special (symmetrical) case of PMPT.

.. note::   The risk measures we will describe below are mainly used to evaluate mutual fund and portfolio manager performance.
            It is important to note that some variables will cause differing results, such as the *frequency* (daily, weekly, monthly, yearly)
            and the *window*(from which date to which date). For example, some services calculate 'during the last 3 years, the 30 days X is Y'.

            Also, regarding time horizon, if you have a mean for a horizon of say 10 days, and a volatility over a year, then
            you can convert that volatility to be over the horizon by multiplying by :math:`\sqrt{10/252}`.

As a case, let's take as an example the following portfolio

>>> from portfolio_management.Portfolio import Portfolio
>>> from datetime import datetime
>>> assets = ['AAPL', 'V', 'KO', 'CAT']
>>> portfolio = Portfolio(assets=assets)
>>> portfolio.slice_dataframe(to_date=datetime(2021, 1, 1), from_date=datetime(2016, 1, 1))
>>> portfolio_returns = portfolio.get_weighted_sum_returns(weights=np.ones(len(assets)) / len(assets))
>>> print(portfolio_returns.head())
Date
1970-01-05 23:59:59   -0.004553
1970-01-06 23:59:59   -0.003728
1970-01-07 23:59:59   -0.006211
1970-01-08 23:59:59    0.001179
1970-01-09 23:59:59   -0.005915
dtype: float64

First, we cover some measures that are based on the *Capital Asset Pricing Model*:

.. autofunction:: portfolio_management.risk_quantification.jensens_alpha
.. autofunction:: portfolio_management.risk_quantification.capm_beta

Risk Deviation Measures
-----------------------

.. autofunction:: portfolio_management.risk_quantification.standard_deviation
.. autofunction:: portfolio_management.risk_quantification.average_absolute_deviation
.. autofunction:: portfolio_management.risk_quantification.lower_semi_standard_deviation

Risk Adjusted Returns Measures Based on Volatility
++++++++++++++++++++++++++++++++++++++++++++++++++

Volatility is simply the average dispersion of the returns around their mean

.. autofunction:: portfolio_management.risk_quantification.treynor_ratio
.. autofunction:: portfolio_management.risk_quantification.sharpe_ratio
.. autofunction:: portfolio_management.risk_quantification.information_ratio
.. autofunction:: portfolio_management.risk_quantification.modigliani_ratio



.. autofunction:: portfolio_management.risk_quantification.roys_safety_first_criterion

>>> print(roys_safety_first_criterion(portfolio_returns=portfolio_returns, minimum_threshold=0.02, period=252))
0.7591708635828361

Value at Risk (VaR)
-------------------

Measures of risk-adjusted return based on volatility treat all deviations from the mean as risk, whereas measures of
risk-adjusted return based on lower partial moments consider only deviations below some predefined minimum return
threshold, t as risk i.e. positive deviations aren't risky. VaR is a more probabilistic view of loss as the risk of a portfolio

.. autofunction:: portfolio_management.risk_quantification.value_at_risk_historical_simulation
.. autofunction:: portfolio_management.risk_quantification.value_at_risk_variance_covariance
.. autofunction:: portfolio_management.risk_quantification.value_at_risk_monte_carlo
.. autofunction:: portfolio_management.risk_quantification.conditional_value_at_risk

Risk Adjusted Returns Measures Based on VaR
+++++++++++++++++++++++++++++++++++++++++++
Value at Risk computes the expected loss over a specified period of time given a confidence level

.. autofunction:: portfolio_management.risk_quantification.excess_return_value_at_risk
.. autofunction:: portfolio_management.risk_quantification.conditional_sharpe_ratio

Drawdown Risk
-------------

.. autofunction:: portfolio_management.risk_quantification.drawdown_risk

Partial Moments Risk
--------------------

These measures consider downside risk, and do not assume that returns are normally adjusted.
The mean and variance do not completely describe the distribution, therefore using only both of
them (i.e. the first and second moment of a distribution), would technically assume a normal distribution.

.. autofunction:: portfolio_management.risk_quantification.lpm
.. autofunction:: portfolio_management.risk_quantification.hpm

Risk Adjusted Returns Measures Based on Partial Moments
+++++++++++++++++++++++++++++++++++++++++++++++++++++++

.. autofunction:: portfolio_management.risk_quantification.omega_ratio
.. autofunction:: portfolio_management.risk_quantification.sortino_ratio
.. autofunction:: portfolio_management.risk_quantification.kappa_three_ratio
.. autofunction:: portfolio_management.risk_quantification.gain_loss_ratio
.. autofunction:: portfolio_management.risk_quantification.upside_potential_ratio


Risk Parity (1996)
==================

Hierarchical Risk Parity (2016)
-------------------------------

Universal Portfolio Algorithm (2016)
====================================

