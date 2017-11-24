# Dense 3D Regression for Hand Pose Estimation

This respository contains python codes of the paper. It is developped and tested on Debian GNU/Linux 8 64-bit.

## Requirements:
- tensorflow == 1.3
- tfplot (for visualization on tf summary files)
- numpy
- opencv >= 2.4 (optional, for cpu visualization) 

## Data Preparations:
Download the datasets, create soft links for them to [exp/data](./exp/data) and run data/$dataset.py to create the TFRecord files. More details are [here](./exp/data).

## Usage:
Both training and testing functions are provided by `model/hourglass\_um\_crop\_tiny.py`. Here is an example:
```bash
python model/hourglass_um_crop_tiny.py --dataset 'icvl' --batch_size 40 --num_stack 2 --fea_num 128 --debug_level 2 --is_traing True
```
where the hyper parameter configuration is explained in the source python files.

## Results:
We provide the estimation results by the proposed method for [ICVL](./exp/result/icvl.txt), [NYU](./exp/result/nyu.txt), [MSRA15](./exp/result/msra.txt). They are xyz coordinates in mm, the 2D projection method is in the function _xyz2uvd_ from [here](data/util.py#L23)

## Pretrained Models:
Run the script to download and install the corresponding datasets. $ROOT denote the root path of this project.
```bash
cd $ROOT
bash ./exp/scripts/fetch_icvl_models.sh
bash ./exp/scripts/fetch_msra_models.sh
bash ./exp/scripts/fetch_nyu_models.sh
```
To perform testing, simply run
```
python model/hourglass_um_crop_tiny.py --dataset 'icvl' --batch_size 3 --num_stack 2 --fea_num 128 --debug_level 2 --is_traing False
python model/hourglass_um_crop_tiny.py --dataset 'nyu' --batch_size 3 --num_stack 2 --fea_num 128 --debug_level 2 --is_traing False
python model/hourglass_um_crop_tiny.py --dataset 'msra' --pid 0 --batch_size 3 --num_stack 2 --fea_num 128 --debug_level 2 --is_traing False
```
in which msra dataset should use pid to indicate which person to test on. In the [testing function](data/hourglass_um_crop_tiny.py#L23), the third augument is used to indicate which model with corresponding training step will be restored. We use step of -1 to indicate our pre-trained model.
