# 🃏 Riddler

### Local LLM Security Fuzzing & Adversarial Prompt Testing

Riddler is a locally-oriented LLM security testing framework designed to evaluate how language models respond to adversarial prompt transformations and jailbreak attempts.

It provides a controlled environment for experimenting with different attack strategies against locally hosted models through **Ollama**, allowing security researchers and students to study LLM robustness without requiring external model APIs.

---

## 🎯 Why Riddler?

Modern LLM applications can be exposed to prompt injection, jailbreak attempts, adversarial prompting, and other forms of instruction manipulation.

Riddler helps investigate these behaviors by:

* Generating adversarial variations of user prompts
* Testing multiple attack strategies
* Running models locally through Ollama
* Classifying model responses
* Recording whether an attack appears successful
* Providing reproducible results for security research

The goal is not simply to generate malicious prompts, but to understand **why and when an LLM's safety behavior fails**.

---

## 🧠 Architecture

```text
                         ┌─────────────────────┐
                         │    Target Prompt    │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │      Riddler        │
                         │   Attack Engine     │
                         └──────────┬──────────┘
                                    │
                ┌───────────────────┼───────────────────┐
                │                   │                   │
                ▼                   ▼                   ▼
          History Framing      Taxonomy           Genetic
             (HST)              (TAX)              (GEN)
                │                   │                   │
                └───────────────────┼───────────────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │       Ollama        │
                         │    Local LLM        │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │     Response        │
                         │    Classifier       │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │  Jailbreak Result   │
                         └─────────────────────┘
```

---

# ⚔️ Attack Modes

Riddler supports multiple adversarial prompting techniques.

| Mode  | Name                 | Purpose                                                                  |
| ----- | -------------------- | ------------------------------------------------------------------------ |
| `hst` | History Framing      | Reframes a request using historical/contextual narratives                |
| `tax` | Taxonomy Paraphraser | Transforms prompts using taxonomy-based paraphrasing                     |
| `gen` | Genetic              | Iteratively mutates prompts to search for effective adversarial variants |

These techniques allow the same underlying request to be evaluated from different adversarial perspectives.

---

# 🖥️ Local LLM Support

Riddler has been configured to work with **Ollama**, allowing security testing against locally hosted models.

Example models:

```text
qwen2.5:3b
qwen2.5-coder:3b
```

This allows experiments to be performed without sending prompts or responses to an external LLM provider.

### Example Architecture

```text
Riddler
   │
   ▼
Ollama
   │
   ├── qwen2.5:3b
   │
   └── qwen2.5-coder:3b
```

---

# 🚀 Installation

### Requirements

* Python 3.10+
* Poetry
* Ollama
* Git
* Linux / WSL recommended for local development

Clone the repository:

```bash
git clone https://github.com/WHENEVERSTOP24/Riddler_LLM_Secuirty.git
cd Riddler_LLM_Secuirty
```

Install dependencies:

```bash
poetry install
```

Activate the environment:

```bash
poetry shell
```

---

# 🦙 Configure Ollama

Make sure Ollama is running before starting an attack.

Check installed models:

```bash
ollama list
```

Example:

```text
NAME                    SIZE
qwen2.5:3b              1.9 GB
qwen2.5-coder:3b        1.9 GB
```

For WSL environments, configure the Ollama host appropriately so that the WSL environment can communicate with the Windows Ollama service.

---

# 🧪 Basic Usage

A simple local fuzzing test can be executed with:

```bash
poetry run riddler fuzz \
  -m ollama/qwen2.5-coder:3b \
  -a hst \
  -c har \
  -t "Explain cybersecurity best practices for a beginner."
```

For configuration-driven experiments:

```bash
poetry run riddler fuzz \
  -C local_config.example.json
```

---

# 📊 Example Result

Riddler produces structured results similar to:

```text
┌───────────────────────────────┬──────────────────────┬───────────────┐
│ Prompt                        │ Model                │ Attack        │
├───────────────────────────────┼──────────────────────┼───────────────┤
│ Example security question     │ qwen2.5-coder:3b     │ HST           │
└───────────────────────────────┴──────────────────────┴───────────────┘

Adversarial Prompt:
    [generated adversarial variation]

Response:
    [model response]

Jailbreak:
    ❌
```

A successful result indicates that the generated adversarial prompt caused the target model to produce a response that the configured classifier considered to violate the expected safety behavior.

---

# 🔬 Research Applications

Riddler can be used for:

### LLM Red Teaming

Evaluate the robustness of locally deployed language models against adversarial prompting.

### Jailbreak Research

Study how different prompt transformations influence model safety behavior.

### Model Comparison

Run the same attack strategies against different local models.

```text
                 ┌──────────────┐
                 │ Target Prompt│
                 └───────┬──────┘
                         │
                ┌────────┴────────┐
                ▼                 ▼
          Qwen 2.5 3B      Qwen 2.5 Coder
                │                 │
                ▼                 ▼
             Results           Results
                │                 │
                └────────┬────────┘
                         ▼
                    Comparison
```

### Security Education

Riddler can be used as a practical environment for understanding LLM security concepts and adversarial prompt engineering.

---

# 🛠️ Riddler Customizations

This repository contains modifications made to the original FuzzyAI project, including:

* Renamed CLI interface to **Riddler**
* Custom Riddler terminal banner
* Local Ollama-oriented configuration
* Qwen 2.5 Coder as the default taxonomy model for History Framing
* WSL → Windows Ollama connectivity support
* Local configuration template
* Riddler-specific project metadata

The Python package structure remains compatible with the original project architecture.

---

# 🔐 Security & Responsible Use

Riddler is intended for:

* Authorized security testing
* LLM robustness research
* Security education
* Controlled red-team experiments
* Testing models that you own or have permission to evaluate

Do not use Riddler to bypass safety controls on systems or services without authorization.

---

# 📁 Project Structure

```text
Riddler_LLM_Secuirty/
│
├── src/
│   └── fuzzyai/
│       ├── attacks/
│       ├── classifiers/
│       ├── handlers/
│       ├── llm/
│       │   └── providers/
│       │       └── ollama/
│       ├── cli.py
│       └── ...
│
├── tests/
├── resources/
├── local_config.example.json
├── pyproject.toml
├── README.md
└── .gitignore
```

---

# 🧰 Technology Stack

| Technology | Purpose                             |
| ---------- | ----------------------------------- |
| Python     | Core framework                      |
| Poetry     | Dependency management               |
| Ollama     | Local LLM inference                 |
| Qwen 2.5   | Target language model               |
| MongoDB    | Experiment/result storage           |
| AsyncIO    | Concurrent attack execution         |
| WSL        | Local Linux development environment |

---

# 📌 Roadmap

* [x] Local Ollama integration
* [x] Qwen 2.5 support
* [x] History Framing attack
* [x] Taxonomy attack
* [x] Genetic attack
* [x] Local classification
* [x] Riddler CLI branding
* [ ] Improved experiment dashboard
* [ ] Automated model comparison
* [ ] Attack success analytics
* [ ] Research-oriented reporting
* [ ] Additional local classifiers
* [ ] Expanded Ollama model support

---

# 📜 Attribution

Riddler is a customized/adapted version of **CyberArk FuzzyAI**.

Original project:

https://github.com/cyberark/FuzzyAI

The original project's licensing and attribution requirements remain applicable to the portions derived from the upstream project.

Riddler-specific modifications and research work are maintained in this repository.

---

# 👨‍💻 Author

**Anubhav Rajput**

Computer Science — Cybersecurity

Focus areas:

* LLM Security
* AI Red Teaming
* Security Automation
* SOC Engineering
* Offensive Security

GitHub:

https://github.com/WHENEVERSTOP24

---

## ⭐ Riddler

> **Break the prompt. Understand the model. Secure the system.**
