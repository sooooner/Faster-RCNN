# Human Detection Model
This is a repo that developed a single human detection model(Faster-RCNN) using data from the [Motion Keypoint Detection AI Contest](https://dacon.io/competitions/official/235701/overview/description) conducted by dacon.

## Description

Project process description 
+ Description  

## Repo Structure

```
.
└── res/
    ├── data                                # Data doesn't go up to the repo.
    └── ...    
└── saved_model/                        
    ├── saved_model                         # Weights doesn't go up to the repo.
    └── ...   
└── utils/      
    └── models/                              
        ├── __init__.py 
        ├── base_model.py                   # Pre-trained backbone(resnet50, resnet101, VGG16)
        ├── classifier.py                   # Faster-RCNN's classifier module  
        ├── frcnn.py                        # Faster-RCNN(Integration of all modules)      
        ├── layers.py                       # Other model layer functions(RoI Pooling, NMS, ...)           
        ├── rpn.py                          # Region Proposal Network module
        └── train_step.py                   # Function for model training in 4 steps
    ├── __init__.py                                
    ├── Augmentation.py                     # Data augmentation(Shift, Flip, Add noise, etc...)  
    ├── intersection_over_union.py          # calculate iou between ground truth and prediction                
    ├── label_generator.py                  # A function that generate the ground truth          
    └── utils.py                            # Other necessary functions
├── .dockerignore                           
├── .gitignore                              
├── Dockerfile                              # A file to create an docker image for development environment
├── requirements.txt                        # Libraries needed to create image
├── config.py                               # model config py
├── human_detection.ipynb                   # Examples of progress 
├── SQL_loader.ipynb                        # MySQL communication example
├── README.md                               
└── train.py                                # model training and save weight py
```

## Usage
### Data
Download the data through this [link](https://dacon.io/competitions/official/235701/overview/description)

### Saved model


### Development environment
You don't need to clone this repo, just run Dockerfile and the development environment to run this repo image will be installed. And just follow the command line described below. (You can just clone and install requirements and use it.)
```terminal
docker build --tag {repo image name}:{tag} .

docker network {network name}
docker run -it --name {volume container name} -v {data path}:/data --network {network name} ubuntu /bin/bash
docker run -it -d --name {mysql name} -p 3306:3306 --network {network name} --volumes-from {volume container name} -e [MYSQL_ALLOW_EMPTY_PASSWORD=ture] mysql
docker run -it -d --name {tensorflow container name} -p 8888:8888 --network {network name} {repo image name}:{tag}
docker run -it -d --name {model server name} -p 8500:8500 -p 8501:8501 --network {network name} -v {saved model path}:/models/{model name} -e MODEL_NAME={model name} tensorflow/serving
```
Then, you can create a container, connect it, and use it.

If you use MySQL, you can modify train.py and utils/db_uploader.py a little and use it.
**Refer to SQL_loader.ipynb**

### Training
Running train.py will train the model and store the trained weights.
However, since it provides a trained model, there is no need to train it separately.
```terminal
python train.py
```

### inference
When model training is complete, insert your image path and run inference.py, you can see the result.
```terminal
python inference.py 
```

## Author
Soon Ho Kwon

github/sooooner  
https://tnsgh0101.medium.com/

## License
Copyright © 2016 Jon Schlinkert Released under the MIT license.

