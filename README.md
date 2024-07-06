## Sic Bo Optimal Strategy
This code uses a genetic algorthm to play the casino game Sic Bo optimally.

_Disclaimer: By construction, no strategy grants the player a positive expected value in terms of payouts. Rather, we solve a slightly different question here._

### Gameplay
Sic Bo is played using three fair six-sided dice and a board consisting of predictions for the dice's values. Predictions include but are not limited to the specific sum of the dice (e.g. 4, 9, 12), that a one will be present on one of the faces, that all three of the dice will show the same value, or that a 4 and a 6 will be present in the outcome of the dice. The player bets on which of these outcomes will happen, and they may select as many as they choose and a specific dollar amount for each. Based on the probabilities of these occurrences, a specific payout is granted as a multiple of the initial bet. For instance, a valid bet on all three dice showing the same value pays 31 times the initial bet (1 to 31), and a 4 and a 6 being present pays 1 to 6. Note that specific payouts may depend on the casino; the payouts used here come from an undisclosed casino in Canada. 

When individually selected, several events are isometric to each other. Each of these has a negative expected payout, as can be seen through basic statistical computations. A linearity of expectation argument shows that no combination of events leads to positive expected returns, by consequence. Therefore, there is no way to ensure positive returns in the long run when playing sic bo. 

Here, we instead ask the following question: which combination of events leads to the greatest probability of making money? If we take a basic example using a $1 bet, the singular event of the sum being "big" (between 11 and 17) pays 1 to 1, and the event has a $107/216$ probability of winning. Therefore, there is a $107/216$ probability that we make $1 (and otherwise we lose), so there is a $107/216$ probability of making money. Now, if we add the option of the sum being "small" (between 4 and 10), again betting $1 on this event, there are four cases:

1. The sum is 3, and we lose both. The payout is -$2.
2. The sum is "small," so we make +$1 on the small bet, but lose $1 by consequence of the sum not being "big." The payout is $1 - $1 = $0.
3. The sum is "big," which follows the same argument as the sum being "small." The payout is $0.
4. The sum is 18, which follows the same argument as the sum being 3. The payout is -$2.

Thus, when selecting these two events, there is a probability of 0 that we see positive returns.

### Algorithm
Note that this problem is highly nonlinear, and it depends greatly on which events have already been selected when adding new ones. However, brute forcing the $2^{50}$ combinations is computationally infeasible. There may be a solution that exploits group theory to find symmetries in the selections of sets (e.g. "5 and 6" with "1 and 6" is equivalent in expectation to "3 with 4" and "2 with 3"), and work on this approach is ongoing. For these reasons, we used a genetic algorithm to find the best combination of options. The algorithm works iteratively by first proposing $N$ combinations of buttons, choosing the $B$ that produce the highest probabilities of making money, and then repeating the process by again using the best $B$ as proposals, along with a large selection of mutations of them (i.e. the same proposals with random events potentially deselected and random new ones potentially added) such that there are again $N$ combinations in the subsequent epoch. After repeating this process for sufficiently many epochs, we may arrive at an optimal solution. 

Running the algorithm several times, we were able to find a combination granting a $180/216 \approx 0.829$ probability of making money. It is possible that a better combination exists, but we are currently unsure. Have fun experimenting with this notebook!
