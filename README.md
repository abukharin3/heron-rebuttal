Dear Reviewer rYUe, 

Thank you for the very insightful and detailed review! Although you have presented several critiques of our paper, we believe many of them are addressed by our rebuttal. In particular, we have (1) added more discussion on the limitations/use cases of HERON, (2) Added stronger baselines and more evaluation environments, (3) Clarified the code generation experimental setting, and (4) cleaned up the writing, including adding citations and fixing descriptions in the methodology. Our detailed responses are provided below.

- **Weakness 1.1: Lack of difficulty in obtaining a reward function from weak reward factors.**
We want to point out that designing a reward function from reward features is one of the most challenging aspects of reinforcement learning. This problem has a very large literature behind it (See lines 28-42, 90-115) and it has been shown numerous times choosing a heuristic reward function is difficult. Based on your suggestions and that of URgo, we implemented several heuristics ourselves, but find that they cannot perform as well as HERON and have a large tuning cost. Therefore we believe the problem of obtaining a reward function from weak reward factors is still of significant interest to the RL community.

- **Weakness 1.2: Requirement of reward factors makes HERON inapplicable to fine tuning general LLMs.**
Although HERON is motivated by RLHF, we do not claim that it can replace RLHF in every scenario. Indeed, we cannot imagine a way for weak reward factors alone to finetune a LLM for value alignment. However, in our experiments we show that HERON can reach SOTA performance in two tasks (traffic light control and code generation), both of which are tasks that have been heavily studied as applications of RL. Based on this success, we believe that although HERON may be inapplicable in some settings where weak factors cannot capture the true reward, there are several important settings (notably any that currently employ a reward engineering approach) where HERON could be useful.
Moreover, we believe that extensions of HERON could be used to finetune general purpose LLMs. Human feedback could be used as the first level of the decision tree, and then weak reward signals (i.e. unit tests, grammar checkers) could be used as secondary or tertiary levels. In this way human and machine/weak feedback signals could be combined for more efficient training. We will add a paragraph discussing the limitations of where HERON can be applied, as well as possible extensions of HERON, in the next version of the text.

- **Weakness 2: Need for stronger baselines.**
Thank you for the suggestion! We have implemented the baseline you suggested in seven environments: traffic light control, mountain car, pendulum, ant, cheetah, and hopper. The results can be seen in Figure 1 of the rebuttal material. We heavily tune this baseline, searching for $\beta \in (\{0.3, 0.4, 0.5, …, 1.0\})$. The results show that HERON either matches or beats this baseline in all environments. However, this baseline has a much higher tuning cost than HERON. We further investigate the tuning cost of the suggested  reward engineering baseline in Figure 2 of the rebuttal material. We find that this baseline is quite sensitive to the choice of hyperparameter setting, and even when heavily tuned sometimes cannot match the performance of HERON. This can be seen clearly for traffic light control, where none of the configurations can beat HERON (out of 32). In this experiment we investigated different normalization strategies (weight norm, clipping, standard normalization, and max-based normalization). Although we only tried one setting per reward hierarchy with HERON, it still outperforms all 32 configurations of the suggested reward engineering baseline. We suspect that the true reward weights in this setting are not similar to the exponentially decreasing pattern used by the suggested reward engineering baseline. These experiments further confirm the efficacy of HERON.

- **Weakness 3.1: Code generation details: Pass@K.**
We apologize for the lack of details on the code generation experiments presented in the main text. The APPS dataset contains 5000 test problems. On each problem we generate 200 programs, and then calculate Pass@k as in [44]. So in total, each method is evaluated on a total of 1 million generated programs. Therefore there are tens to hundreds of thousands of correct programs for every method. This is the same setting used in one of the most popular RL code generation works, CodeRL [3]. We will add relevant details to the text.

- **Weakness 3.2: Code generation experiments: random seeds for code generation.**
Our code is based on CodeRL, which takes a very long time to run (almost a week for the whole training process). Similar to their work, we only used one seed for each method due to the large computational cost. However, we recently ran experiments on CodeT5-plus, using the same HERON hyper-parameter setting as before, and find that our method can outperform both CodeRL and PPOCoder across all metrics on APPS (see Table 1 of the rebuttal material). This indicates the gains from HERON are fairly stable for code generation. In addition, we evaluate on random seeds in all of our other experiments where the computational cost is not as high.

- **Weakness 3.3: Problems with Filtered Pass@K.**
There is a misunderstanding here. Filtered Pass@k was proposed in [44] and is used in CodeRL [3]. Filtered Pass@K samples 200 samples from the model, and then checks which one passes unit tests that are already given in the problem. Then we calculate pass@K on these programs, giving us filtered pass@K. We clarify that this procedure is applied to all methods, not just HERON, so there is no unfair advantage given to HERON. In addition, this metric is actually very realistic, since a practitioner would always use the freely available unit tests available in the problem to filter the generated programs. We will clarify this in the text.

- **Weakness 3.4: Code generation performance with small K in Pass@k.**
In our case, we can generate more than 1 program per second, and with further parallelization we can generate even faster. Therefore Pass@K with larger K is more important, since inference is cheap. We remark that HERON has a much smaller tuning cost than CodeRL and PPOCoder, so even achieving equivalent performance may be seen as a success from our method. In addition, the gains in Pass@K are similar to those achieved by algorithms proposed in other papers. Finally, we recently ran experiments on CodeT5-plus, using the same HERON hyper-parameter setting as before, and find that our method can outperform both CodeRL and PPOCoder across all values of K on APPS (see Table 1 of the rebuttal material).

- **Weakness 4.1: Objective in line 176**
Thank you for catching this. Line 176 shows the expected return given a stochastic policy [1], but as you mentioned it is often approximated with a proxy to reduce variance [PPO, TRPO]. To fix this error and reduce confusion, we will change line 176 to be the expected discounted return, with no assumption of a stochastic policy. In addition, we will change the wording in lines 177-178 for correctness.

[1] Sutton, Richard S., et al. "Policy gradient methods for reinforcement learning with function approximation." Advances in neural information processing systems 12 (1999).
APA	

- **Weakness 4.2: Citing the Bradley-Terry preference model.**
Thank you for pointing this out. We will cite the original authors in addition to [17].

- **Weakness 4.3: Learn reward model on top of state features.**
Thank you for the suggestion. We will cite the works you mentioned in our methodology section.

- **Weakness 4.4: No RLHF works in the related works section**
Currently we discuss RLHF works in the introduction (see lines 43-55), as RLHF primarily serves as a motivating field, rather than a method we can directly compare our method to. We do not include it in the related works section to avoid redundancy. For now we will add the missing works you suggested to this lines 43-55 (in the introduction), but we are open to adding RLHF to the related works section.

- **Weakness 4.5: Contribution of $\alpha^{F(\cdot)}$.**
Please see our response to question 4.

- **Weakness 4.6: Mentioning DDPG in the main text.**
Thank you for the suggestion. We will add this detail to the main text.

- **Weakness 4.7: More descriptive table captions needed.**
Thank you for pointing this out. We will make the captions more descriptive, such that the tables are more or less self-contained.


- **Weakness 4.8: Clarity of HERON's description in the intro.**
Thank you for the suggestion. We will make this more clear from the onset. In particular, we will edit lines 56-73 to make the description of HERON more clear and discuss learning from a single ranking.

- **Question 1: Where is HERON inapplicable?**
HERON is inapplicable in two settings: (1) when weak reward signals are not available. However, in this case reward engineering, which is the main algorithm we try to improve upon, is also not available; (2) when the true reward signal cannot be approximated as a decision tree over the weak reward signals. This could happen in some cases where the expert believes two factors are equally important. In this setting, we could develop an extension to HERON where one factor is the sum of two equally important factors, or where the level of each of the equally important factors is switched randomly at every training iteration.

- **Question 2: Comparison to new baseline.**
We have addressed this in weakness 2.

- **Question 3: Pass@K metric clarification.**
We have addressed this in weakness 3.

- **Question 4: The setup with $\alpha$ is confusing.**
We apologize for the lack of clarity. The intuition here is that depending on the category of the reward factor received (this technique is primarily designed for categorical data), the difference between the reward achieved by two policies should be closer or further apart. For example, if one program passes the unit tests and the other program doesn’t, we would want the difference in reward to be larger than in the case where both programs fail the unit tests but one wins on its AST similarity score. 
We have also provided an equation in the rebuttal material. We will add this equation to the text in the next version.

- **More discussion on limitations needed.**
We will add our discussions on HERON’s limitations to the text. In particular, we will include the drawbacks of relying on reward factors and some cases where creating a decision tree would be difficult (such as when two factors a re equally important). 





