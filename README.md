# Bayesian Dynamic Pricing Simulation

This project implements a Bayesian dynamic pricing simulation using Thompson Sampling and linear programming.

The model simulates a firm choosing prices for a product over multiple rounds while learning about uncertain demand. Demand is modelled using a Gamma-Poisson Bayesian framework, and the optimal pricing policy is selected using a linear programming optimisation step.

The project also visualises how the model updates its beliefs about demand over time and generates an animated GIF of the pricing simulation.

## Project Overview

Dynamic pricing is the process of adjusting prices based on demand, uncertainty, and business constraints.

In this project, the true demand curve is hidden from the pricing algorithm. The algorithm must learn demand at different price levels by observing realised customer demand after selecting prices.

The simulation combines:

* Bayesian updating
* Thompson Sampling
* Gamma-Poisson conjugate priors
* Linear programming
* Revenue maximisation
* Inventory constraints
* Data visualisation and animation

## Core Idea

The model has a set of candidate prices:

* 1.99
* 2.49
* 2.99
* 3.49
* 3.99
* 4.49

The true demand is generated from a hidden linear demand function:

$$
D(p) = 50 - 7p
$$

where:

* $D(p)$ is the expected demand at price $p$
* $p$ is the selected price

The algorithm does not directly know this true demand function. Instead, it learns demand over time through observed sales.

## Bayesian Demand Model

Demand at each price is modelled as a Poisson random variable:

$$
D \sim \text{Poisson}(\lambda)
$$

The demand rate $\lambda$ is given a Gamma prior:

$$
\lambda \sim \text{Gamma}(\alpha, \beta)
$$

This is useful because the Gamma distribution is the conjugate prior for the Poisson distribution.

After observing demand, the belief is updated using:

$$
\alpha_{\text{new}} = \alpha_{\text{old}} + \text{observed demand}
$$

$$
\beta_{\text{new}} = \beta_{\text{old}} + 1
$$

The posterior mean demand estimate is then:

$$
\mathbb{E}[\lambda] = \frac{\alpha}{\beta}
$$

## Thompson Sampling

At each round, the model samples a possible demand estimate for each price from its current Gamma beliefs.

This creates a balance between:

* Exploration: testing prices where demand is still uncertain
* Exploitation: choosing prices that currently appear profitable

The sampled demand estimates are then passed into an optimisation problem to decide the pricing policy.

## Optimisation Problem

The model uses linear programming to choose a randomised pricing distribution.

The objective is to maximise expected revenue:

$$
\text{Expected Revenue} = p \times \hat{D}(p)
$$

subject to:

* price probabilities summing to 1
* expected demand not exceeding available inventory
* probabilities being non-negative

The optimisation returns a probability distribution over prices. The selected price is then sampled from this distribution.

## Simulation Process

Each round follows these steps:

1. Sample demand estimates from the current Gamma beliefs
2. Solve a linear programming problem to find the optimal pricing distribution
3. Randomly select a price using the optimised distribution
4. Observe realised demand from the hidden demand model
5. Record revenue
6. Update the Bayesian belief for the selected price
7. Repeat the process over multiple rounds

## What the Code Does

The script:

* Defines a set of possible price levels
* Creates hidden true demand using a linear demand function
* Sets Gamma priors for demand at each price
* Uses Thompson Sampling to estimate demand
* Uses linear programming to optimise expected revenue
* Simulates realised demand using a Poisson distribution
* Updates beliefs using Gamma-Poisson conjugacy
* Stores belief history over time
* Visualises posterior demand distributions
* Visualises implied revenue distributions
* Plots realised demand and selected prices over time
* Saves an animated GIF of the simulation

## Technologies Used

* Python
* NumPy
* SciPy
* Matplotlib
* Tabulate

## Installation

Install the required packages:

```bash
pip install numpy scipy matplotlib tabulate
```

If you want to save the animation as a GIF, you may also need ImageMagick installed on your system.

On macOS, this can usually be installed with:

```bash
brew install imagemagick
```

On Linux, it can usually be installed with:

```bash
sudo apt-get install imagemagick
```

## How to Run

Run the script with:

```bash
python bayesian_dynamic_pricing.py
```

The script will print intermediate belief updates, selected prices, realised demand, and revenue.

It will also show a final visualisation and attempt to save an animated GIF called:

```text
bayesian_dynamic_pricing.gif
```

## Example Output

The script prints information such as:

```text
True demands: [...]
True revenues: [...]
Optimal distribution: [...]
Selected price 3.49 => demand 24, revenue 83.76
```

The exact output changes between runs because the model uses random sampling.

## Visualisation

The final plot contains three panels:

1. Posterior demand distributions by price
   Shows the model's current beliefs about demand at each price.

2. Implied revenue distributions by price
   Shows how uncertainty in demand translates into uncertainty in revenue.

3. Realised demand and selected price over time
   Shows which prices were chosen and what demand was observed during the simulation.

The animation replays how the model's beliefs evolve across the simulation rounds.

## Financial and Business Interpretation

This project demonstrates how a pricing algorithm can learn from demand uncertainty over time.

At the start, the model has weak prior beliefs about demand at each price. As it observes realised demand, it updates its beliefs and becomes more confident about which prices are likely to generate higher revenue.

The Thompson Sampling step allows the model to keep exploring uncertain prices while still favouring prices that appear profitable.

The linear programming step adds a practical business constraint by ensuring that expected demand does not exceed available inventory.

## Limitations

This is a simplified educational simulation and should not be treated as a production pricing system.

Important limitations include:

* Uses a simple linear hidden demand curve
* Assumes demand follows a Poisson distribution
* Assumes independent demand beliefs for each price
* Does not model competitor pricing
* Does not model customer behaviour over time
* Does not include seasonality or market trends
* Does not account for price elasticity changing over time
* Does not include costs, margins, or profit optimisation
* Uses simulated demand rather than real sales data
* The model only updates the selected price each round

## Possible Improvements

Future extensions could include:

* Using real transaction or sales data
* Modelling profit instead of revenue
* Adding product costs and margins
* Adding multiple products
* Adding competitor pricing effects
* Modelling customer segments
* Using a contextual bandit model
* Testing different priors
* Comparing Thompson Sampling with epsilon-greedy exploration
* Adding regret analysis
* Adding cumulative revenue tracking
* Backtesting the pricing strategy
* Building an interactive dashboard using Streamlit

## Purpose

This project was created as a beginner quantitative modelling project to practise:

* Bayesian inference
* Thompson Sampling
* dynamic pricing
* revenue optimisation
* linear programming
* simulation modelling
* Python data visualisation
* applied probability and statistics

## Disclaimer

This project is for educational purposes only. It is a simplified simulation of dynamic pricing and does not represent a production-ready pricing or trading system.
