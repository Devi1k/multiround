1. 执行mkdir.sh
2. python run_classifier.py
```shell
TASK_NAME="cmnli"
CURRENT_DIR=$(cd -P -- "$(dirname -- "$0")" && pwd -P)
export GLUE_DATA_DIR=$CURRENT_DIR/datasets
# download and unzip dataset
if [ ! -d $GLUE_DATA_DIR ]; then
  mkdir -p $GLUE_DATA_DIR
  echo "makedir $GLUE_DATA_DIR"
fi
cd $GLUE_DATA_DIR
if [ ! -d $TASK_NAME ]; then
  mkdir $TASK_NAME
  echo "makedir $GLUE_DATA_DIR/$TASK_NAME"
fi
cd $TASK_NAME
if [ ! -f "train.json" ] || [ ! -f "dev.json" ] || [ ! -f "test.json" ]; then
  rm *
  wget https://storage.googleapis.com/cluebenchmark/tasks/cmnli_public.zip
  unzip cmnli_public.zip
  rm cmnli_public.zip
else
  echo "data exists"
fi
echo "Finish download dataset."

# make output dir
if [ ! -d $CURRENT_DIR/${TASK_NAME}_output ]; then
  mkdir -p $CURRENT_DIR/${TASK_NAME}_output
  echo "makedir $CURRENT_DIR/${TASK_NAME}_output"
fi
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