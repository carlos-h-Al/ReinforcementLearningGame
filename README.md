# Reinforcement Learning Projects

## Project No 1 - 🤖 AI Robo-Trainer: A Deep Q-Learning Project

This project demonstrates the process of training an AI agent from scratch using **Deep Q-Learning (DQN)**, a key algorithm in Reinforcement Learning. The goal is to teach a simple "robot" to navigate a 2D grid and find food.

What makes this project insightful is its **two-phase training strategy**. The agent is first trained to learn the basic rules and then is explicitly *finetuned* to correct "bad habits" and unlearn inefficient behaviors, much like training a real student.

https://github.com/user-attachments/assets/d0b0d4a9-9acc-47f3-aa32-92a94b9467a4

### The Result: A Smart & Efficient Agent

After the initial training, the agent was functional but clumsy. After the finetuning phase, its performance became nearly perfect.

Finetuned Agent in Action (8/10 Perfect Games):

| **Test Run (Turtle Graphics)** | **Initial Model (Model 2)** | **Finetuned Model (ft3_0)** |
| --- | --- | --- |
| Game 1  | 933 / 5000 | 291 / 5000 |
| Game 2 | 2108 / 5000 | 2046 / 5000 |
| Game 3 | 5000 / 5000 | 5000 / 5000 |
| Game 4 | 738 / 5000 | 5000 / 5000 |
| Game 5 | 3476 / 5000 | 5000 / 5000 |
| Game 6 | 2565 / 5000 | 5000 / 5000 |
| Game 7 | 1444 / 5000 | 5000 / 5000 |
| Game 8 | 505 / 5000 | 5000 / 5000 |
| Game 9 | 4682 / 5000 | 5000 / 5000 |
| Game 10 | 5000 / 5000 | 5000 / 5000 |
| Success Rate | 20% (Perfect Games) | 80% (Perfect Games) |

### The Main Challenge and its Solution

The initial training (Phase 1) produced an agent that understood the basic goal. It learned that "food is good" and "crashing is bad". However, when tested in a visual environment, a classic Reinforcement Learning problem emerged: the agent got stuck in endless loops.

This happens because the agent found a "local optimum", developing a strategy that felt safe and gave some rewards (like "being on the same axis as food") but failed to achieve the true goal ("eating the food"). My challenge was to teach the AI how to break these bad habits. I solved this by treating the agent like a student: first, let it learn the material, and second, correct its specific mistakes.

#### Phase 1: Initial Training (The "Student")

* **Goal:** To learn the basics of the game as fast as possible.

* **Method:** I used Deep Q-Learning (DQN). The agent's "brain" is a simple PyTorch neural network (`Linear_QNet`) that takes the 13-point game state (danger nearby, current direction, food location) and outputs the best action (straight, left, right).

* **Rewards:** I created a tiered reward system to guide it:

  * `+20`: For eating the food (The main goal!)

  * `+10`: For being 1 block away (You're close!)

  * `+5`: For being on the same row or column (Good direction!)

  * `-20`: For going out of bounds or taking too long.

**Result of Phase 1:** The agent learned to find food! This initial model (`model_2`) achieved a massive high score (337,877) in the virtual-only training environment, proving the concept worked.

#### Phase 2: Finetuning (The "Specialist")

* **Goal:** To eliminate the endless loops and bad habits.

* **Method:** I loaded the "brain" from our best-performing model (`killer_model.pth`) and changed the rules.

* **Key Changes:**

  1. **No More Exploration:** I turned off the agent's randomness (epsilon = 0). It would now only use what it had learned.

  2. **Reward Shaping (Penalties):** This is the most important part. I introduced negative rewards (punishments) for loop-like behavior:

    * **Loop 1 (Spinning):** If the agent turned left or right 15 times in a row, it received a `-5` penalty.

    * **Loop 2 (Circling Food):** If it performed a specific "turn-and-move" pattern 10 times, it received a `-5` penalty.

  3. **Removed Tier 2 Reward:** I removed the `+10` for "being close," as I suspected this was causing the agent to circle the food instead of eating it.

Result of Phase 2: Success! The penalties forced the agent to learn that looping was "bad" and less profitable than finding the food. The agent learned to "get unstuck" and find a new path. Its high score became much more consistent, as seen in the table above where it rapidly learns to hit the game's score cap.

### Technology Stack

* **Python 3**

* **PyTorch:** For building and training the Deep Q-Network.

* **NumPy:** For handling state data and calculations.

* **Matplotlib:** For plotting the training scores.

* **Turtle (used in a separate script):** For visually testing the agent's performance.
