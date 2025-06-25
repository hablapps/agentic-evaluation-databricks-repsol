# What Comes After Coding: Evaluating Agentic Behaviour

This repository contains notebooks and example code used in the talk "What Comes After Coding: Evaluating Agentic Behaviour", presented by Alfonso Roa at the Databricks User Group Meetup in Madrid (hosted at the Repsol Campus).

- [Slides of the presentation](https://github.com/hablapps/agentic-evaluation-databricks-repsol/blob/main/Evaluating%20Agentic%20Behaviour%20-%20slides.pdf) (in Spanish).

The notebooks demonstrate how to:

- Build a clickbait detector agent using **LangGraph** and **DSPy**.
- Track and evaluate agentic behavior using **MLflow**, implementing custom
  scorers and judges.

The notebooks focus not only on building agents but also on measuring their correctness, reliability, and alignment—key concerns for production-grade AI systems.

## 🚀 Getting started

### Import the notebook in databricks

To import the notebooks into a Databricks workspace:

1. Copy one of the notebook links

   - [**Clickbait agent - LangGraph.ipynb**](https://raw.githubusercontent.com/hablapps/agentic-evaluation-databricks-repsol/refs/heads/main/Clickbait%20agent%20-%20LangGraph.ipynb)
   - **Clickbait agent - DSPy.ipynb** (_available soon_)
2. In the workspace browser, navigate to the location where you want to import the notebook.
3. Right-click the folder and select **Import** from the menu.
4. Click the **URL** radio button and paste the link you just copied in the field
5. Click **Import**. The notebook will be imported and opened automatically.

More detailed instructions in the [Databricks Docs](https://docs.databricks.com/aws/en/notebooks/).

### Prerequisites 

The notebooks include step-by-step instructions, making it easy to follow along and understand the configuration and usage. This section provides a high-level overview of the key prerequisites needed to run the agent examples successfully.

#### Global parameters

Each notebook includes global parameters for:

- **Unity Catalog location**: defines where models, prompts, and artifacts are stored
- **Model serving endpoint**: used for invoking LLMs via Databricks Model Serving

#### Jina AI tool 

The agents use the [Jina AI Reader API](https://jina.ai/reader/) to extract titles and content from webpages. This functionality enables the agent to process live URLs provided by the user.

- A **free API key** is required, which can optionally be stored securely using **Databricks Secrets**.

#### Clickbait dataset

To evaluate the agent’s performance, a labeled dataset of clickbait vs. non-clickbait headlines is used. You can download the dataset from Kaggle: [clickbait dataset](https://www.kaggle.com/datasets/amananandrai/clickbait-dataset).

## 🧠 Agent variants

- LangGraph version: [Clickbait agent - LangGraph.ipynb](https://github.com/hablapps/agentic-evaluation-databricks-repsol/blob/main/Clickbait%20agent%20-%20LangGraph.ipynb)
- DSPy version : _available soon_

## 📊 Evaluation Techniques

This project uses the new **MLflow 3.x GenAI API**, specifically the evaluate method, to evaluate agentic behavior in a structured and extensible way.

### 🧩 Modular evaluation via functional wrappers

We define lightweight functional wrappers around the full agent or individual nodes (e.g., the classifier or rewriter). This allows evaluating specific parts of the agent independently. These wrappers are passed to the `predict_fn` parameter of the `mlflow.genai.evaluate()` method.

### 🛠 Custom framework-agnostic scorers

Custom scorers are implemented in a way that is agnostic to the underlying agent framework (LangGraph or DSPy). This enables reuse, portability, and clean separation between agent logic and evaluation logic.

### 🧑‍⚖️ Custom judges from internal components

As an example, the clickbait classification logic is reused as a custom judge. After the agent rewrites a clickbait title, the classifier is invoked again to verify whether the new version is still clickbait.

### 🧑‍⚖️ Judges for qualitative feedback

The notebooks also integrate MLflow’s built-in judges to evaluate agent output based on qualitative rules:

- 🛡 Safety — checks for harmful, unethical, or inappropriate content
- 📏 Guidelines — enforces custom constraints defined in natural language

This setup supports unit-like testing, integration-style validation, and property-based evaluation, all within a consistent MLflow workflow.

---
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Creative Commons License" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br />This work is licensed under a <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License</a>.
