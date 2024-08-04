# FastDup
A powerful tool for improving datasets.
![image](https://github.com/user-attachments/assets/43d82b11-6398-485c-a71d-465a9273c0dd)

One of the most important things specifically in neural network and generally in data science, is having a good and reliable dataset. Without that, it's almost impossible to get good results regardless to the target.
*FastDup* is a powerful tool which allows analysis, improvement and better understanding a given dataset. 

In the sequel, we will see some of the features one can use when working with FastDup.
We will provide some examples when running this tool on the RAVIR eye retina dataset.

# Few words about RAVIR dataset
RAVIR is a unique dataset of the retina, which supposed to help for multiple health issues diagnosis, such as diabease, cancer and many more.
The retina, is the only place in the body which allows examining the veins and arteries directly without any invasive action.  
Veins and arteries, can help with early diagnosis which can be very important is some cases (such as cancer). 
The main purpose of RAVIR dataset, is performing classification between veins and arteries, which can lead to better diagnosis. The classification is done as semantic segmentation.
![image](https://github.com/user-attachments/assets/42729cb0-273b-4c28-beea-08898d40dce0)

# FastDUP features
Semantic segmentation of the retina, between veins and arteries is a challenging task, so the better the images' quality, the results of the task should be better.
Using FastDup, we can easily examine our dataset and get important information about it. 
FastDup data analysis, can include: 
  ● Identifying duplicates and near-duplicates.
  
  ● Detecting anomalies and outliers.
  
  ● Clustering similar images or videos.


*For Example, we can get information (and visualization) about images that exist in our dataset.*

![image](https://github.com/user-attachments/assets/c3ea078e-2353-4117-a703-c20459065fed)


*And also about our dataset in a more general perspective:*

![image](https://github.com/user-attachments/assets/a53a057f-db20-4a42-a2ef-b04acef5fcfd)


# Applying FstDup on RAVIR
* Identifying duplicates and near-duplicates:
The command used for getting the above information is: 
fd.similarity()
Which can display a table showing information about similarities between images. In the table, one can see RAVIR has quite a few similar images.
![image](https://github.com/user-attachments/assets/ef07c173-0d30-4874-8952-48f2ad841685)

* Visualization:
![image](https://github.com/user-attachments/assets/484a634d-fb56-48bf-81a6-7ff968ac8b00)


Observing those image with the naked eye, the similar images can be easily distinguish, which leads to the conclusion - a pre-process should be applied on these images (This way the distance between them will be larger).

* Detecting anomalies and outliers.
The command used for getting the above information is: 
stats_df = fd.outliers()

The anomaly information is determind by the distance variable which can be seen in the next table:
![image](https://github.com/user-attachments/assets/20f388ef-b065-4833-a29c-174f9f69e104)

Outliers are important and should be addressed carefully, otherwise, they can mislead the model and causing it to classify wrongly.
We can see that running the outliers analysis on RAVIR test images shows one outlier.
Outliers could cause because of poor image quality and noise. We will see in the sequel how to deal with it.

* Visualization:
![image](https://github.com/user-attachments/assets/38212fcb-f17a-483c-8048-e03a5dd72ab0)

* Clustering similar images or videos.
Clustering and grouping images on RAVIR is pretty similar to the duplicates and near duplicates task and it does not provide any interesting information.
Despite that, maybe in the future, it could have very important role for datasets like RAVIR.
For example, grouping similar retina images, can help detect and diagnose some health issues.

* Visualization:
![image](https://github.com/user-attachments/assets/10e4891f-cf9e-4bb6-9b8e-d33e9b56dd46)

* Other information options:
FastDup can provide also information about dark, bright and blur images.
Let's have a look on the blur images analysis:
Using the following command, with a threshold input we can filter blurred images:
stats_df = fd.img_stats()
Since the veins and arteries can be very thin and hard to detect, the shrapness (or blurriness) of the images is so important.

# An innovative feature - image quality improvement 

