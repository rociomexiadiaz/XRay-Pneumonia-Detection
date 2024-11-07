# XRay-Pneumonia-Detection

## Data Download  
The data is downloaded from the Kaggle dataset https://www.kaggle.com/datasets/paultimothymooney/chest-xray-pneumonia/data. The data has been split and shuffled into folders for training, validation and testing in a 0.8:0.1:0.1 ratio.
## Data Analysis    
The dataset contains an imbalanced number of pneumonia and healthy X-ray images, requiring appropiate handling to avoid bias towards the majority class. The images have large and varying, non-square dimensions. They will have to be scaled down to fit into the GPU, and reshaped to a consistent square shape suitable for input into the CNN. Finally, although images are grey-scale, they are stored in a RGB format and will have to be reformatted to decrease the computational cost.  
Note: the pneumonia images are labeled with both bacterial and viral types; however, the model developed here is designed to classify only between healthy and pneumonia images, not between bacterial and viral pneumonia.
## Data Pre-processing  
The images were resized to a square dimension of 256 pixels. To preserve the original aspect ratio, the smallest edge was resized to 256 pixels, and the other edge was scaled before applying a center crop. The images were then converted to greyscale, and their pixel values were normalized to a mean of 0.5 with a standard deviation of 0.5. Data augmentation was applied to the training set, including random rotations between -10 and 10 degrees and random shearing. Horizontal and vertical flips were not included, as flipping medical images is unrealistic for the test set. To address class imbalance, a weighted random sampler was used in the dataloader for the training set.
## Defining the CNN  
The model implemented is a CNN model, it has a flexible architecture to allow depth modifications. Given that the images in the datasets have been resized to a dimension of 256 and the task is a binary classification, the default values are set to accept an image of size 256 and have the follow architecture:  

*   2 Convolutional blocks with a ReLU activation and Max Pooling layer.
*   The convolutional layers have a kernel size of 3x3, padding and stride are set to 1 to maintain the spatial dimensions, the output dimensions of the layers are 128 and 256.
*   Max pooling layers have a kernel size of 2x2 and a stride of 2 to half the spatial dimensions.
*   After the convolutional blocks, a flattening layer, a fully connected linear layer and a Sigmoid activation (for the BCE loss) are applied.

In the training function, a threshold of 0.5 is applied to convert the output of the sigmoid layer to a predicted class, and ADAM is used for optimisation. The learning rate is set to 0.0001 and the batch size to 32.
## Training the CNN  
The model has been trained and validated the loss and accuracy are plotted.
## Evaluating the CNN    
The model demonstrated a good performance across both the minority and majority classes, achieving very similar accuracies. This consistency suggests that the model can generalise well ensuring reliable predicitions regardless of the class imbalance.
The model showed the following performance metrics: 

- TPR: 94.86%  
- TNR: 93.08%  
- Recall: 94.86%   
- Precision: 97.36%  
- F1 score: 96.10%

These metrics underscore the overall robustness of the model and its potential for deployment in clinical settings.
