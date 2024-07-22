

**1) Brief description of the problem and data**

In this project I will make use of a convolutional nueral network (CNN) in order to detect metatstatic cancer in cells. The data used will be from a (Kaggle Competition)[https://www.kaggle.com/c/histopathologic-cancer-detection/data]. The data are RGB images, with corresponding labels supplied for the training data. I will run my model on the test data to generate models and validate the test accuracy using Kaggle's submission feature.

The data are split into two binary exlusive classes. The images are of cells that either do or do not have cancer. In total, there are 220,025 training cases. I chose to use 80% for training, and 20% for validation in accordance with the rule of thumb. The According to the Kaggle listing, the training data, test data, and labels combined consist of a ~7.75 GB file.

The individual image files are all .tif files of shape (96, 96, 3). According to the Kaggle listing, the training label is assigned as positive if at least one pixel in the center (32, 32) of an image contains tumorous tissue. The rest of the image is provided to enable CNN with padding schemes / convolution stride that eats away at the exterior of the image.
The test dataset consists of unlabeled .tif images. The goal of this project is to predict the labels of those test images with the CNN I will build.

**2) Exploratory Data Analysis**

Please refer below example photo from the training data, along with its type and shape.

<img width="366" alt="image" src="https://github.com/user-attachments/assets/41d1f2f4-4d92-4625-a742-127c7a1d0072">

<img width="266" alt="image" src="https://github.com/user-attachments/assets/691fe676-71df-4f80-9d55-7427a2854de7">

**3) Model Architecture and Construction**

I will try a couple different architectures, because CNN architecture itself is a hyperparameter that needs optimization. Keras provides a wonderful API to build simple CNN's in. In particular, the Sequential API is built for single input single output models. We are inputting single images, and want to output a single label, so the Sequential API works perfect.

We learned in class that the basic effective architecture for a CNN looks like [convolve -> convolve -> maxpool]$_n$, and then fed into a dense ANN for classification. I will use $n = 3$ for my project, because as $n$ increases the CNN develops the ability to "see" more macroscopic trends in the data, which may indicate the present of cancerous tissue. By preventing too many layers, we can try and prevent overfitting.

We learned that either relu or PreLu is the best choice of activation function for hidden layers. The Keras Sequential API doesn't have a built in PreLu, and I fear that any that I implement will be algorthmically inefficient in Python, so I won't worry about optimizing the interem activation functions. We will use (3x3) filters as suggested. I will try both the Adam and RMSProp optimizers, and various values of learning rate between (0.0001, 0.001). I will also use a learning_rate scheduler to minimize variance towards the end of training. I will try Dropout layers in various locations. I will use binary cross entropy as my loss function, because it is the most appropriate for logit outputs from a sigmoid output activation.


<img width="269" alt="image" src="https://github.com/user-attachments/assets/a20e4c3d-2947-4267-9195-902695fe5364">

<img width="270" alt="image" src="https://github.com/user-attachments/assets/f16599d0-a697-4e86-b14c-581bdd8681a3">


**4) Conclusion**

The test set accuracy is an abysmal 50%, which is pretty impossible because while it should be as high as the training accuracy, there is no way that the model was just making random guesses, which is what an accuracy of 50% implies.

I found a very nice resource which I am going to follow along with for my final. Here's the resource if y'all ran into similar problems that I did:

https://keras.io/examples/vision/image_classification_from_scratch/

**Some primary takeaways:**

A too small eta will have just as negative a performance on the CNN's ability to properly fit as too high an eta. The "Adam" optimizer slightly outperformed the "RMSprop" optimizer, by about 0.05 accuracy points. That's quite significant. The inclusion of multiple dropout layers really helped to prevent overfitting, which was easy to spot when the accuracy began to dip back down / loss asymptoted.

I did this project on Google collab, which has the annoying feature of randomly booting users out during cell executions. Moreover, it totally resets the environment sometimes, which means I had to re-load and re-train all my models many times. This project took way longer than I wanted it to, but served as a great introduction to both CNN and to Google collab. Thanks for taking the time to review my project, and have a wonderful day.


