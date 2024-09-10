# HCoTT: Hierarchical Chain-of-Thought Distillation

We will open source the code after the paper is officially accepted. 

# Frame Work of HCoTT
![](./main.pdf)

# Example of HCoTT
![](./example.pdf)
This is a three-level HCoT. $q$: "what causes direct damage to the lungs? }". $A_c$: "\textit{(A) breathing (B) oxygen (C) influenza (D) parasites (E) cigars (F) germs (G) steroids (H) respiring}". $a_{t}^*$: "\textit{(E) cigars". 

All explanation nodes need to go through the Mask function after being generated. $v_{1}^1$,$v_{2}^1$ are the explanations deduced from the problem $q$. $v_{1}^1$: "[Mask] can directly damage the lungs when smoked, leading to respiratory issues and diseases such as lung cancer.". $v_{2}^1$: "[Mask] can cause direct damage to the lungs when smoked, leading to respiratory issues and potential lung diseases.". 

$v_{1}^2$,$v_{2}^2$ are the explanations deduced from $v_{1}^1$, and $v_{3}^2$,$v_{4}^2$ are the explanations deduced from $v_{2}^1$. $v_{1}^2$:"\textit{Smoking [Mask] introduces harmful chemicals and toxins directly into the lungs, causing damage to the respiratory system.}". $v_{2}^2$:"[Mask] and [Mask] can also cause direct damage to the lungs by causing infections and inflammation.". $v_{3}^2$:"[Mask] and [Mask] can also cause direct damage to the lungs by causing infections and respiratory illnesse". $v_{4}^2$:"Smoking [Mask] exposes the lungs to harmful chemicals and toxins, which can lead to direct damage and irritation of the lung tissue.". 


# Prompt of HCoTT

Here we can show two prompts under the three-layer HCoT, Prompt1 is used for the second layer CoT, use $q$, $A_{c}$, $a_{t}^{*}$ to generate $v_{1}^{1}$, $v_{2}^{1}$. Prompt2 is used for the third layer CoT, use $q$, $A_{c}$, $a_{t}^{*}$, $v_{1}^1$ to generate $v_{1}^{2}$, $v_{2}^{2}$ and $q$, $A_{c}$, $a_{t}^{*}$, $v_{2}^{1}$ to generate $v_{3}^{2}$, $v_{4}^{2}$. The current prompt is still a very simple version, and we will continue to optimize the construction of the prompt in the future.

In the process of constructing the HCoT, we adopt a one-shot method to let the large language model learn the relationship between the question, answer, and explanation in a given case, and automatically construct the required explanation. The advantage of this is that the bias caused by human subjective factors can be reduced as much as possible. Here are the two prompts we constructed:

Prompt1 is: "Q: what will move to another area if their habitat will no longer support them? Answer choices: (a) density (b) Birds (c) squids (d) humans (e) clouds (f) gravity(g) cows(h) Whales A: The answer is cows. The explanation is: If a habitat can no longer support animals then those animals will move to another area.
Q: {} A: The answer is {}. The brief explanation is:".

Prompt2 is: "Q: what will move to another area if their habitat will no longer support them? Answer choices:(a) density(b) Birds(c) squids(d) humans(e) clouds(f) gravity(g) cows(h) Whales A: The answer is cows. The brief explanation is: If a habitat can no longer support animals then those animals will move to another area. A brief explanation of another perspective is: Cows are social animals.
Q: {} A: The answer is {}. The brief explanation is:{}. A brief explanation of another perspective is:"
