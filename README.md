# (PyTorch) Depth from Videos in the Wild

## Introduction to the Project

This project is a Pytorch re-implementation of the following Google ICCV 2019 paper:  
**[Depth from Videos in the Wild: Unsupervised Monocular Depth Learning from Unknown Cameras](https://arxiv.org/abs/1904.04998)**

<p align="center">
  <img src="demo/kitti_0926drive0001_0018.gif" width="600" />
</p>

## Environment Setup
The codes has been executed under: (Python 3.6.5 + Pytoch 1.7.1) and (Python 3.8 + Pytoch 1.5 or Pytoch 1.7.1)

A [conda environment](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-with-commands) is recommended:
```
pip install opencv-python
conda install pytorch==1.7.1 torchvision==0.8.2 torchaudio==0.7.2 -c pytorch
conda install --file requirements.txt
```

## Data Preparation

We provide `gen_data.py` to generate training datasets. There are two types of datasets available:

<details><summary><strong>KITTI</strong></summary>
<p>  
  
#### Download Raw Data   
Please visit the [official website](http://www.cvlibs.net/datasets/kitti/raw_data.php) to download the entire raw KITTI dataset 
and unzip it to a folder named kitti_raw.  
Alternatively, you can also run the follows:
```
./datasets/data_prep/kitti_raw_downloader.sh
```
#### Generate Training Dataset   
```
python gen_data.py \
--dataset_name [kitti_raw_eigen or kitti_raw_stereo] \
--dataset_dir /path/to/kitti_raw \
--save_dir /path/to/save/the/generated_data \
--mask color
```

</p>
</details>

<details><summary><strong>VIDEOs</strong></summary>
<p>  

Training datasets can also be generated from your own videos under the same folder.   
  
*[Optional]* If the camera intrinsics are known, please put the 9 entries of its flattented camera intrinsics in a text file.
```
1344.8 0.0 640.0 0.0 1344.8 360.0 0.0 0.0 1.0
```

Then generate the training dataset by running:
```
python gen_data.py \
--dataset_name video \
--dataset_dir /path/to/your/video_folder \  # please do not use spaces in video names for now
--save_dir /path/to/save/the/generated_data \
--intrinsics /path/to/your/camera_intrinsics_file \ # If not set, default intrinsics are produced according to IPhone 
--mask color
```
  
</p>
</details>

More options for `gen_data.py` can be found in `options/gen_data_options.py`.

## Train Models
```
python train.py --data_path /path/to/save/the/generated_data    # the folder generated by gen_data.py
                --png    # remove this if the training images are generated in jpg format
                --learn_intrinsics
                --weighted_ssim
                --boxify
                --model_name /path/to/save/model    # relative path under the $Project/models folder 
```

## Run Inference
```
python infer_depth.py --input_path path/to/a/folder/containing/images    # or path/to/a/video
                      --model_path path/to/a/folder/containing/checkpoints    # keep the saved folder structure since its parent folder must have opt.json
```
Available pretrained models:
* **KITTI416x128**: [download](https://drive.google.com/file/d/1uj3CNNw5buvxqNIJJmY30_9kRY0HpS9Z/view?usp=sharing)

## Reference
* [Google Research vid2depth](https://github.com/tensorflow/models/tree/37ec31714f68255532b4c35f117bc33fd7f90692/research/vid2depth): we reused its codes to develop gen_data.py
* [Niantic Labs monodepth2](https://github.com/nianticlabs/monodepth2): we followed its code structure and reused some of its auxiliary functions
* [Official Google Tensorflow Repository](https://github.com/google-research/google-research/tree/57b60e7a7a5efc358adf4041a062ae435e6155be/depth_from_video_in_the_wild): we constructed the deeplearning networks and implemented their training based on it. 

## Updates
**(Feb 25th, 2022)** this project was made public!
