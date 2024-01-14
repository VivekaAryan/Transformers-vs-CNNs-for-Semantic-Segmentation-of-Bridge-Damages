# Transformers-vs-CNNs-for-Semantic-Segmentation-of-Bridge-Damages

Structural health monitoring is crucial for ensuring the safety and durability of civil infrastructure. Detecting defects like cracks and corrosions is a key aspect, serving as early indicators of potential issues. While traditional CNNs are common for semantic segmentation in this domain, recent advances introduce transformer models like SegFormer. This project compares SegFormer with YOLOv8, evaluating their performance on bridge damage detection using the dacl10k dataset. The study includes multi-task architecture for various monitoring tasks, extending to identify bridge objects. YOLOv8m-seg outperformed SegFormer (MiT b5) with a higher mIOU of 0.312, despite having fewer parameters.

## Data

The "dacl10k" dataset (Johannes Flotzinger 2023) stands out as a highly diverse collection for multi-label semantic segmentation. Comprising 9,920 High-definition JPG images sourced from real-world bridge inspections, dacl10k identifies 13 damage classes and 6 crucial bridge components. These components play a pivotal role in building assessments, guiding actions such as restoration works or imposing traffic load limitations and bridge closures.

Annotations for these images follow the LabelMe format, stored in JSON files. The JSON file's top-level structure contains image information, including filename and size, along with annotations. The "image" key holds details such as the image filename, width, and height. The annotations are stored in an array under the "shapes" key. The "label" field designates the class label of the annotated object. Within the "points" field, the coordinates of the annotated shape are specified, with each set of coordinates representing the vertices of the polygon. The data is furnished for multiple occurrences of the same class.

### Data Preprocessing for SegFormer


The SegFormer model from HuggingFace was implemented, and dataset preprocessing was conducted to meet its specifications for training. Images were resized to 512 x 512, using provided code by Johannes Flotzinger in 2023. Annotations for SegFormer required 512 x 512 segmentation maps, where each pixel indicated the class index. Classes were labeled from 1 to 20, with 0 representing the background. After resizing annotations, validation was performed, and an example segmentation map is shown below. The original dataset had 19 x 512 x 512 segmentation masks. To comply with SegFormer's 2D mask requirement, a conversion from one-hot encoded channels to a single mask was done, saved in .png format.

### Data Preprocessing for YOLOv8

Resizing both images and annotations to 640 x 640 dimensions for uniformity in diverse-sized training data. YOLOv8-seg model requires annotations in .txt files, each line detailing class index and polygon coordinates (Ultralytics YOLOv8 2023). Converted LabelMe JSON format to annotation.txt for compatibility.

## Training

### Segformer Training
Implemented SegFormer b1, b3, and b5 using HuggingFace with ImageNet-1k pretrained weights. Conducted hyperparameter tuning due to the models' size increase (14 million to 82 million parameters). Experimentation results in the section display pixelwise accuracies for training and validation, along with mean Intersection over Union (mIOU) on the validation split.

### YOLOv8 Training

Utilized the 'Ultralytics' Python package to train small and medium YOLOv8-seg variants on the dataset, harnessing YOLOv8 Seg's capabilities for object detection and segmentation. Configuration file specified training arguments, including dataset path, number of classes, and class indexing and mapping.

## Predictions
![image](https://github.com/VivekaAryan/Transformers-vs-CNNs-for-Semantic-Segmentation-of-Bridge-Damages/assets/52493029/1920903e-976e-4a45-ab81-f716e6f9add8)

![image](https://github.com/VivekaAryan/Transformers-vs-CNNs-for-Semantic-Segmentation-of-Bridge-Damages/assets/52493029/dfba8ac3-d061-44e0-b57f-7b2a93c5e5a1)

![image](https://github.com/VivekaAryan/Transformers-vs-CNNs-for-Semantic-Segmentation-of-Bridge-Damages/assets/52493029/723affe0-50bd-4720-9011-67361cd194ce)


