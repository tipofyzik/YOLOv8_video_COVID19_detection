# Detecting persons with and without mask on video via YOLOv8 ML model

For viewing big-size notebooks: https://nbviewer.org/

## Datasets
For this project two datasets were used:
1. Face mask detection dataset: https://www.kaggle.com/datasets/andrewmvd/face-mask-detection
2. Custom dataset to work with the mentioned above. It includes **mask_data.yaml** file that is required for working with face mask detection dataset and two images for testing. Just fownload files from **mask_data** folder, create your custom dataset on Kaggle, and add these files there. I named my custom dataset **mask_data**.
3. One more custom dataset is **test-dataset**. It includes all original videos that have been process by the model. Download them, create your own **test-dataset** on Kaggle, and upload these videos. You can access these videos through the following link: 
https://drive.google.com/drive/folders/1NzYsQyrqTmnkYklwg2KuKgTwr7twVWc6

## 1. Introduction
The COVID-19 pandemic dramatically changed our lives, particularly regarding daily interactions. As a result, almost all the countries started developing various protective measures to reduce the spread of the virus. Major measures were keeping the social distance and wearing face masks in public spaces. These protocols aimed to minimize the risk of infection transmission and protect public health.

Tasks:  
This project focuses on creating a computer vision system to monitor and detect violations of COVID-19 protocols, especially indoor. The system uses the cutting-edge deep learning techniques and technologies to achieve the following goals:  
**·** Detect humans in a monocular video sequence. Furthermore, the system should identify if the person wears a face mask.   
**·** The system should estimate an approximate distance between people and decide if this distance is sufficient to meet health protocols.  
**·** The system decides whether there are persons who don't follow the health protocols and gives the result.  

The system is tested on videos with different people densities to evaluate its effectiveness and accuracy. For this, public videos are used and the result is uploaded into a private platform, Google Drive.  
Methodology and tools: the project is implemented using the Python programming language, since it has a vast amount of libraries which provide the opportunity to work with deep learning algorithms and models. To train and test the system, public datasets are used.  

## 2. Project implementation
To make the computer vision system for once needs, we need to define which tasks should this system perform and their complexity. After this we can start to select a model for our case. Detection system performs an object detection task whose goal is to recognize, locate and classify the objects into different classes. There are various deep learning algorithms but for object detection YOLO algorithm is one of the best choices for its performance, accuracy and speed.   
Once we have defined the task for the system and chosen an appropriate model(s) for it, we are required for raw data which our model(s) will be trained on. In most cases, we can use public datasets. For detection of common things, like animals or persons, we can use Common Objects in Context (COCO) dataset. In case of more specific objects, like laptops, we are required for custom data, which can either be found on the internet or collected manually.   

### 2.1. Project goals
The objective of this project is to develop a system capable of detecting people, identifying whether they are wearing face masks, and measuring the distance between individuals. At the output, the system should determine the person(s) in the video who don’t follow the health protocols. Therefore, the system checks for compliance with two major rules: wearing a mask in public places and maintaining a social distance of at least 1 meter (COVID-19: physical distancing, n.d.). Two random persons follow health protocols if they both wear masks even though the distance between them is less than 1 meter. In other cases, we say that the health protocols are violated.  

### 2.2. Model choice, data preparation, and training
The You Only Look Once (YOLO) algorithm was originally designed for object detection and remains one of the leaders in this task. Due to its efficiency, high speed, accuracy, and ease of use, this is often considered as the best choice for real-time object detection tasks. Recently, the 9th version of this algorithm has been released (Wang, Yeh, & Liao, 2024). YOLOv9 loses less information during the training thanks to the new architecture solution (YOLOv9: SOTA Object Detection Model Explained, 2024), which leads to better performance compared with other models (see fig. 1). This allows it to outperform the model's predecessor, YOLOv8, in the context of accuracy and efficiency. For now, this is the best choice for various object detection tasks. For this work, I trained and applied both these YOLO versions to my test videos to look at the practical difference between them. It's important to note that there are several model sizes, each of which is suitable for specific needs. Here, I chose a medium-size model as it’s well-balanced in accuracy and performance and has acceptable process speed.  
![image](https://github.com/user-attachments/assets/c6ca21ca-877d-47a0-87cf-3929287b3b9b)  
For human detection, I applied pretrained YOLO models without additional training on custom datasets, whereas for face masks detection the custom dataset is required. For this reason, I used a publicly available dataset on kaggle to train the YOLO model. The original dataset had annotation in XML-format, so it has been converted into the appropriate format to allow the algorithm process data.
To train the model, the Kaggle platform is used for providing 30 hours of free GPU use, which is pretty enough for a small, individual project like this.  

### 2.3. Results
The detection system works in two stages: first, the system identifies all the persons it can and then it checks whether detected individuals wear face masks or not. If the mask isn't worn or worn incorrectly, (e.g., the mask doesn't cover the nose), the system marks a bounding box around individuals’ faces in red, which means that health protocols aren't met. Otherwise, the bounding box around the face is green. Every detected individual is highlighted with a blue bounding box. This project was majorly performed using YOLOv9, however, since this is the pretty “fresh” model, my interest also lied in comparing this version with its predecessor, YOLOv8.   
The model output videos can be accessed through the following link https://drive.google.com/drive/folders/1MJp1Jj3Aej6LiAFbI4HCDY3IdF6mHL4d?usp=sharing. In total there are seven videos: 2 with model versions comparison and 5 with model outputs.  

#### 2.3.1. Detection results
The model demonstrated good results for individuals that are mostly in the foreground of the video. The further people from the camera, the harder it is to detect them. The very distant objects aren't detected at all. This factor also affected the mask detection because masks can only be found only on recognized persons. Furthermore, there are some errors that occur during the video processing. For example, the model can detect the persons’ reflections in mirrors and windows like real persons (see fig.2). Or it can identify the person properly but can’t recognize the mask on them if this person stands sideways to the camera (see fig.3). Such errors may occur because of the medium-size model, meaning it has average accuracy and learning capacity.  
![image](https://github.com/user-attachments/assets/71aeae34-0168-4ee4-b9b6-eeb9d28faf1f)  

The outputs of the developed detection system can be found at the link above. There are 5 different videos each name of which starts with “output_crowd_video”. If the system marks a person's face with a red bounding box we say that this person doesn’t follow the health protocol. Otherwise, the individual’s face will be marked with a green bounding box, meaning everything is fine.  
In the “output_crowd_video5” video file you can see that the system works almost flawlessly. There are only a few persons in each frame with clearly seen masks on their faces. The system detects the mask wrongly only for a person in the background where the picture is blurry.  

#### 2.3.2. YOLOv8 vs. YOLOv9
The result is quite ambiguous. In some frames YOLOv8 shows itself better than YOLOv9, whereas in other frames the situation is completely opposite. The difference is clearly seen in the quality of mask detection. While the 8th version detects a lot of masks correctly in one frame, the 9th version does this better in the other one. However, it’s important to note that both models work well on average and to see real differences between these versions the bigger tests should be conducted. For example, models could be trained on bigger datasets that can be obtained by augmentation.   
![image](https://github.com/user-attachments/assets/d1ef7359-83e0-4f99-8f6f-1d04a57c109d)  

## 2.4. Challenges and failures
The most challenging task is estimation of approximate distance between people. To do so we are required to know at least intrinsic camera parameters, such as focal length or optical center and object and camera positions in the real world. In other words, we must know extrinsic and intrinsic camera parameters to evaluate the distance. However, since public dataset are used, meaning there are only images/videos and the corresponding annotations to them and no additional information about the camera is provided, we can do nothing with it. Even if we take some average human height it will only help for the persons that are in the same plane in the real world.   
Let’s say we take a person from the foreground. We know the size of his bounding box (from a person detection model) and can state that the height of this box is equal to the average human’s height. We decided to take another person that stands right next to the first one and higher than them. By using simple proportions, we can derive the second person’s height. However, imaging that the higher person stands behind the first one. It means that now his bounding box is smaller, and the system will say that both persons are the same height. Now, imagine if some person is far away from the camera, his height will be estimated as a few centimeters relative to the person from the foreground. Without camera parameters and real-world coordinates we can only rely on bounding boxes that won’t bring us to any result. So, this subtask just can’t be performed without additional information.  

## 2.5. Possible improvements
There are different aspects that can be improved in this project. But improvements require more data, time, and GPU resources to train the model. Let’s say, we want to improve the mask and human detection. For this, we can choose the model with a bigger size like YOLOv9c or YOLOv9e (YOLOv9: A Leap Forward in Object Detection Technology, n.d.) meaning both models have more parameters. However, such an approach requires more data, which can only partially be acquired by using augmentation techniques. To make the YOLO model more accurate on mask detection tasks, we need more images with people in masks in different angles and positions. Furthermore, the bigger the model, the slower the training and the process speed which means that free-of-use resources can be used less amount of times. Therefore, if we choose a bigger model with training on a bigger dataset, we will be required to pay for hardware resources (GPU) and space.  
In this small project, YOLOv8 and YOLOv9 were compared. However, since the dataset for detecting masks is not large enough, the difference is hard to see. In some frames, one version demonstrates performance better than the other, while in other frames the result is the exact opposite. It would seem that YOLOv9 should show better results than its predecessor, YOLOv8, since it outperformed YOLOv8 in accuracy and speed. However, the original dataset is quite small and I think this is the reason we barely see the difference between the models: as before we need more diverse data, maybe with different color palettes. For instance, we can add data with a gradient between black and white or include data featuring people with backgrounds similar to their clothing. As a proof you can look at the “output_crowd_video3.avi” video. You can clearly see that the YOLOv9 model detects people poorly even though they all are close enough to the camera (see fig. 5). A lot of people have a low contrast with the background due to their clothes color and, therefore, they aren’t always recognized.  
![image](https://github.com/user-attachments/assets/f787c309-2637-44c7-823e-72669dccffd0)  

The most struggling subtask in the entire project is the estimation of distance between people, since we have no information about public cameras and other objects. To resolve this issue the following approach might be used.  
We acquire data via cameras with known intrinsic parameters. With focal length and the coordinates of certain objects (some person, for example), we can derive the distance between the camera and this object (Rosebrock, 2015). We can optimize this process using a deep learning model, so the distance will be estimated automatically. Knowing the distances between camera and 2 different objects (segments) and knowing at least approximately angle between them (let's say, a few degrees if objects are far from the camera), we can derive the distance between these objects using the cosine theorem.  
The final suggestion to improve the detection system as a whole is to train models not only for identifying persons but also for tracking them. This will highlight people who don't wear masks or wear them incorrectly for most of the video. Let’s say the system detected some person. If this person doesn't meet the health protocol more than 50% during the video record, the system saves a couple of frames with him in the “violator” folder.  

## 3. Conclusion
In this paper, I explained the development process of a detection system that checks individuals for compliance with health protocols. The analysis showed that it works great for videos where there are approximately up to 10-15 humans. For crowds, where the density is high, people from the foreground are detected well, while far from the camera people aren't recognized at all. Furthermore, contrast proves to be a significant factor, as individuals wearing clothing that blends into their surroundings are less noticeable compared to those whose attire contrasts with the background. Extended datasets and more complex YOLO versions are recommended to improve the detection.  
For recognized humans, mask detection works pretty good: it paints the bounding face box green if the mask is worn properly, meaning that the health protocols are followed. If the mask is worn incorrectly or is absent at all, the health protocols are violated and the bounding face box turns red.  
Furthermore, the comparison of YOLOv8 and YOLOv9 was conducted. It seems that YOLOv9 works quite better than its predecessor, however, the distinction between them is quite hard to see since both versions work well on average. To make it more clear, further investigation is required.  
The subtask of estimation distance between people is turned out to be unsolvable in our conditions. The camera intrinsic parameters, such as focal length, must be provided to make the assessment possible. Once we have these parameters, the suggested approach for estimation can be used.  

## Literature
COVID-19: physical distancing. (n.d.). World Health Organization. https://www.who.int/westernpacific/emergencies/covid-19/information/physical-distancing  
Rosebrock, A., (2015, January). Find distance from camera to object/marker using Python and OpenCV. PyImageSearch. https://pyimagesearch.com/2015/01/19/find-distance-camera-objectmarker-using-python-opencv/  
Wang, C.-Y., Yeh, I.-H., & Liao, H.-Y. M. (2024). _YOLOv9: Learning what you want to learn using programmable gradient information._ https://doi.org/10.48550/arXiv.2402.13616  
_YOLOv9: A Leap Forward in Object Detection Technology._ (2024). Ultralytics Documentation. https://docs.ultralytics.com/models/yolov9/  
_YOLOv9: SOTA Object Detection Model Explained._ (2024, February 23). Ultralytics Documentation. https://encord.com/blog/yolov9-sota-machine-learning-object-dection-model/  
