```shell
TASK_NAME="cmnli"
CURRENT_DIR=$(cd -P -- "$(dirname -- "$0")" && pwd -P)
export GLUE_DATA_DIR=$CURRENT_DIR/datasets
python run_classifier.py \
      --model_type=bert \
      --model_name_or_path=$MODEL_NAME \
      --task_name=$TASK_NAME \
      --do_train \
      --do_eval \
      --do_lower_case \
      --data_dir=$GLUE_DATA_DIR/${TASK_NAME}/ \
      --max_seq_length=128 \
      --per_gpu_train_batch_size=16 \
      --per_gpu_eval_batch_size=16 \
      --learning_rate=3e-5 \
      --num_train_epochs=3.0 \
      --logging_steps=24487 \
      --save_steps=24487 \
      --output_dir=$CURRENT_DIR/${TASK_NAME}_output/ \
      --overwrite_output_dir \
      --seed=42
```