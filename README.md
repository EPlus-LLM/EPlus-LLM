# EPlus-LLM
**EPlus-LLM series, natural language for auto-building energy modeling via LLM**

<div align="center">
  <img src="/figs/graphic.png" alt="Illustration of EPlus-LLMv2 for Auto-building energy modeling" width="600"/>
</div>


## 🎉 News
- ⚡️ [2025/01/01] (update #2): We release EPlus-LLMv2, successfully addressing the challenge of auto-building energy modeling (ABEM) in complex scenarios. The new version of the platform supports a wide range of modeling scenarios encountered in real-world building applications, significantly enhancing its breadth and flexibility. Based on comprehensive datasets and a large-scale LLM, we integrate techniques such as LoRA, mixed precision training, and model quantification to reduce computational burden and achieve efficient fine-tuning (without compensating performance).
[Paper coming soon](https://doi.org/10.1016/j.apenergy.2024.123431).
- 🔥 [2024/05/016] (update #1): We first successfully implement natural language-based auto-building modeling by fine-tuning a large language model (LLM). 
[Paper here](https://doi.org/10.1016/j.apenergy.2024.123431).

## 🚀 Key Features
- Scalability: Auto-generates complex EnergyPlus models, including varying geometries, materials, thermal zones, hourly schedules, and more.
- Accuracy & Efficiency: Achieves 100% modeling accuracy while reducing manual modeling time by over 98%.
- Interaction & Automation: A user-friendly human-AI interface for seamless model creation and customization.

<div align="center">
  <img src="/figs/v2_paltform.png" alt="Description" width="600"/>
  <p><em>A user-friendly human-AI interface for EPlus-LLMv2.</em></p>
</div>

- Flexible Design Scenarios:

  ✅ Geometry: square, L-, T-, U-, and hollow-square-shaped buildings  
  ✅ Roof types: flat, gable, hip – customizable attic/ridge height  
  ✅ Orientation & windows: custom WWR, window placement, facade-specific controls  
  ✅ Walls & materials: thermal properties, insulation types  
  ✅ Internal loads: lighting, equipment, occupancy, infiltration/ventilation, schedules, heating/cooling setpoints  
  ✅ Thermal zoning: configurable multi-zone layouts with core & perimeter zones  

<div align="center">
  <img src="/figs/v2_prompt-model.png" alt="Prompt-Model Description" width="600"/>
  <p><em>The relationship between the prompt and the model.</em></p>
</div>

## 🏗️ Target Users
This current platform is designed for engineers, architects, and researchers working in building performance, sustainability, and resilience. It is especially useful during early-stage conceptual design when modeling decisions have the greatest impact.
<div align="center">
  <img src="/figs/v2_example1.png" alt="Examples of EPlus-LLMv2" width="600"/>
  <p><em>EXample scenarios of EPlus-LLMv2.</em></p>
</div>

## 🚀 Quick Start

This repository contains v2 and v1 of EPlus-LLM, along with implementation details for the ABEM reference.


📂 Repository Structure

```
    ── README.md                           # Project documentation
    ── v2                                  # V2 model for complex ABEM scenarios in real-world
    ── v1                                  # V1 model for simple ABEM scenarios
    ── requirements.txt                    # Dependencies for this project
```

🔧 Installation

- Clone the repository:
```
    git clone https://github.com/Gangjiang1/EPlus-LLM.git
    cd EPlus-LLM
```
- Install required dependencies:
```
    pip install -r requirements.txt
```

▶️ Running Auto-Building Energy Modeling via EPlus-LLM
```
    cd v2
    python EPlus-LLM/v2/Inference.py
```
