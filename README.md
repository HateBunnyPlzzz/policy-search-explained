# A gentle introduction to Policy Search Algorithms — Reinforcement Learning

To recommend a specific arm I also use the estimated expected value.

There is a whole other class of algorithms that directly work with the representation of the Policy.

## What is a Policy?

Mapping from states to actions, or S -> Actions probabilities, but in this case, we really have no states. *So far we have not considered any states*. So what would a policy be in this case? It could be just one arm or it could be some kind of probability distribution over the arms.

Denoting this by $\pi _{t}\left( a\right)$ — Probability of pulling the arm (a),

Our goal is to keep changing this policy over time such that eventually, I will be recommending with probability one to pull the **best arm**. So now what a learning algorithm has to do in such a case is, it has to give me a way of changing pi(t) with time.

So one way of thinking about things like epsilon-greedy or soft-max is that they defined this $\pi _{t}$ in an indirect fashion.

Probability of picking an action was $a_{t}=a$, that is exactly what $\pi _{t}\left( a\right)$ is.

$$\pi _{t}\left( a\right) \cong pr( a_{t}= a)$$

Later will be conditioning this on the states also that’ll essentially be the probability that $a_{t}=a$ given that $s_{t}=s$ but since there’s only one state we do not have to do that. Some of the earliest known approaches to solving bandits use this kind of *Direct Policy Search approach*. Let us assume a very simplified situation which is called **Binary Bandit**, where there are only two outcomes 0 or 1 for bad and good outcomes respectively.

So $R_{t}$ can be either 0 or 1 — let us say we have only 2 arm bandit cases — arm1 and arm2.

So **if $R_{t}=1$**, at some time — action we took at time $t$, reward I got was 1 — then I'll make

$$\pi _{t+1}\left( a_{t}\right) =\pi _{t}\left( a_{t}\right) +\alpha \left[ 1-\pi _{t}\left( a_{t}\right) \right]$$
when alpha is small enough, so the best action will **eventually converge** to a probability of 1.

When alpha ~ very large -> This might cause oscillations while convergence.

$$\pi_{t+1}(a’ ) = \pi_{t}(a’)(1 — \alpha)$$
$$a’=a_{t}$$
The above is one set of updated rules.

**If** $R_{t}=0$
Flip this around, Reducing the probability of the action taken at time t,   
$$\pi _{t+1}\left( a_{t}\right) =\pi _{t}\left( a_{t}\right) \left( 1-\beta \right)$$
Increasing the probability of the action I didn’t take at time t.  
$$\begin{aligned}\pi _{t+1}\left( a’\right) =\pi _{t}\left( a’\right) +\beta \left[ 1-\pi _{t}\left( a’\right)\right] \end{aligned}$$
Doing this just to make sure it’s a probability distribution due to constraint of probabilities having to sum to 1, will automatically reward the other action.

Depending on the relationship between alpha and beta, So you have different kinds of algorithms

**When** $\alpha$ = $\beta$ we have $L_{(R-p)}$ algorithm [Linear Reward Penalty] :  
You do something, you change the parameters on a reward and you also change the parameters when you get a penalty.

*Reward is +1 and penalty is 0, and you change them by equal amounts.*

**When** $\alpha$ >> $\beta$, $L_{(R-\varepsilon p)}$, I do something when I get a reward, and I do a much much smaller change when I get a penalty.

**When** $\beta=0$, $L_{(R-I)}$, $I$ stands for Inaction, *Ignore the penalties to avoid spiking and oscillations*  
So when the reward comes, I do something — when the penalty happens, I do nothing i.e, leave the probabilities as it is.

From the above-mentioned update rules, it turns out that there is a very very small change, the algorithm itself is the same, all we are doing is changing these things around and it turns out that the convergence behavior of these three algorithms is very different and depending on how rewarding your arms are. 
The takeaway from LRP methods is that whenever the difference between the reward values of the BEST arm and the non-optimal arm is larger then LRP will work fine, but when the difference between both considerations is very small, then LRP will going to have a hard time. In such cases, LRI works better, because whenever a small reward comes also, you keep hiking the probability which might lead to oscillations. In cases where rewards are scarce to come by, it’s best to ignore the penalties.

So taking this further now this looks like some kind of ad hock way of coming up with the policy updates, it seems a very natural way of doing it right!   
***But Is there a more systematic way of thinking about learning the policy directly?***  
Interestingly there exists approaches name Policy Gradient Methods, which will be having their separate discussion in the very upcoming posts.
