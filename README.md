# RAVS

This is the official implementation of the **NeurIPS 2026** paper "Agentic Geometry Problem Solving via Human-like Parallel Bidirectional Reasoning". We present a unified neuro-symbolic reasoning framework, named **Reflect Agent Verified Solve (RAVS)**, which deeply integrates large language models, agent architectures, and formal symbolic solvers. The LLM serves as a planner responsible for high-level semantic understanding and path reflection, while the symbolic solver acts as an executor responsible for formal verification and rigorous theorem application. The neural reasoning capability of the LLM and the logical completeness of the symbolic system complement each other, fundamentally eliminating the risk of hallucinations. We also construct the first bidirectional symbolic reasoning engine that fully unifies forward and backward solving. On the FormalGeo7K benchmark, RAVS achieves a 96.06% solving accuracy, substantially outperforming existing state-of-the-art methods, without requiring any
additional problem-specific annotated data.

![architecture.png](architecture.png)

## Running

Download the dataset and log from [Google Drive](https://drive.google.com/file/d/16Q4ebwKjtmgsC6PmhrURXYfJFyyQ2qhT/view?usp=sharing) or [Baidu Netdisk](https://pan.baidu.com/s/1qu6dtSlxhXzj7NvqnTQjmw?pwd=s4qm), and extract them to the current project. Create a new `.env` file in the project directory Now your directory structure should look like:

```text
RAVS/
|--datasets/
|  |--diagram/
|  |--ggbs/
|  |--problems/
|  |--gdl.json
|  |--summarize_prompt.txt
|  |--system_prompt.txt
|  └──system_prompt_no_bidirectional.txt
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
|     |--dataset.py
|     |--symbolic_solver.py
|     └──utils.py
|
|--.env
|--.gitignore
|--architecture.png
|--LICENSE
|--pyproject.toml
└──README.md
```

Create a new Python environment and install dependencies:

```shell
conda create -n RAVS python=3.12.12
conda activate RAVS
cd RAVS
pip install -e .
```

Drawing figures and tables in the paper:

```shell
cd src/ravs
python chart.py
```

To reproduce our experiments:

```shell
cd src/ravs
python agent_loop.py
```

Before running `agent_loop.py`, please add a `.env` file in the `RAVS` directory and configure the following parameters:

```text
Deepseek_BASE_URL="https://api.deepseek.com"
Deepseek_API_KEY="your_api_key"
Deepseek_MODEL_ID="deepseek-v4-pro"
```

If you wish to use a different base model, such as MiMo-V2.5-Pro, you can add the following information to the `.env` file and also modify the parameter `model_names=['MIMO']` in the `main` function of `agent_loop.py`. If you want to run with multi-processing, you can add multiple model names, e.g., `model_names=['Deepseek', 'Deepseek', 'MIMO']`. Each model name corresponds to one process.

```text
MIMO_BASE_URL="https://token-plan-cn.xiaomimimo.com/v1"
MIMO_API_KEY="your_api_key"
MIMO_MODEL_ID="mimo-v2.5-pro"
```

## Results

### Main Results

| Method           | Total |    L1 |    L2 |    L3 |    L4 |    L5 |    L6 |
|------------------|------:|------:|------:|------:|------:|------:|------:|
| Backward-DFS     | 33.73 | 67.99 | 32.99 |  9.78 |   5.9 |  5.36 |  0.62 |
| Backward-RS      | 34.05 | 68.85 | 33.51 |  9.86 |  5.22 |  4.76 |  0.62 |
| Backward-BFS     | 35.44 | 69.56 | 35.26 | 12.22 |  6.24 |  6.85 |  0.93 |
| Forward-DFS      | 36.16 | 56.85 | 40.36 | 25.32 | 12.47 |  8.04 |  4.05 |
| Forward-BFS      | 38.86 | 61.37 | 37.89 | 32.33 | 17.91 |  9.23 |  2.49 |
| Forward-RS       | 39.71 | 60.74 | 39.33 | 37.22 | 16.55 |  6.25 |  4.05 |
| Kimi-K2          | 60.75 | 69.05 | 70.29 | 65.77 |  36.0 | 33.33 | 19.15 |
| DeepSeek v3      | 60.79 | 75.72 | 58.81 | 63.26 | 46.29 | 23.44 | 33.87 |
| GPT-5 mini       | 64.79 | 74.11 |  63.3 | 64.66 |  53.5 | 53.23 | 41.46 |
| Qwen3-VL         | 65.93 | 74.53 | 65.43 | 72.18 | 50.96 | 41.94 | 36.67 |
| Doubao seed 1.8  | 69.14 | 74.11 | 69.15 | 71.43 | 64.33 |  50.0 | 51.67 |
| GPT-5.2          | 73.14 | 80.38 |  73.4 | 74.81 | 63.06 | 59.68 | 46.67 |
| Claude4.5 Sonnet | 75.79 | 84.55 | 73.94 | 76.32 | 67.52 | 64.52 | 48.33 |
| T5-small         | 36.14 | 53.01 | 33.16 | 35.23 | 22.86 |  7.81 |  3.23 |
| BART-base        |  54.0 | 78.62 | 53.63 | 51.14 | 27.43 | 15.62 |  4.84 |
| Inter-GPS        |  60.5 | 81.07 | 60.62 | 60.98 | 38.86 | 20.31 | 11.29 |
| DualGeoSolver    | 62.11 | 63.64 |  68.1 | 65.94 |  58.0 | 60.98 | 31.71 |
| NGS              |  62.6 | 62.88 | 65.64 | 71.01 |  57.0 | 60.98 | 36.59 |
| FGeo-DRL         | 80.85 | 97.63 | 92.11 | 70.89 |  61.6 | 39.39 | 31.37 |
| FGeo-TP          | 80.86 | 96.43 | 85.44 | 76.12 | 62.26 | 48.88 | 29.55 |
| FGeo-ISRL        | 85.16 | 95.12 |  93.3 | 82.26 | 73.24 |  61.9 |  35.2 |
| HyperGNet        | 88.36 | 96.44 | 92.49 | 91.29 | 78.29 | 57.81 | 51.61 |
| NSS              | 89.63 | 97.35 | 91.69 | 92.79 |  84.0 | 68.42 | 42.55 |
| Pri-TPG          | 89.29 | 99.16 | 96.28 | 87.92 | 77.07 | 66.13 |  30.0 |
| Ours             | 96.06 | 99.74 | 98.72 | 94.59 |  96.0 | 91.23 |  61.7 |

### Results on Geometry3K, GeoQA and FormalGeo7K

| Method        | Geometry3K | GeoQA | FormalGeo7K |
|---------------|-----------:|------:|------------:|
| E-GPS         |      90.40 |     - |           - |
| DualGeoSolver |          - | 65.20 |           - |
| FGeo-ISRL     |          - |     - |       85.16 |
| HyperGNet     |      91.99 | 85.64 |       88.36 |
| Pri-TPG       |      95.16 | 85.02 |       89.29 |
| Ours          |      98.11 | 94.64 |       96.06 |

### Ablation study

| Method                | Total |    L1 |    L2 |    L3 |    L4 |    L5 |    L6 |
|-----------------------|------:|------:|------:|------:|------:|------:|------:|
| RAVS                  | 96.06 | 99.74 | 98.72 | 94.59 |  96.0 | 91.23 |  61.7 |
| w/o Bidirectional     |  92.8 | 98.68 | 97.12 | 92.79 |  92.0 | 75.44 | 40.43 |
| w/o Reflection        | 93.83 | 99.21 | 97.76 | 93.24 | 93.33 | 78.95 | 46.81 |
| w/o Second-pass Retry | 90.57 | 97.62 | 96.49 | 91.44 | 83.33 | 70.18 |  38.3 |

## Citation

coming soon...