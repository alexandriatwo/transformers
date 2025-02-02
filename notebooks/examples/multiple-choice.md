# Multiple Choice

Based on the script [`run_swag.py`](multiple-choice.md).

## PyTorch script: fine-tuning on SWAG

`run_swag` allows you to fine-tune any model from our [hub](https://huggingface.co/models) \(as long as its architecture as a `ForMultipleChoice` version in the library\) on the SWAG dataset or your own csv/jsonlines files as long as they are structured the same way. To make it works on another dataset, you will need to tweak the `preprocess_function` inside the script.

```bash
python examples/multiple-choice/run_swag.py \
--model_name_or_path roberta-base \
--do_train \
--do_eval \
--learning_rate 5e-5 \
--num_train_epochs 3 \
--output_dir /tmp/swag_base \
--per_gpu_eval_batch_size=16 \
--per_device_train_batch_size=16 \
--overwrite_output
```

Training with the defined hyper-parameters yields the following results:

```text
***** Eval results *****
eval_acc = 0.8338998300509847
eval_loss = 0.44457291918821606
```

## PyTorch version, no Trainer

Based on the script [run\_ner\_no\_trainer.py](https://github.com/huggingface/transformers/blob/master/examples/multiple-choice/run_swag_no_trainer.py).

Like `run_swag.py`, this script allows you to fine-tune any of the models on the [hub](https://huggingface.co/models) \(as long as its architecture as a `ForMultipleChoice` version in the library\) on the SWAG dataset or your own data in a csv or a JSON file. The main difference is that this script exposes the bare training loop, to allow you to quickly experiment and add any customization you would like.

It offers less options than the script with `Trainer` \(but you can easily change the options for the optimizer or the dataloaders directly in the script\) but still run in a distributed setup, on TPU and supports mixed precision by the mean of the [🤗 `Accelerate`](https://github.com/huggingface/accelerate) library. You can use the script normally after installing it:

```bash
pip install accelerate
```

then

```bash
export DATASET_NAME=swag

python run_swag_no_trainer.py \
  --model_name_or_path bert-base-cased \
  --dataset_name $DATASET_NAME \
  --max_seq_length 128 \
  --per_device_train_batch_size 32 \
  --learning_rate 2e-5 \
  --num_train_epochs 3 \
  --output_dir /tmp/$DATASET_NAME/
```

You can then use your usual launchers to run in it in a distributed environment, but the easiest way is to run

```bash
accelerate config
```

and reply to the questions asked. Then

```bash
accelerate test
```

that will check everything is ready for training. Finally, you can launch training with

```bash
export DATASET_NAME=swag

accelerate launch run_swag_no_trainer.py \
  --model_name_or_path bert-base-cased \
  --dataset_name $DATASET_NAME \
  --max_seq_length 128 \
  --per_device_train_batch_size 32 \
  --learning_rate 2e-5 \
  --num_train_epochs 3 \
  --output_dir /tmp/$DATASET_NAME/
```

This command is the same and will work for:

* a CPU-only setup
* a setup with one GPU
* a distributed training with several GPUs \(single or multi node\)
* a training on TPUs

Note that this library is in alpha release so your feedback is more than welcome if you encounter any problem using it.

## Tensorflow

```bash
export SWAG_DIR=/path/to/swag_data_dir
python ./examples/multiple-choice/run_tf_multiple_choice.py \
--task_name swag \
--model_name_or_path bert-base-cased \
--do_train \
--do_eval \
--data_dir $SWAG_DIR \
--learning_rate 5e-5 \
--num_train_epochs 3 \
--max_seq_length 80 \
--output_dir models_bert/swag_base \
--per_gpu_eval_batch_size=16 \
--per_device_train_batch_size=16 \
--logging-dir logs \
--gradient_accumulation_steps 2 \
--overwrite_output
```

