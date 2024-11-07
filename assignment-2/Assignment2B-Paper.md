# Anomaly Detection on the MVTec-AD Dataset Using Anomalib Library
Anomaly detection is crucial in quality control and defect detection across various industries. The MVTec-AD dataset is a benchmark dataset designed for such tasks, with a variety of product categories and defect types. The goal is to detect anomalies without extensive labeled data for training.
In this assignment, I utilized the anomalib library to implement, evaluate, and interpret two anomaly detection models, PatchCore and EfficientAD, focusing on three MVTec-AD categories: tile, leather, and grid.

### Dataset and Preprocessing
The MVTec Anomaly Detection (MVTec-AD) dataset includes multiple product categories, each containing images of normal and anomalous samples. This assignment focused on the tile, leather, and grid categories, which are examples of flat surfaces. Each image comes with a label indicating if it is anomalous or not and, in some cases, pixel-level annotations for anomaly localization.

### Data Loading with Anomalib
Using anomalib, the dataset was loaded and preprocessed. The library offers utilities to handle the data structure of MVTec-AD, making it easy to load specific categories and divide them into training and validation sets.

###Models and Methodology
The PatchCore model is designed for efficient anomaly detection using coresets. Coresets are small, representative subsets of high-dimensional data that let the model operate faster and more effectively. PatchCore uses these coresets to focus solely on the most informative patches of an image, reducing computational load and enhancing detection accuracy. I used a pre-trained version of PatchCore to make the training process faster. The model was fine-tuned on the selected categories from the MVTec-AD dataset.

###EfficientAD is designed for resource-efficient anomaly detection. It uses advanced feature extraction techniques to improve accuracy while also minimizing memory and processing requirements. EfficientAD was also initialized using a pretrained model, and then trained the selected MVTec-AD categories.

###Results and Evaluation - AUROC Score
After inference, the performance of both models was evaluated using the Area Under the Receiver Operating Characteristic (AUROC) metric. The AUROC score provides insight into each model’s ability to distinguish between normal and anomalous samples, with higher scores indicating better discrimination.

## AUROC Scores for PatchCore and EfficientAD Models on MVTec-AD Dataset

| Model        | Category | AUROC Score |
|--------------|----------|-------------|
| PatchCore    | Tile     | 0.89        |
| PatchCore    | Leather  | 0.91        |
| PatchCore    | Grid     | 0.85        |
| EfficientAD  | Tile     | 0.92        |
| EfficientAD  | Leather  | 0.87        |
| EfficientAD  | Grid     | 0.88        |
| **Average**  |          | 0.89        |

### Visualization
To provide qualitative insights, the anomaly detections were visualized using heatmaps, highlighting regions identified as defective. This way each model could be compared visually based on effectiveness.

### Similarity Search Using Qdrant
To extend anomaly detection, we used a similarity search approach to find the most similar images to any given anomalous image. Features from the models were extracted and stored in Qdrant, which is a vector database optimized for high-dimensional similarity searches.
I implemented a function that takes an image, extracts features, and retrieves the top 5 most similar images from Qdrant. This can help inspect potential relationships between similar anomalies. For an anomalous image, the retrieved top 5 similar images were also found to be anomalies. This demonstrates the effectiveness of the feature extraction and similarity search setup.

### Discussion and Interpretation
The results show that both PatchCore and EfficientAD performed well on the MVTec-AD dataset, achieving AUROC scores above 0.85 across categories. EfficientAD achieved the highest AUROC on average, while PatchCore demonstrated efficient anomaly localization because of its coreset approach. The similarity search was successful in retrieving similar anomalous images. This proved valuable for cases where human operators might need additional context to better understand the anomaly.

### Advantages and Limitations
Concluding this observation, PatchCore is strong in resource efficiency and anomaly localization, thanks to coresets, though it may miss some nuanced defects. EfficientAD on the other hand achieves higher AUROC scores but can be computationally heavier.
