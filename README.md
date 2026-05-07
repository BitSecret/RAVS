# RAVS

This is the official implementation of the paper "Agentic Geometry Problem Solving via Human-like Parallel Bidirectional
Reasoning". We present a unified neuro-symbolic reasoning framework, named **Reflect Agent Verified Solve (RAVS)**,
which deeply integrates large language models, agent architectures, and formal symbolic solvers. The LLM serves as a
planner responsible for high-level semantic understanding and path reflection, while the symbolic solver acts as an
executor responsible for formal verification and rigorous theorem application. The neural reasoning capability of the
LLM and the logical completeness of the symbolic system complement each other, fundamentally eliminating the risk of
hallucinations. We also construct the first bidirectional symbolic reasoning engine that fully unifies forward and
backward solving. On the FormalGeo7K benchmark, RAVS achieves a 95.80% solving accuracy, substantially outperforming
existing state-of-the-art methods, without requiring any additional problem-specific annotated data.

![architecture.png](architecture.png)

## Running

Download the dataset and log from
[Google Drive](https://drive.google.com/file/d/1Rziz2vaXKUsVaaRTmaJm9SUFPOK50oB5/view?usp=drive_link) or
[Baidu Netdisk](https://pan.baidu.com/s/1lDEF-vdjKxHd7YGmPkGjPA?pwd=fffs), and extract them to the current project.
Create a new `.env` file in the project directory Now your directory structure should look like:

    RAVS/
    |--datasets/
    |  |--diagram/
    |  |--ggbs/
    |  |--problems/
    |  |--summarize_prompt.txt
    |  |--system_prompt.txt
    |  └──gdl.json
    |
    |--outputs/
    |  |--agent/
    |  |--log/
    |  |--fig-statistics.pdf
    |  └──tab-main_results.txt
    |
    |--src/
    |  └──ravs/
    |     |--agent_loop.py
    |     |--chart.py
    |     |--symbolic_solver.py
    |     └──utils.py
    |
    |--.env
    |--.gitignore
    |--architecture.png
    |--LICENSE
    |--pyproject.toml
    └──README.md

Create a new Python environment and install dependencies:

    $ conda create -n RAVS python=3.12.12
    $ conda activate RAVS
    $ cd RAVS
    $ pip install -e .

Drawing figures and tables in the paper:

    $ cd src/ravs
    $ python chart.py

To reproduce our experiments:

    $ cd src/ravs
    $ python agent_loop.py

Before running `agent_loop.py`, please add a `.env` file in the `RAVS` directory and configure the following parameters:

    Deepseek_BASE_URL="https://api.deepseek.com"
    Deepseek_API_KEY="your_api_key"
    Deepseek_MODEL_ID="deepseek-v4-pro"

If you wish to use a different base model, such as Qwen3.6 Plus, you can add the following information to the `.env`
file and also modify the parameter `model_names=['BaiLian']` in the `main` function of `agent_loop.py`. If you want to
run with multi-processing, you can add multiple model names, e.g., `model_names=['Deepseek', 'Deepseek', 'BaiLian']`.
Each model name corresponds to one process.

    BaiLian_BASE_URL="https://dashscope.aliyuncs.com/compatible-mode/v1"
    BaiLian_API_KEY="your_api_key"
    BaiLian_MODEL_ID="qwen3.6-plus"

## Citation

coming soon...