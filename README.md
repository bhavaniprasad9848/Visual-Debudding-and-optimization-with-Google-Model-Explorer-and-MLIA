````markdown
# Visual Debugging and optimization with google Model Explorer and MLIA

An AI model optimization project completed using the Arm Create 2026 hands-on workflow and Arm AI Model Optimizer, built on NetsPresso.

This project optimizes a WideResNet-50 image classifier for the Alif Ensemble E8, targeting its Ethos-U85 NPU through ExecuTorch.

## Results

| Model stage | Memory (MB) | Estimated latency (ms) | Top-1 accuracy (%) | PTE size (MB) |
|---|---:|---:|---:|---:|
| Original model | 2.01 | 477.14 | 73.98 | N/A |
| Graph optimization | 2.01 | 477.14 | 73.98 | N/A |
| INT8 quantization | 1.96 | 350.24 | 72.96 | 42.71 |

The final model achieved:

- **26.6% lower estimated latency**, from 477.14 ms to 350.24 ms.
- **2.5% lower reported memory**, from 2.01 MB to 1.96 MB.
- **72.96% Top-1 accuracy**, a decrease of 1.02 percentage points from the original model.

Latency and memory were estimated using Arm ML Inference Advisor (MLIA). These results were not measured on a physical Alif E8 board. Accuracy was evaluated on the workshop's 162-image evaluation dataset.

## Configuration

| Component | Setting |
|---|---|
| Model | `wideresnet50_cls_14_pt2_pytorch_static_fp32` |
| Target device | `alif_e8_devkit` |
| Runtime | `executorch` |
| Backend | `ethos_u` |
| Quantization scheme | `W8-A8` |
| Weight granularity | `TENSOR` |
| Weight calibration | `MINMAX` |
| Activation calibration | `HISTOGRAM` |
| Calibration dataset | `openimages_filtered_calib_seed42_n98` |
| Evaluation dataset | `openimages_filtered_eval_seed42_n162` |
| Accuracy metric | `TOP1_ACCURACY` |

## Optimization Workflow

1. Created a project using the workshop's preset model and datasets.
2. Configured graph optimization and graph quantization.
3. Applied graph transformations, including operator fusion.
4. Quantized weights and activations from FP32 to INT8.
5. Evaluated accuracy before and after optimization.
6. Estimated latency and memory using MLIA.
7. Exported the optimized ExecuTorch program as `model.pte`.

## Repository Files

| File | Purpose |
|---|---|
| `run.yaml` | Pipeline stages, profiling targets, and accuracy evaluation |
| `run.detail.yaml` | Detailed optimization, calibration, and profiling settings |

The exported model is attached to the GitHub release.

## Download the Model

Download **model.pte** from the [v1.0.0 release](https://github.com/bhavaniprasad9848/wideresnet50-alif-e8-optimization/releases/tag/v1.0.0).

The file is an ExecuTorch program targeting Alif E8. Running it on the board requires compatible firmware and the ExecuTorch runtime.

## Running the Experiment

Reproduction requires Python 3.11, the `arm-ai-model-optimizer` CLI, valid server authentication, and access to the workshop's preset model and datasets.

After creating the corresponding project and experiment, place both configuration files in the experiment directory and run:

```powershell
np validate
np run --profile-method mlia --wait
np report
np export
```

To inspect the model graph in NetsPresso Probe:

```powershell
np visualize auth
np visualize
```

Workshop server access and preset availability may be time-limited.

## Acknowledgments

Based on the Arm Create 2026 hands-on session using Arm AI Model Optimizer, NetsPresso, Alif Ensemble E8, and ExecuTorch.

## Author

[Bhavani Prasad Vemula](https://github.com/bhavaniprasad9848)
````
