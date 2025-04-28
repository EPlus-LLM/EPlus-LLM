# EPlus-LLM

<div align="center" style="line-height: 1;">

[![GitHub Stars](https://img.shields.io/github/stars/Gangjiang1/EPlus-LLM?style=social)](https://github.com/Gangjiang1/EPlus-LLM)
[![GitHub Forks](https://img.shields.io/github/forks/Gangjiang1/EPlus-LLM?style=social)](https://github.com/Gangjiang1/EPlus-LLM/network/members)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

[English](./README.md) | [中文版](./README_zh.md)

</div>


**EPlus-LLM 系列：通过大语言模型（LLM）实现建筑能耗建模自动化**

**_EPlus-LLM [v1](https://huggingface.co/EPlus-LLM/EPlus-LLMv1)/[v2](https://huggingface.co/EPlus-LLM/EPlus-LLMv2) 模型及快速上手代码 [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Gangjiang1/EPlus-LLM/blob/main/v2/EPlus-LLMv2_inference.ipynb) 已在 [Huggingface](https://huggingface.co/EPlus-LLM) 开源_**

<div align="center">
  <img src="/figs/graphic.png" alt="Illustration of EPlus-LLMv2 for Auto-building energy modeling" width="700"/>
</div>


## 🎉 最新动态
- 📄 [2025/04/18] (update #4): 基于 EPlus-LLMv2 平台的论文已被 _Automation in Construction_ 接收发表
[论文链接](https://doi.org/10.1016/j.autcon.2025.106223)。
- ⚡️ [2025/01/15] (update #3): EPlus-LLMv2 发布，成功扩展到复杂场景下的自动建筑能耗建模（ABEM）。新版本支持真实建筑应用中多种建模需求，显著增强了适应性和灵活性。通过大规模数据集和大语言模型，结合 LoRA、混合精度训练与模型量化技术，大幅降低计算开销，实现高效微调（性能无损）。
[论文即将发布](https://doi.org/10.1016/j.apenergy.2024.123431)。
- 📄 [2025/01/14] (update #2): 关于使用提示工程（prompt engineering）指导大语言模型实现自动建筑能耗建模的论文已被 _Energy_ 接收。
[论文链接](https://doi.org/10.1016/j.energy.2025.134548)。
- 🔥 [2024/05/016] (update #1): 我们首次实现了使用大语言模型，基于自然语言自动生成建筑能耗模型。
[论文链接](https://doi.org/10.1016/j.apenergy.2024.123431)。

## 🚀 核心特点与功能
- 可扩展性：自动生成复杂 EnergyPlus 模型，包括建筑几何、材料、热区、小时级别的运行时间表等。
- 精准高效：建模准确率 100%，手动建模时间减少超过 98%。
- 交互与自动化：提供友好的人机交互界面，实现无缝建模与定制。

<div align="center">
  <img src="/figs/v2_paltform.png" alt="Description" width="600"/>
  <p><em>A user-friendly human-AI interface for EPlus-LLMv2.</em></p>
</div>

- 灵活的设计场景支持：

  ✅ 几何形状：正方形、L型、T型、U型、中空方形建筑等
  ✅ 屋顶类型：平屋顶、人字屋顶、四坡屋顶，可自定义阁楼/脊高
  ✅ 朝向与窗户：可自定义窗墙比、窗户分布、各立面的特殊设计
  ✅ 墙体与材料：热工性能、绝热层类型
  ✅ 内部负荷：照明、设备、人员密度、渗透/通风率、时刻表安排、冷热设定点
  ✅ 热区划分：支持核心区与周边区等多热区布局

<div align="center">
  <img src="/figs/v2_prompt-model.png" alt="Prompt-Model Description" width="600"/>
  <p><em>The relationship between the prompt and the model.</em></p>
</div>

## 🏗️ 目标用户

本平台面向建筑性能、可持续发展和韧性设计领域的工程师、建筑师及研究人员，尤其适用于概念设计阶段，助力实现最佳建模决策。但不限于此。

<div align="center">
  <img src="/figs/v2_example1.png" alt="Examples of EPlus-LLMv2" width="600"/>
  <p><em>EXample scenarios of EPlus-LLMv2.</em></p>
</div>

## 🚀 快速开始

以下提供一段代码示例，展示如何快速加载 EPlus-LLM 并自动生成建筑能耗模型。 
  
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Gangjiang1/EPlus-LLM/blob/main/v2/EPlus-LLMv2_inference.ipynb)

```python
# ⚠️ Please make sure you have adequate GPU memory.
# ⚠️ Please make sure your EnergyPlus version is 9.6 for successful running.

# ! pip install -U bitsandbytes -q
from transformers import AutoModelForSeq2SeqLM, AutoTokenizer
import torch
from peft import PeftModel, PeftConfig

# Load the rest port of IDF file.
file_path = "v2_nextpart.idf"
output_path = "v2_final.idf"

# Load the EPlus-LLMv2 config. 
peft_model_id = "EPlus-LLM/EPlus-LLMv2"
config = PeftConfig.from_pretrained(peft_model_id)

# Load the base LLM, flan-t5-xxl, and tokenizer
model = AutoModelForSeq2SeqLM.from_pretrained("google/flan-t5-xxl", load_in_8bit=True)
tokenizer = AutoTokenizer.from_pretrained("google/flan-t5-xxl")

# Load the Lora model
model = PeftModel.from_pretrained(model, peft_model_id)

# Generation config
generation_config = model.generation_config
generation_config.max_new_tokens = 5000
generation_config.temperature = 0.1
generation_config.top_p = 0.1
generation_config.num_return_sequences = 1
generation_config.pad_token_id = tokenizer.eos_token_id
generation_config.eos_token_id = tokenizer.eos_token_id

# Please provide your input here — a description of the desired building
# For more details, please refer to the paper: https://doi.org/10.1016/j.autcon.2025.106223
input=f"""
Simulate a U-shaped building that is 99.73 meters high, with a gable roof.
The horizontal segment is 732.31 meters long and 17.54 meters wide.
The left vertical segment is 256.31 meters long and 206.96 meters wide.
The right vertical segment is 431.54 meters long and 62 meters wide.
The roof ridge is 8.77 meters to the length side of the horizontal segment, and 128.16 meters, 215.77 meters to the width side of the vertical segments, respectively.
The attic height is 139.71 meters. The building orientation is 62 degrees to the north.
The building has 3 thermal zones with each segment as one thermal zone.
The window-to-wall ratio is 0.32. The window sill height is 33.91 meters, the window height is 65.82 meters, and the window jamb width is 0.01 meters.
The window U-factor is 6.36 W/m2K and the SHGC is 0.89.
The wall is made of wood, with a thickness of 0.48 meters and the wall insulation is RSI 1.6 m2K/W, U-factor 0.63 W/m2K.
The roof is made of metal, with a thickness of 0.09 meters and the roof insulation is RSI 5.4 m2K/W, U-factor 0.19 W/m2K.
The floor is made of concrete, covered with carpet. The ventilation rate is 2.32 ach. The infiltration rate is 0.55 ach.
The people density is 16.61 m2/person, the light density is 4.48 W/m2, and the electric equipment density is 22.63 W/m2.
Occupancy starts at 7:00 and ends at 18:00. The occupancy rate is 1. The unoccupancy rate is 0.3.
The heating setpoint is 21.54 Celsius in occupancy period and 15.86 Celsius in unoccupancy period.
The cooling setpoint is 22.6 Celsius in occupancy period and 26.72 Celsius in unoccupancy period.
"""
input_ids = tokenizer(input, return_tensors="pt", truncation=False)
generated_ids = model.generate(input_ids = input_ids.input_ids,
                           attention_mask = input_ids.attention_mask,
                           generation_config = generation_config)
generated_output = tokenizer.decode(generated_ids[0], skip_special_tokens=True)
# Output the building energy model in IDF file
with open(file_path, 'r', encoding='utf-8') as file:
    nextpart = file.read()
final_text = nextpart + "\n\n" + generated_output
with open(output_path, 'w', encoding='utf-8') as f:
    f.write(final_text)
print(f"Building Energy Model Auto-Generated: {output_path}")
```

## 📝 引用

如果本项目对你的研究有所帮助，请引用以下论文：

```
@article{jiang2025EPlus-LLMv2,
  author    = {Gang Jiang and Jianli Chen},
  title     = {Efficient fine-tuning of large language models for automated building energy modeling in complex cases},
  journal   = {Automation in Construction},
  volume    = {175},
  pages     = {106223},
  year      = {2025},
  month     = {July},
  doi       = {https://doi.org/10.1016/j.autcon.2025.106223}}

@article{jiang2025prompting,
  author    = {Gang Jiang and Zhihao Ma and Liang Zhang and Jianli Chen},
  title     = {Prompt engineering to inform large language models in automated building energy modeling},
  journal   = {Energy},
  volume    = {316},
  pages     = {134548},
  year      = {2025},
  month     = {Feb},
  doi       = {https://doi.org/10.1016/j.energy.2025.134548}}

@article{jiang2025EPlus-LLM,
  author    = {Gang Jiang and Zhihao Ma and Liang Zhang and Jianli Chen},
  title     = {EPlus-LLM: A large language model-based computing platform for automated building energy modeling},
  journal   = {Applied Energy},
  volume    = {367},
  pages     = {123431},
  year      = {2024},
  month     = {Aug},
  doi       = {https://doi.org/10.1016/j.apenergy.2024.123431}}
```
