<link rel="stylesheet" href="assets/css/custom.css">

# ChartMind <img src="images/chartMind.png" width="32" height="32" style="vertical-align: middle;">: Benchmark and Deconstruction for <br> Multimodal Chart Reasoning

**<span style="color: #2174A8;">Tong Li</span><sup>†</sup>, <span style="color: #2174A8;">Guodao Sun</span><sup>†</sup>, <span style="color: #2174A8;">Shunkai Wang</span><sup>†</sup>, <span style="color: #2174A8;">Zuoyu Tang</span><sup>†</sup>, <span style="color: #2174A8;">Yang Shu</span><sup>†</sup>, <span style="color: #2174A8;">Xueqian Zheng</span><sup>†</sup>, <span style="color: #2174A8;">Zhentao Zheng</span><sup>†</sup>, <br> <span style="color: #2174A8;">Qi Jiang</span><sup>‡</sup>, <span style="color: #2174A8;">Haixia Wang</span><sup>†</sup>, <span style="color: #2174A8;">Ronghua Liang</span><sup>‡</sup>**

<sup>†</sup>Zhejiang University of Technology <sup>‡</sup>Zhejiang University of Science and Technology 

<div style="text-align: center; margin: 30px 0;">
  <a href="#" style="background-color: #2174A8;
color: white; padding: 8px 16px; margin: 0 8px; text-decoration: none; border-radius: 5px; display: inline-block;">📑 Paper</a>
  <a href="#" style="background-color: #2174A8;
color: white; padding: 8px 16px; margin: 0 8px; text-decoration: none; border-radius: 5px; display: inline-block;">⭐ Code</a>
  <a href="https://huggingface.co/datasets/guodaosun/Mega60k" style="background-color: #2174A8;
color: white; padding: 8px 16px; margin: 0 8px; text-decoration: none; border-radius: 5px; display: inline-block;">🧱 Dataset</a>
</div>

<div style="text-align: center;
margin: 40px 0;">
<img src="images/cover.png" alt="ChartMind Overview" style="max-width: 85%; height: auto;">
</div>

## Abstract
Existing MLLMs chart reasoning evaluations are limited to single visual inputs and “black-box” metrics, lacking “white-box” analysis of thinking processes and reasoning patterns. To alleviate this, we present Mega60K, a benchmark covering 21 chart types and 11 question-answer tasks, enriched with reasoning traces. We further introduce the reasoning deconstruction framework to quantify multimodal activation strategies. Additionally, we conduct the reasoning paradigm transfer experiment to explore logic alignment via reasoning supervision. Evaluations across 12 representative MLLMs under three reasoning settings—visual-only, multimodal fusion, and multimodal compensation—yield the following insights: High-level reasoning tasks (e.g., multi-step logic, pattern recognition, layout optimization) serve as gold-standard for distinguishing MLLMs; Naive modality stacking offers limited reasoning gains, while structured modalities yield measurable compensatory effects in quantitative tasks under visual degradation. Reasoning paradigm transfer improves the student model (Qwen2.5-VL-7B) by 18.77% in reasoning accuracy and enables alignment with the teacher model’s (Gemini 2.5 Flash) reasoning style.

## Mega60k Dataset Overview 📊

### 21 Chart Types
<div style="text-align: center; margin: 30px 0;">
  <img src="images/charts.png" alt="21 Chart Types in ChartMind" style="max-width: 100%; height: auto;">
</div>

### 11 Question Tasks
- **Visual Understanding (4 Tasks)** 👁️ - chart type recognition (CTR), visual element counting (VEC), spatial relation perception (SRP), and visual pattern recognition (VPR)
- **Numerical Analysis (5 Tasks)** 🔢 - numerical extraction (NE), extremum value judgment (EVJ), statistical computation (SC), numerical filtering (NF), and numerical comparison (NC)
- **Logical Reasoning (2 Tasks)** 💡 - multi-step reasoning (MSR) and visual analysis (VA)

## ChartMind Evaluation Space 📈

### Evaluation Metrics
We employ six key metrics to comprehensively assess model performance:

- **TightAcc (T<sub>acc</sub>):** Exact keyword matching between model answers and ground truth for strict factual accuracy assessment.
- **RelaxAcc (R<sub>acc</sub>):** Numerical accuracy with 5% error tolerance to account for unit conversion and precision variations.
- **MixAcc (M<sub>acc</sub>):** Multiplicative combination of TightAcc and RelaxAcc for answers containing both textual and numerical elements.
- **Inference Time:** Time cost (in seconds) from input reception to answer generation.
- **Reasoning Tokens:** Token count in the model's explicit reasoning trace, reflecting thinking expansion degree.
- **Reasoning Drift:** Semantic deviation from question-answering context, measured using Sentence-BERT similarity between reasoning units and task context.

### Benchmarking Configurations
To evaluate the multimodal reasoning capabilities of MLLMs, we design three experimental configurations:

- **Visual (Baseline):** Chart image + question → answer + reasoning 
- **Fusion:** Chart image + SVG + question → answer + reasoning  
- **Compensation:** Degraded chart image + SVG + question → answer + reasoning

<div style="overflow: auto; max-width: 100%; max-height: 600px;">
<table class="tg" style="undefined;table-layout: fixed; width: 1500px"><colgroup>
<col style="width: 164px">
<col style="width: 98px">
<col style="width: 101px">
<col style="width: 101px">
<col style="width: 101px">
<col style="width: 101px">
<col style="width: 101px">
<col style="width: 101px">
<col style="width: 101px">
<col style="width: 101px">
<col style="width: 101px">
<col style="width: 101px">
<col style="width: 101px">
<col style="width: 101px">
</colgroup>
<thead>
  <tr>
    <th class="tg-larh" rowspan="2">MLLMs</th>
    <th class="tg-larh" rowspan="2">Eaperiment</th>
    <th class="tg-larh">CTR</th>
    <th class="tg-larh">VEC</th>
    <th class="tg-larh">SRP</th>
    <th class="tg-larh">VPR</th>
    <th class="tg-larh">VE</th>
    <th class="tg-larh">EVJ</th>
    <th class="tg-larh">SC</th>
    <th class="tg-larh">NF</th>
    <th class="tg-larh">NC</th>
    <th class="tg-larh">MSR</th>
    <th class="tg-larh">VA</th>
    <th class="tg-larh" rowspan="2">Average</th>
  </tr>
  <tr>
    <th class="tg-larh"><em>T<sub>acc</sub></em></th>
    <th class="tg-larh"><em>R<sub>acc</sub></em></th>
    <th class="tg-larh"><em>T<sub>acc</sub></em></th>
    <th class="tg-larh"><em>T<sub>acc</sub>/R<sub>acc</sub></em></th>
    <th class="tg-larh"><em>T<sub>acc</sub>/R<sub>acc</sub></em></th>
    <th class="tg-larh"><em>T<sub>acc</sub>/R<sub>acc</sub></em></th>
    <th class="tg-larh"><em>R<sub>acc</sub></em></th>
    <th class="tg-larh"><em>M<sub>acc</sub></em></th>
    <th class="tg-larh"><em>T<sub>acc</sub>/R<sub>acc</sub></em></th>
    <th class="tg-larh"><em>T<sub>acc</sub>/R<sub>acc</sub></em></th>
    <th class="tg-larh"><em>T<sub>acc</sub>/R<sub>acc</sub></em></th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-larh" colspan="14">open-source models</td>
  </tr>
  <tr>
    <td class="tg-vml0">CogVLM2</td>
    <td class="tg-vml0">visual</td>
    <td class="tg-49iy">5.80%</td>
    <td class="tg-49iy">15.30%</td>
    <td class="tg-49iy">16.20%</td>
    <td class="tg-49iy">7.30%</td>
    <td class="tg-49iy">7.00%</td>
    <td class="tg-49iy">5.20%</td>
    <td class="tg-49iy">2.90%</td>
    <td class="tg-49iy">4.50%</td>
    <td class="tg-49iy">35.90%</td>
    <td class="tg-49iy">5.80%</td>
    <td class="tg-49iy">11.40%</td>
    <td class="tg-49iy">10.66%</td>
  </tr>
  <tr>
    <td class="tg-vml0">LLaVA 1.5</td>
    <td class="tg-vml0">visual</td>
    <td class="tg-49iy">9.70%</td>
    <td class="tg-49iy">49.70%</td>
    <td class="tg-49iy">13.60%</td>
    <td class="tg-49iy">2.30%</td>
    <td class="tg-49iy">2.00%</td>
    <td class="tg-49iy">1.80%</td>
    <td class="tg-49iy">2.10%</td>
    <td class="tg-49iy">2.70%</td>
    <td class="tg-49iy">37.70%</td>
    <td class="tg-49iy">3.90%</td>
    <td class="tg-49iy">7.40%</td>
    <td class="tg-49iy">12.08%</td>
  </tr>
  <tr>
    <td class="tg-vml0">DeepSeek-VL2 </td>
    <td class="tg-vml0">visual</td>
    <td class="tg-49iy">85.80%</td>
    <td class="tg-49iy">74.80%</td>
    <td class="tg-49iy">52.90%</td>
    <td class="tg-49iy">31.00%</td>
    <td class="tg-49iy">40.50%</td>
    <td class="tg-49iy">32.70%</td>
    <td class="tg-49iy">11.90%</td>
    <td class="tg-49iy">16.00%</td>
    <td class="tg-49iy">49.30%</td>
    <td class="tg-49iy">19.20%</td>
    <td class="tg-49iy">28.30%</td>
    <td class="tg-49iy">40.22%</td>
  </tr>
  <tr>
    <td class="tg-vml0" rowspan="3">InternVL3</td>
    <td class="tg-vml0">visual</td>
    <td class="tg-49iy">75.90%</td>
    <td class="tg-49iy">84.10%</td>
    <td class="tg-49iy">57.10%</td>
    <td class="tg-49iy">29.59%</td>
    <td class="tg-49iy">58.10%</td>
    <td class="tg-49iy">45.70%</td>
    <td class="tg-49iy">35.40%</td>
    <td class="tg-49iy">21.50%</td>
    <td class="tg-49iy">64.60%</td>
    <td class="tg-49iy">22.60%</td>
    <td class="tg-49iy">27.80%</td>
    <td class="tg-49iy">47.49%</td>
  </tr>
  <tr>
    <td class="tg-vml0">fusion</td>
    <td class="tg-49iy">76.50%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">75.60%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">22.80%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">4.00%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">10.10%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">5.10%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">40.70%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">18.50%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">49.50%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">4.10%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">1.50%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">28.04%<span style="color: red;">↓</span></td>
  </tr>
  <tr>
    <td class="tg-vml0">compensation</td>
    <td class="tg-49iy">88.10%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">88.00%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">50.00%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">23.3%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">75.20%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">48.00%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">55.80%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">17.10%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">63.00%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">8.10%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">45.50%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">51.10%<span style="color: green;">↑</span></td>
  </tr>
  <tr>
    <td class="tg-vml0" rowspan="3">LLaMA4 Maverick</td>
    <td class="tg-vml0">visual</td>
    <td class="tg-49iy">100.00%</td>
    <td class="tg-49iy">84.90%</td>
    <td class="tg-49iy">73.60%</td>
    <td class="tg-49iy">46.30%</td>
    <td class="tg-49iy">56.80%</td>
    <td class="tg-49iy">49.70%</td>
    <td class="tg-49iy">47.50%</td>
    <td class="tg-49iy">28.40%</td>
    <td class="tg-49iy">71.60%</td>
    <td class="tg-49iy">39.10%</td>
    <td class="tg-49iy">38.80%</td>
    <td class="tg-49iy">57.88%</td>
  </tr>
  <tr>
    <td class="tg-vml0">fusion</td>
    <td class="tg-49iy">100.00%</td>
    <td class="tg-49iy">84.40%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">48.80%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">23.90%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">25.50%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">19.50%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">22.40%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">10.70%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">56.80%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">19.70%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">19.90%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">39.24%<span style="color: red;">↓</span></td>
  </tr>
  <tr>
    <td class="tg-vml0">compensation</td>
    <td class="tg-49iy">99.90%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">86.30%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">63.30%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">36.70%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">41.70%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">38.90%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">32.60%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">18.80%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">62.30%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">30.40%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">22.40%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">48.48%<span style="color: red;">↓</span></td>
  </tr>
  <tr>
    <td class="tg-vml0" rowspan="3">Qwen2.5-VL-32B</td>
    <td class="tg-vml0">visual</td>
    <td class="tg-49iy">99.10%</td>
    <td class="tg-49iy">84.90%</td>
    <td class="tg-49iy">69.60%</td>
    <td class="tg-49iy">40.60%</td>
    <td class="tg-49iy">53.90%</td>
    <td class="tg-49iy">45.10%</td>
    <td class="tg-49iy">37.20%</td>
    <td class="tg-49iy">24.40%</td>
    <td class="tg-49iy">64.60%</td>
    <td class="tg-49iy">36.10%</td>
    <td class="tg-49iy">36.40%</td>
    <td class="tg-49iy">53.81%</td>
  </tr>
  <tr>
    <td class="tg-vml0">fusion</td>
    <td class="tg-49iy">99.10%</td>
    <td class="tg-49iy">84.90%</td>
    <td class="tg-49iy">68.30%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">38.40%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">48.80%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">43.50%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">33.80%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">21.70%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">63.10%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">32.90%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">34.30%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">51.71%<span style="color: red;">↓</span></td>
  </tr>
  <tr>
    <td class="tg-vml0">compensation</td>
    <td class="tg-49iy">99.10%</td>
    <td class="tg-49iy">81.90%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">57.60%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">27.70%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">37.30%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">33.60%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">25.70%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">14.20%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">56.70%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">25.70%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">29.40%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">44.45%<span style="color: red;">↓</span></td>
  </tr>
  <tr>
    <td class="tg-vml0" rowspan="3">Qwen2.5-VL-72B</td>
    <td class="tg-vml0">visual</td>
    <td class="tg-49iy">99.80%</td>
    <td class="tg-49iy">85.00%</td>
    <td class="tg-49iy">69.50%</td>
    <td class="tg-49iy">39.80%</td>
    <td class="tg-49iy">58.50%</td>
    <td class="tg-49iy">47.00%</td>
    <td class="tg-49iy">43.30%</td>
    <td class="tg-49iy">23.80%</td>
    <td class="tg-49iy">67.80%</td>
    <td class="tg-49iy">37.30%</td>
    <td class="tg-49iy">34.30%</td>
    <td class="tg-49iy">55.10%</td>
  </tr>
  <tr>
    <td class="tg-vml0">fusion</td>
    <td class="tg-49iy">99.40%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">86.70%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">66.30%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">36.10%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">52.30%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">44.50%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">37.80%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">20.80%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">64.60%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">32.80%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">34.60%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">52.35%<span style="color: red;">↓</span></td>
  </tr>
  <tr>
    <td class="tg-vml0">compensation</td>
    <td class="tg-49iy">99.00%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">84.20%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">57.80%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">26.90%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">38.00%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">35.70%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">27.90%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">13.40%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">58.00%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">25.10%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">28.50%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">44.95%<span style="color: red;">↓</span></td>
  </tr>
  <tr>
    <td class="tg-larh" colspan="14">closed-source models</td>
  </tr>
  <tr>
    <td class="tg-vml0" rowspan="3">Claude 3.5 Haiku</td>
    <td class="tg-vml0">visual</td>
    <td class="tg-49iy">100.00%</td>
    <td class="tg-49iy">85.20%</td>
    <td class="tg-49iy">67.10%</td>
    <td class="tg-49iy">40.00%</td>
    <td class="tg-49iy">53.40%</td>
    <td class="tg-49iy">46.50%</td>
    <td class="tg-49iy">33.90%</td>
    <td class="tg-49iy">23.00%</td>
    <td class="tg-49iy">65.60%</td>
    <td class="tg-49iy">32.30%</td>
    <td class="tg-49iy">32.50%</td>
    <td class="tg-49iy">52.68%</td>
  </tr>
  <tr>
    <td class="tg-vml0">fusion</td>
    <td class="tg-49iy">100.00%</td>
    <td class="tg-49iy">87.80%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">65.10%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">38.50%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">51.30%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">45.30%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">31.40%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">22.50%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">62.80%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">33.00%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">29.90%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">51.60%<span style="color: red;">↓</span></td>
  </tr>
  <tr>
    <td class="tg-vml0">compensation</td>
    <td class="tg-49iy">99.60%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">91.00%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">57.90%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">27.20%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">37.60%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">40.10%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">22.60%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">18.20%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">56.40%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">28.90%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">24.00%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">45.77%<span style="color: red;">↓</span></td>
  </tr>
  <tr>
    <td class="tg-vml0" rowspan="3">GLM-4V-Plus</td>
    <td class="tg-vml0">visual</td>
    <td class="tg-49iy">99.90%</td>
    <td class="tg-49iy">85.50%</td>
    <td class="tg-49iy">73.70%</td>
    <td class="tg-49iy">47.00%</td>
    <td class="tg-49iy">63.80%</td>
    <td class="tg-49iy">53.30%</td>
    <td class="tg-49iy">45.00%</td>
    <td class="tg-49iy">27.30%</td>
    <td class="tg-49iy">76.20%</td>
    <td class="tg-49iy">42.30%</td>
    <td class="tg-49iy">40.20%</td>
    <td class="tg-49iy">59.47%</td>
  </tr>
  <tr>
    <td class="tg-vml0">fusion</td>
    <td class="tg-49iy">99.70%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">81.40%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">62.20%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">38.10%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">46.50%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">42.80%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">33.90%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">18.70%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">68.30%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">32.40%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">35.90%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">50.90%<span style="color: red;">↓</span></td>
  </tr>
  <tr>
    <td class="tg-vml0">compensation</td>
    <td class="tg-49iy">99.30%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">80.60%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">54.50%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">31.10%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">38.40%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">35.00%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">28.30%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">16.30%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">63.10%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">28.10%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">27.30%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">45.64%<span style="color: red;">↓</span></td>
  </tr>
  <tr>
    <td class="tg-vml0" rowspan="3">Doubao 1.5   Vision-Pro</td>
    <td class="tg-vml0">visual</td>
    <td class="tg-49iy">100.00%</td>
    <td class="tg-49iy">86.00%</td>
    <td class="tg-49iy">75.60%</td>
    <td class="tg-49iy">44.20%</td>
    <td class="tg-49iy">63.10%</td>
    <td class="tg-49iy">48.80%</td>
    <td class="tg-49iy">41.30%</td>
    <td class="tg-49iy">35.20%</td>
    <td class="tg-49iy">72.60%</td>
    <td class="tg-49iy">42.20%</td>
    <td class="tg-49iy">46.30%</td>
    <td class="tg-49iy">59.57%</td>
  </tr>
  <tr>
    <td class="tg-vml0">fusion</td>
    <td class="tg-49iy">100.00%</td>
    <td class="tg-49iy">87.20%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">73.10%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">44.90%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">60.30%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">51.30%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">38.70%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">34.60%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">71.50%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">36.90%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">43.90%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">58.40%<span style="color: red;">↓</span></td>
  </tr>
  <tr>
    <td class="tg-vml0">compensation</td>
    <td class="tg-49iy">99.7%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">85.80%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">61.50%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">32.10%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">42.50%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">38.70%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">23.70%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">21.50%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">58.80%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">23.70%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">22.50%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">46.41%<span style="color: red;">↓</span></td>
  </tr>
  <tr>
    <td class="tg-vml0" rowspan="3">GPT-4o</td>
    <td class="tg-vml0">visual</td>
    <td class="tg-49iy">100.00%</td>
    <td class="tg-49iy">89.40%</td>
    <td class="tg-49iy">80.90%</td>
    <td class="tg-49iy">55.10%</td>
    <td class="tg-49iy">77.80%</td>
    <td class="tg-49iy">59.20%</td>
    <td class="tg-49iy">61.00%</td>
    <td class="tg-49iy">41.70%</td>
    <td class="tg-49iy">77.10%</td>
    <td class="tg-49iy">52.40%</td>
    <td class="tg-49iy">44.90%</td>
    <td class="tg-49iy">67.23%</td>
  </tr>
  <tr>
    <td class="tg-vml0">fusion</td>
    <td class="tg-49iy">100.00%</td>
    <td class="tg-49iy">90.50%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">80.10%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">57.49%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">70.70%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">55.00%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">56.60%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">39.10%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">74.80%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">54.50%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">35.20%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">64.91%<span style="color: red;">↓</span></td>
  </tr>
  <tr>
    <td class="tg-vml0">compensation</td>
    <td class="tg-49iy">100.00%</td>
    <td class="tg-49iy">87.60%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">70.60%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">45.50%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">52.90%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">47.40%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">44.90%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">31.90%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">67.20%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">38.10%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">22.30%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">55.31%<span style="color: red;">↓</span></td>
  </tr>
  <tr>
    <td class="tg-vml0" rowspan="3">Gemini 2.5 Flash</td>
    <td class="tg-vml0">visual</td>
    <td class="tg-49iy">99.50%</td>
    <td class="tg-49iy">88.60%</td>
    <td class="tg-49iy">81.40%</td>
    <td class="tg-49iy">53.90%</td>
    <td class="tg-49iy">67.90%</td>
    <td class="tg-49iy">56.90%</td>
    <td class="tg-49iy">56.50%</td>
    <td class="tg-49iy">44.20%</td>
    <td class="tg-49iy">82.10%</td>
    <td class="tg-49iy">58.10%</td>
    <td class="tg-49iy">45.80%</td>
    <td class="tg-49iy">66.81%</td>
  </tr>
  <tr>
    <td class="tg-vml0">fusion</td>
    <td class="tg-49iy">99.60%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">95.20%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">84.00%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">58.40%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">73.00%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">61.40%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">64.90%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">52.60%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">84.30%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">65.80%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">46.70%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">71.45%<span style="color: green;">↑</span></td>
  </tr>
  <tr>
    <td class="tg-vml0">compensation</td>
    <td class="tg-49iy">99.90%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">94.90%<span style="color: green;">↑</span></td>
    <td class="tg-49iy">75.10%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">51.50%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">61.80%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">58.30%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">55.20%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">43.20%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">75.60%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">55.40%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">32.60%<span style="color: red;">↓</span></td>
    <td class="tg-49iy">63.95%<span style="color: red;">↓</span></td>
  </tr>
</tbody></table>
</div>

### Degradation and Compensation Reasoning
This section presents fine-grained degradation and compensation question-answering statistics. Performance changes exceeding 5% are highlighted.

<div style="overflow: auto;
max-width: 100%; max-height: 600px;">
<table class="tg" style="undefined;table-layout: fixed; width: 1500px"><colgroup>
<col style="width: 164px">
<col style="width: 121px">
<col style="width: 121px">
<col style="width: 121px">
<col style="width: 121px">
<col style="width: 121px">
<col style="width: 121px">
<col style="width: 121px">
<col style="width: 121px">
</colgroup>
<thead>
  <tr>
    <th class="tg-49iy" rowspan="2">MLLMs</th>
    <th class="tg-49iy" colspan="5">Structural-level</th>
    <th class="tg-49iy" colspan="2">Pixel-level</th>
    <th class="tg-49iy" rowspan="2">Average</th>
  </tr>
  <tr>
    <th class="tg-49iy">data mark omission</th>
    <th class="tg-49iy">occlusion</th>
    <th class="tg-49iy">label omission</th>
    <th class="tg-49iy">axis omission</th>
    <th class="tg-49iy">legend omission</th>
    <th class="tg-49iy">blurring</th>
    <th class="tg-49iy">rotation</th>
  </tr></thead>
<tbody>
  <tr>
    <td class="tg-vml0" rowspan="2">Qwen2.5-VL-72B</td>
    <td class="tg-49iy">42.53%</td>
    <td class="tg-49iy">53.65%</td>
    <td class="tg-49iy">46.24%</td>
    <td class="tg-49iy">46.65%</td>
    <td class="tg-49iy">50.80%</td>
    <td class="tg-49iy">44.12%</td>
    <td class="tg-49iy">46.13%</td>
    <td class="tg-49iy">47.09%</td>
  </tr>
  <tr>
    <td class="tg-49iy">43.66%</td>
    <td class="tg-49iy">56.73%</td>
    <td class="tg-49iy">58.95%<span style="color: green;">↑12.71%</span></td>
    <td class="tg-49iy">41.45%<span style="color: red;">↓5.20%</span></td>
    <td class="tg-49iy">48.97%</td>
    <td class="tg-49iy">45.39%</td>
    <td class="tg-49iy">45.29%</td>
    <td class="tg-49iy">47.73%</td>
  </tr>
  <tr>
    <td class="tg-vml0" rowspan="2">GPT-4o</td>
    <td class="tg-49iy">49.37%</td>
    <td class="tg-49iy">59.21%</td>
    <td class="tg-49iy">53.11%</td>
    <td class="tg-49iy">54.62%</td>
    <td class="tg-49iy">60.07%</td>
    <td class="tg-49iy">55.23%</td>
    <td class="tg-49iy">48.40%</td>
    <td class="tg-49iy">54.33%</td>
  </tr>
  <tr>
    <td class="tg-49iy">52.87%</td>
    <td class="tg-49iy">68.10%<span style="color: green;">↑8.89%</span></td>
    <td class="tg-49iy">71.72%<span style="color: green;">↑18.61%</span></td>
    <td class="tg-49iy">55.51%</td>
    <td class="tg-49iy">65.23%<span style="color: green;">↑5.16%</span></td>
    <td class="tg-49iy">59.59%</td>
    <td class="tg-49iy">52.51%</td>
    <td class="tg-49iy">60.13%<span style="color: green;">↑5.80%</span></td>
  </tr>
  <tr>
    <td class="tg-vml0" rowspan="2">Gemini 2.5 Flash</td>
    <td class="tg-49iy">44.92%</td>
    <td class="tg-49iy">59.11%</td>
    <td class="tg-49iy">43.77%</td>
    <td class="tg-49iy">52.61%</td>
    <td class="tg-49iy">57.62%</td>
    <td class="tg-49iy">58.48%</td>
    <td class="tg-49iy">55.41%</td>
    <td class="tg-49iy">53.07%</td>
  </tr>
  <tr>
    <td class="tg-49iy">65.50%<span style="color: green;">↑20.58%</span></td>
    <td class="tg-49iy">81.44%<span style="color: green;">↑22.34%</span></td>
    <td class="tg-49iy">83.62%<span style="color: green;">↑39.85%</span></td>
    <td class="tg-49iy">56.43%</td>
    <td class="tg-49iy">64.71%<span style="color: green;">↑7.09%</span></td>
    <td class="tg-49iy">68.68%<span style="color: green;">↑10.20%</span></td>
    <td class="tg-49iy">64.90%<span style="color: green;">↑9.48%</span></td>
    <td class="tg-49iy">67.71%<span style="color: green;">↑14.65%</span></td>
  </tr>
  <tr>
    <td class="tg-vml0" rowspan="2">Average</td>
    <td class="tg-49iy">45.61%</td>
    <td class="tg-49iy">57.32%</td>
    <td class="tg-49iy">47.71%</td>
    <td class="tg-49iy">51.29%</td>
    <td class="tg-49iy">56.17%</td>
    <td class="tg-49iy">52.61%</td>
    <td class="tg-49iy">49.98%</td>
    <td class="tg-49iy"></td>
  </tr>
  <tr>
    <td class="tg-49iy">54.01%<span style="color: green;">↑8.40%</span></td>
    <td class="tg-49iy">68.76%<span style="color: green;">↑11.44%</span></td>
    <td class="tg-49iy">71.43%<span style="color: green;">↑23.72%</span></td>
    <td class="tg-49iy">51.13%</td>
    <td class="tg-49iy">59.64%</td>
    <td class="tg-49iy">57.88%<span style="color: green;">↑5.28%</span></td>
    <td class="tg-49iy">54.23%</td>
    <td class="tg-49iy"></td>
  </tr>
</tbody></table>
</div>

<div style="text-align: center;
margin: 40px 0;">
<img src="images/degraded_type.png" alt="CV Overview" style="max-width: 85%; height: auto;">
</div>

### Reasoning Instances: Qualitative Analysis 🔎

To supplement the quantitative metrics, we conduct a **qualitative analysis** of explicit reasoning traces. We contrast successful alignment examples with failure cases to illustrate the spectrum of MLLM reasoning capabilities and pinpoint common modes of error, such as semantic drift and hallucination.

#### Positive Reasoning Instances
<div style="text-align: center; margin: 40px 0;">
<img src="images/positivate_examples.png" alt="Successful reasoning traces demonstrating correct logical alignment and evidence utilization." style="max-width: 85%; height: auto;">
</div>

#### Negative Reasoning Instances
<div style="text-align: center; margin: 40px 0;">
<img src="images/negative_examples.png" alt="Negative reasoning traces illustrating semantic drift and hallucination." style="max-width: 85%; height: auto;">
</div>

## Reasoning Deconstruction, Analysis and Transfer

### Reasoning Experiment
For deconstructor selection, we compared five representative models covering two mainstream architectures: traditional neural network models (DistilBERT, RoBERTa) and large language models (Llama-4-Scout-17B, Qwen3-32B, DeepSeek-R1).

* **传统模型 (Traditional Models):** 🤖
    Modeled as **text classification problems** (e.g., 4-class classification for reasoning unit parsing, multi-label classification for chart element parsing).

* **大语言模型 (LLMs):** ✨
    Utilize a **prompt-driven deconstruction approach** combined with the **Test-Time Scaling (TTS) strategy** (performing 5 rounds of sampling with Self-Consistency aggregation).

The experimental results (Table 1) show that traditional models excel in computational efficiency ($time < 0.5s$, no API cost) but exhibit lower deconstruction accuracy ($\mathcal{U}_{\text{Acc}}$ only up to $63.04\%$). In contrast, the three LLMs show superior accuracy ($\mathcal{U}_{\text{Acc}} > 72\%$, $\mathcal{E}_{\text{Acc}} \approx 80\%$). **DeepSeek-R1-Distill-Qwen-32B** is ultimately selected as the formal deconstructor due to its best cost-performance ratio.

<div style="overflow: auto; max-width: 100%; max-height: 600px;">
<table style="table-layout: fixed; width: 100%; border-collapse: collapse;">
<thead>
  <tr style="border-top: 2px solid #333; border-bottom: 1px solid #ddd;">
    <th style="padding: 8px; text-align: center; font-weight: bold; border-right: 1px solid #ddd;" colspan="2" rowspan="2">Methods</th>
    <th style="padding: 8px; text-align: center; font-weight: bold; border-right: 1px solid #ddd;" colspan="5">Evaluation Metrics</th>
    <th style="padding: 8px; text-align: center; font-weight: bold;" colspan="2">TTS Metrics</th>
  </tr>
  <tr style="border-bottom: 2px solid #333;">
    <th style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">$\mathcal{U}_{\text{Acc}}$</th>
    <th style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">$\mathcal{E}_{\text{Acc}}$</th>
    <th style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">time (s)</th>
    <th style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">$\mathcal{M}_\Delta$ (MB)</th>
    <th style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">tokens (k)</th>
    <th style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">Pass@1</th>
    <th style="padding: 8px; text-align: center;">Cons@5</th>
  </tr>
</thead>
<tbody>
  <tr style="border-top: 1px solid #ddd;">
    <td style="padding: 8px; font-weight: bold; border-right: 1px solid #ddd;" rowspan="2">Traditional</td>
    <td style="padding: 8px; border-right: 1px solid #ddd;">DistilBERT</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">16.35%</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">66.95%</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">0.22</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">0.63</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">--</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">--</td>
    <td style="padding: 8px; text-align: center;">--</td>
  </tr>
  <tr style="border-bottom: 1px solid #eee;">
    <td style="padding: 8px; border-right: 1px solid #ddd;">RoBERTa</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">63.04%</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">60.40%</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">0.45</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">1.18</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">--</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">--</td>
    <td style="padding: 8px; text-align: center;">--</td>
  </tr>
  <tr style="border-top: 1px solid #ddd;">
    <td style="padding: 8px; font-weight: bold; border-right: 1px solid #ddd;" rowspan="3">LLMs</td>
    <td style="padding: 8px; border-right: 1px solid #ddd;">Llama-4-Scout-17B-16E-Instruct</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">72.44%</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">78.13%</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">10.25</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">--</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">2.77</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">0.780</td>
    <td style="padding: 8px; text-align: center;">0.780</td>
  </tr>
  <tr>
    <td style="padding: 8px; border-right: 1px solid #ddd;">Qwen3-32B</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">72.47%</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">79.65%</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">14.91</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">--</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">2.84</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">0.784</td>
    <td style="padding: 8px; text-align: center;">0.783</td>
  </tr>
  <tr style="border-bottom: 2px solid #333;">
    <td style="padding: 8px; border-right: 1px solid #ddd;">DeepSeek-R1-Distill-Qwen-32B (adopted)</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">72.14%</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">80.69%</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">14.23</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">--</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">2.84</td>
    <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">0.790</td>
    <td style="padding: 8px; text-align: center;">0.784</td>
  </tr>
</tbody>
</table>
</div>

<div style="text-align: center; margin: 40px 0;">
<img src="images/reasoning_unit_type.png" alt="CV Overview" style="max-width: 85%; height: auto;">
</div>

### Reasoning Paradigm Transfer

This section investigates **Reasoning Paradigm Transfer** by validating the efficacy of high-quality reasoning traces as a supervisory signal.

We designate **Gemini 2.5 Flash** (based on its superior performance) as the **Teacher Model**. We then perform supervised fine-tuning on **Qwen2.5-VL-7B** (the **Student Model**) using the **Low-Rank Adaptation (LoRA)** strategy. This aims to align the student model's logical style with the teacher's, enhancing its problem-solving capabilities.

The table below summarizes the substantial performance gains achieved by the student model across tasks and chart types after fine-tuning.

<div style="overflow: auto; max-width: 100%;">
<table style="width: 100%; border-collapse: collapse; font-size: 14px;">
<thead>
    <tr style="border-top: 2px solid #333; border-bottom: 1px solid #ddd;">
        <th style="padding: 8px; text-align: left; border-right: 1px solid #ddd;" rowspan="2">Chart Type</th>
        <th style="padding: 8px; text-align: center; border-right: 2px solid #333; font-weight: bold;" colspan="5">Qwen2.5-VL-7B (Pre-trained)</th>
        <th style="padding: 8px; text-align: center; font-weight: bold;" colspan="5">Qwen2.5-VL-7B (LoRA Fine-tuned)</th>
    </tr>
    <tr style="border-bottom: 2px solid #333;">
        <th style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">SRP</th>
        <th style="border-right: 1px solid #ddd;">EVJ</th>
        <th style="border-right: 1px solid #ddd;">SC</th>
        <th style="border-right: 1px solid #ddd;">MSR</th>
        <th style="border-right: 2px solid #333;">VA</th> <th style="border-right: 1px solid #ddd;">SRP</th>
        <th style="border-right: 1px solid #ddd;">EVJ</th>
        <th style="border-right: 1px solid #ddd;">SC</th>
        <th style="border-right: 1px solid #ddd;">MSR</th>
        <th>VA</th>
    </tr>
</thead>
<tbody>
    <tr>
        <td style="padding: 8px; border-right: 1px solid #ddd;">bar</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">90.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">100.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">85.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">70.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 2px solid #333;">90.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">90.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">100.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">85.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">65.00%</td>
        <td style="padding: 8px; text-align: center;">100.00%</td>
    </tr>
    <tr>
        <td style="padding: 8px; border-right: 1px solid #ddd;">box</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">100.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">75.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">35.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">40.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 2px solid #333;">-</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">100.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">80.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">40.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">55.00%</td>
        <td style="padding: 8px; text-align: center;">-</td>
    </tr>
    <tr>
        <td style="padding: 8px; border-right: 1px solid #ddd;">bubble</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">35.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">50.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">25.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">45.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 2px solid #333;">-</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">75.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">75.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">45.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">50.00%</td>
        <td style="padding: 8px; text-align: center;">-</td>
    </tr>
    <tr>
        <td style="padding: 8px; border-right: 1px solid #ddd;">line</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">70.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">85.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">70.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">80.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 2px solid #333;">75.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">95.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">95.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">75.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">80.00%</td>
        <td style="padding: 8px; text-align: center;">85.00%</td>
    </tr>
    <tr>
        <td style="padding: 8px; border-right: 1px solid #ddd;">node-link</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">-</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">-</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">5.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">60.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 2px solid #333;">-</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">-</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">-</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">75.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">85.00%</td>
        <td style="padding: 8px; text-align: center;">-</td>
    </tr>
    <tr>
        <td style="padding: 8px; border-right: 1px solid #ddd;">pie</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">35.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">100.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">65.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">25.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 2px solid #333;">-</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">70.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">100.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">75.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">45.00%</td>
        <td style="padding: 8px; text-align: center;">-</td>
    </tr>
    <tr>
        <td style="padding: 8px; border-right: 1px solid #ddd;">sankey</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">80.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">85.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">30.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">65.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 2px solid #333;">-</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">100.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">80.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">60.00%</td>
        <td style="padding: 8px; text-align: center; border-right: 1px solid #ddd;">90.00%</td>
        <td style="padding: 8px; text-align: center;">-</td>
    </tr>
    <tr>
        <td style="padding: 8px; border-bottom: 2px solid #333; border-right: 1px solid #ddd;">scatter</td>
        <td style="padding: 8px; text-align: center; border-bottom: 2px solid #333; border-right: 1px solid #ddd;">-</td>
        <td style="padding: 8px; text-align: center; border-bottom: 2px solid #333; border-right: 1px solid #ddd;">90.00%</td>
        <td style="padding: 8px; text-align: center; border-bottom: 2px solid #333; border-right: 1px solid #ddd;">65.00%</td>
        <td style="padding: 8px; text-align: center; border-bottom: 2px solid #333; border-right: 1px solid #ddd;">45.00%</td>
        <td style="padding: 8px; text-align: center; border-bottom: 2px solid #333; border-right: 2px solid #333;">55.00%</td>
        <td style="padding: 8px; text-align: center; border-bottom: 2px solid #333; border-right: 1px solid #ddd;">-</td>
        <td style="padding: 8px; text-align: center; border-bottom: 2px solid #333; border-right: 1px solid #ddd;">95.00%</td>
        <td style="padding: 8px; text-align: center; border-bottom: 2px solid #333; border-right: 1px solid #ddd;">85.00%</td>
        <td style="padding: 8px; text-align: center; border-bottom: 2px solid #333; border-right: 1px solid #ddd;">45.00%</td>
        <td style="padding: 8px; text-align: center; border-bottom: 2px solid #333;">75.00%</td>
    </tr>
    <tr>
        <td style="padding: 8px; font-weight: bold; border-right: 1px solid #ddd;">Average</td>
        <td style="padding: 8px; font-weight: bold; text-align: center; border-right: 1px solid #ddd;">68.33%</td>
        <td style="padding: 8px; font-weight: bold; text-align: center; border-right: 1px solid #ddd;">83.57%</td>
        <td style="padding: 8px; font-weight: bold; text-align: center; border-right: 1px solid #ddd;">47.50%</td>
        <td style="padding: 8px; font-weight: bold; text-align: center; border-right: 1px solid #ddd;">53.75%</td>
        <td style="padding: 8px; font-weight: bold; text-align: center; border-right: 2px solid #333;">73.33%</td>
        <td style="padding: 8px; font-weight: bold; text-align: center; border-right: 1px solid #ddd;">88.33%</td>
        <td style="padding: 8px; font-weight: bold; text-align: center; border-right: 1px solid #ddd;">89.29%</td>
        <td style="padding: 8px; font-weight: bold; text-align: center; border-right: 1px solid #ddd;">67.50%*</td>
        <td style="padding: 8px; font-weight: bold; text-align: center; border-right: 1px solid #ddd;">64.38%*</td>
        <td style="padding: 8px; font-weight: bold; text-align: center;">86.67%</td>
    </tr>
</tbody>
</table>
</div>


## Citation

If you find this work useful for your research, please cite our paper:
```bibtex
@article{ChartMind2025li,
    title={ChartMind: Benchmark and Deconstruction for Multimodal Chart Reasoning},
    author={Tong Li, Guodao Sun, Shunkai Wang, Zuoyu Tang, Yang Shu, Xueqian Zheng, Zhentao Zheng, Qi Jiang, Haixia Wang and Ronghua Liang},
    year={2025}
}
