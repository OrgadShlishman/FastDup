# Introduction
In this blog post, I’ll walk you through my analysis of RAVIR using the powerful fastdup tool. 
This analysis includes detecting duplicates, identifying anomalies, clustering similar images, and presenting innovative application of the insights gained.

![image](https://github.com/user-attachments/assets/43d82b11-6398-485c-a71d-465a9273c0dd)






# Dataset Overview
RAVIR is a dataset of the retina. it includes IR images, which eventually supposed to help with health issues diagnosis (such as cancer, diabease and more).
![image](https://github.com/user-attachments/assets/42729cb0-273b-4c28-beea-08898d40dce0)

The main target of RAVIR dataset, is of use for classifying (Semantic segmentation) between veins and arteries in the retina, which can help for various health diagnosis.
If you want to read more, you can see the semantic segmentation of SegRAVIR:
https://arxiv.org/abs/2203.14928

The dataset is public and can be found in the following link:
https://drive.google.com/file/d/1ZlZoSStvE9VCRq3bJiGhQH931EF0h3hh/view

Due to the required high percision in classification between veins and arteries in the retina, dataset managment tool can be very helpful for using RAVIR.
So, in the next section, I will show how I used fastdup to evaluate the image quality of the RAVIR dataset.





# Installation and setup
The Dataset will be downloaded using the following commands:

```
file_id = '1ZlZoSStvE9VCRq3bJiGhQH931EF0h3hh'
url = f'https://drive.google.com/uc?id={file_id}'

output = 'ravir-dataset.zip'
gdown.download(url, output, quiet=False)

!unzip ravir-dataset.zip
dataset_path = '/content/RAVIR Dataset/'
```

Then, setting up the fastdup model and create fastdup dataset object;
```
work_dir = os.getcwd()
dataset_path = work_dir+'/RAVIR Dataset/test'
fd = fastdup.create(input_dir=dataset_path)
fd.run(overwrite=True)
```

Now we have all the information ready and we can start analyze and visualize the relevant data.





# Data analysis
For RAVIR, it is more interesting to detect low quality images.
Hence, let's search for outliers, bright, dark and blurred images using the fastdup features.

Data reports can provide a few types of information about the images.
We can get high level report, images information report and similarity related information.

On the attached notebook (See in the bottom of this page) each option wlil be shown, but for this document, we will focus on detecting low quality images.

Running images information report is done using the following command:
```
stats_df = fd.img_stats()
stats_df
```

It produces tables like the following:
![image](https://github.com/user-attachments/assets/c3ea078e-2353-4117-a703-c20459065fed)

We can filter images according to its darkness, beightness and according to more features as can be seen in the example of the table above.
Blurry umages are interesting for us more than the other parameters, since a clearer image should lead to a better segmentation results.

Blurry images with blurry threshold parameter set to 700 (All blurred images that have blur of 700 or more) can be found using the following code:

```
stats_df = fd.img_stats()
blur_threshold = 700
blurry_images = stats_df[stats_df['blur'] < blur_threshold]
blurry_images
```

Outliers can also be interesting for us to examine, since outliers images, could be hard to segment.
Outliers can be found using the following command:
```
stats_df = fd.outliers()
stats_df
```

From this analysis of outliers and blurred images, I've seen that RAVIR requires sharper and cleaner images to get better segmentation results.





# Data visualization
It is easy to display the interesting (Low quality) images we found during the data analysis.

For blurred images visualization, we can use the following commands (Displays the images in ascending order sorted by the blur property):
```
fd.vis.stats_gallery(metric='blur')
```

![image](https://github.com/user-attachments/assets/6a8043a9-ffbf-46fc-82ad-8c559d2a2305)


For the outliers visualization we can use:
```
fd.vis.outliers_gallery()
```

![image](https://github.com/user-attachments/assets/13984e10-0282-458b-93c5-d21295729176)

*Other grouping, duplications and more visualization options, can be found on the Python notebook attached in the end of this document.




# Innovative application
My idea for improving the semantic segmentation task on RAVIR, is to make sure we are using high quality to improve the results.
For this task, the idea is using fastdup abilities to filter the low quality images (For example, blurred images and outliers) and then, improve them using image processing and computer vision tools.
After improving the images, when can decide if he wants to update the dataset with them or not.

![image](https://github.com/user-attachments/assets/48b900a8-c30e-4cc0-a4a5-510ebf4ece7a)


*Step 4 is optional.

After filtering the low quality images, we would like to improve their quality. We will do so by using a classical algorithm called Unsharp Mask. The idea is to enhance high frequencies of the image.
It is done by blurring the input image using Gaussian kernel, and then subtract the blurred output of the Gaussian. the result will be an image with high frequencies (usually edges in high resolution details, which will add up to the original image, resulting in better quality and sharper image.



Examples:

![image](https://github.com/user-attachments/assets/ba0696bd-02d3-4ef8-83ac-09e8f1846e65)


The improved feature is defined in a class called *"ImproveImage"*. It can get one image or a list of images and for eahc type of the inputs, it can generate preview or save the image (or images if we have a list of images).


Usage Example:
*For a single image*
'''
improve_image_single = ImproveImage(input_image_path, output_directory)
improve_image_single.preview()
improve_image_single.save()

*For multiple images*
improve_image_multiple = ImproveImage(list_of_input_images_path, output_directory)
improve_image_multiple.preview()
improve_image_multiple.save()
'''

# Conclusion
FastDup is a powerful tool for dataset analysis. It allows to easily better understand, sort and work with datasets.
For RAVIR dataset, the most important thing is the percision and high quality of the image, hence, filter low quality images and improving them using the new innovative feature, could ease and optimize results for veins and arteries semantic segmentation in the retina.

Related Links:
1. Jupyter notebook summary: 
2. RAVIR dataset: https://drive.google.com/file/d/1ZlZoSStvE9VCRq3bJiGhQH931EF0h3hh/view
3. RAVIR paper: https://arxiv.org/abs/2203.14928
4. Fastdup code: https://github.com/visual-layer/fastdup?tab=readme-ov-file
