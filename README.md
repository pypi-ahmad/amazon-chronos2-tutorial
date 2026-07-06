## Amazon Chronos-2: Zero to Mastery (Notebook Tutorial)

Notebook-first, reproducible walkthrough of **Amazon Chronos-2**, a time-series foundation model
released in the `chronos-forecasting` package.

The main artifact is [`Chronos2_Zero_to_Mastery.ipynb`](./Chronos2_Zero_to_Mastery.ipynb). It covers:

- Loading a pretrained Chronos-2 checkpoint and running **zero-shot** forecasts
- Backtesting on a real electricity price dataset
- Using **cross-learning** mode across multiple related series
- Chronos-2’s **multivariate** input formats (arrays and dict-based covariates)
- Optional **LoRA fine-tuning** (opt-in)

Companion notebook:

- [`Chronos2_Multivariate_Jena_Climate.ipynb`](./Chronos2_Multivariate_Jena_Climate.ipynb): Chronos-2 on a **genuinely multivariate**
  real dataset (Jena Climate), including native multivariate targets plus known-future calendar covariates.

This repo is designed to run on **CPU or GPU** (auto-selects CUDA if available). GPU is strongly
recommended for speed.

## Requirements

- Python **>= 3.12**
- [`uv`](https://docs.astral.sh/uv/) (recommended install via Astral docs)
- Internet access (downloads datasets + the `amazon/chronos-2` checkpoint from Hugging Face)

## Setup

```bash
uv sync --dev
```

## Run (Interactive)

```bash
uv run jupyter lab
```

Open `Chronos2_Zero_to_Mastery.ipynb` and run all cells.

## Run (E2E / Non-interactive)

This executes the notebook end-to-end and writes the executed copy to `artifacts/` (the committed
notebook is not modified).

```bash
mkdir -p artifacts
CHRONOS2_TUTORIAL_FAST=1 uv run jupyter nbconvert \
  --to notebook --execute Chronos2_Zero_to_Mastery.ipynb \
  --output-dir artifacts \
  --output Chronos2_Zero_to_Mastery.executed.ipynb \
  --ExecutePreprocessor.timeout=1800
```

Multivariate companion notebook:

```bash
mkdir -p artifacts
CHRONOS2_TUTORIAL_FAST=1 uv run jupyter nbconvert \
  --to notebook --execute Chronos2_Multivariate_Jena_Climate.ipynb \
  --output-dir artifacts \
  --output Chronos2_Multivariate_Jena_Climate.executed.ipynb \
  --ExecutePreprocessor.timeout=1800
```

## Runtime Controls

- `CHRONOS2_TUTORIAL_FAST=1`
  - Reduces backtest windows and cross-learning batch sizes to finish faster (recommended for
    quick verification and CPU runs).
- `CHRONOS2_TUTORIAL_FINETUNE=1`
  - Enables the LoRA fine-tuning section.
  - By default, fine-tuning is skipped unless CUDA is available.

## Notes

- Model and dataset downloads will be cached by your environment (e.g., Hugging Face cache).
- If you hit timeouts or slow execution on CPU, keep `CHRONOS2_TUTORIAL_FAST=1`.

## License

MIT. See [`LICENSE`](./LICENSE).
