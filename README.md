# Strategic Collusion of LLM Agents: Market Division in Multi-Commodity Competitions 📈

## Overview 🌎
This repository contains the accompanying code for the [paper](https://arxiv.org/abs/2410.00031) titled "Strategic Collusion of LLM Agents: Market Division in Multi-Commodity Competitions". The study focuses on the application and implications of Large Language Models (LLMs) in setting prices and strategies in multi-commodity markets. Specifically, it investigates whether LLMs can engage in anti-competitive behavior such as specialization to monopolize different commodities within a market.

## Research Context 🔬
With the increasing deployment of AI technologies in marketplaces, the study explores the behavior of LLM-based pricing agents within the frameworks of both Bertrand and Cournot competition models. Key questions addressed include:
- Are LLMs capable of engaging in anti-competitive, specialization behavior in a multi-commodity market environment?
- How do LLMs adapt their strategies in response to varying market conditions and competition?

## Repository Structure ⛏️
- `cournot.ipynb`: Jupyter notebook containing the code for the Cournot competition experiments.
- `bertrand.ipynb`: Jupyter notebook containing the code for the Bertrand competition experiments.

## Setup ⚙️
All that you need to set up the repository is to create a `.env` file at the top-level directory and set an environment variable for our Open AI API Key like so:
```
OPENAI_API_KEY=<YOUR_KEY_HERE>
```
Adjust the constants and number of rounds in the two notebooks as you see fit, and run the experiment directly. 

The notebooks contain all the `pip install` commands required for package installation, so you can start with a fresh python environment. 


## Implementation Details 📖

### Parsing

We found that using a JSON-style output format yielded more consistent, albeit imperfect, results. The output is then converted into a JSON object, which would then be parsed to update variables like the market history, firm info, etc.

### Validation

After parsing an agent's response, the information was validated. In both problem settings, we ensured that every field (e.g. price/quantity, observations, etc.) was included in the output.

During initial Bertrand experiments, we would give LLMs the option to invest in product A or product B in every run, but only if they met the prerequisites (number of investment options and enough money). However, the agents often chose to invest when these prerequisites were not met. As a result, we transitioned to calculating the list of possible options given the state of each agent and then prompted each agent to choose from the list of options. This significantly reduced the frequency of invalid options being chosen. With this new format, we added another validation step that ensured that the LLM picked an option from the list of options we gave it.

### Manual Assist & Retry Behavior

For our experiments, we also implemented a Manual Assist feature in the case where our code wasn't able to parse the LLM output. With Manual Assist, we would read the output and manually enter the values to be stored. 

In cases where the output was simply malformed, the LLM would be re-prompted 3 times. If it failed 3 times, the experiment would be interrupted. However, this only occurred during initial prompting while we were experimenting with the prompt format.

We find that in our experiments, the Manual Assist feature was only used $< 10$ times, and the re-prompting feature was **never** used. 

### Querying Format
For both experiments, we used the OpenAI API and set the user message to the entire prompt. We also specified the response format of the API to be a JSON object.

## How to Cite 📚
If you use any of the material here, please cite our paper:

```
@misc{lin2024strategiccollusionllmagents,
      title={Strategic Collusion of LLM Agents: Market Division in Multi-Commodity Competitions}, 
      author={Ryan Y. Lin and Siddhartha Ojha and Kevin Cai and Maxwell F. Chen},
      year={2024},
      eprint={2410.00031},
      archivePrefix={arXiv},
      primaryClass={cs.GT},
      url={https://arxiv.org/abs/2410.00031}, 
}
```


