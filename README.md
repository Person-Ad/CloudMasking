# AI-Powered Cloud Masking for Satellite Imagery

## Objective
This project develops an AI-based system for automated cloud detection in multispectral satellite imagery, generating binary cloud masks using advanced machine learning and deep learning techniques. The goal is to accurately identify cloud-covered regions while balancing performance and efficiency.

## Dataset
- **Size**: Approximately 10,000 satellite image pairs (512x512 pixels).
- **Composition**: Each pair includes a 4-band TIFF image (Red, Green, Blue, Near-Infrared) and a binary cloud mask (1 for clouds, 0 for background).
- **Data Split**: 90:10 train-test split, with 5-fold cross-validation on the training set.

## Approach
1. **Exploratory Data Analysis (EDA)**:
   - Analyzed cloud cover distribution across the dataset.
   - Categorized images into:
     - **Fully Clouded**: >80% coverage (25.66% of data).
     - **Partially Clouded**: 20%-80% coverage.
     - **Cloud-Free**: <20% coverage (6.63% of data).
   - Identified ~8% noisy data points (e.g., mislabeled masks).

2. **Preprocessing**:
   - **Data Augmentation**: Applied horizontal/vertical flipping and rotations to enhance generalization.
   - **Band Usage**: Utilized all four bands (R, G, B, NIR) for their spectral information.
   - **Normalization**: Divided pixel values by 2^16 for stable training.
   - **Noise Handling**: Excluded noisy samples from initial training, then iteratively relabeled them using a semi-supervised approach over ~8 iterations.

3. **Model Selection**:
   - Explored architectures: U-Net, U-Net++, DCNet, DeeplabV3+, SegFormer, and Cloudformer.
   - Selected **DeeplabV3+** (ResNet-50 backbone) for its superior balance of accuracy and efficiency.
   - Compared with a baseline SVM model (Dice: 0.77 on uncleaned subset).

4. **Training**:
   - Trained DeeplabV3+ with 5-fold cross-validation over 10-15 epochs per fold.
   - Used a batch size of 10, learning rate of 5e-4, Adam optimizer, combined BCE-Dice loss, and gradient clipping.
   - Incorporated relabeled noisy masks in training while validating on original masks.

5. **Evaluation**:
   - Achieved a Dice coefficient of **0.8966** on the hidden test set (with noisy data).
   - Performance improved to ~**0.94** on a cleaned test set.

## Key Findings
- DeeplabV3+ effectively segments clouds, with strong generalization despite noisy data.
- Challenges include distinguishing clouds from spectrally similar features (e.g., snow, ice) due to limited texture differentiation.
- Noise-aware training significantly boosted performance, highlighting the impact of data quality.

## Future Work
- **Texture Analysis**: Enhance differentiation of clouds from snow/ice using advanced texture features.
- **Ensemble Methods**: Combine multiple models (e.g., SegFormer, Cloudformer) for improved robustness.

## Team Contributions
| Team Member          | ID       | Contribution(s)                  |
|----------------------|----------|-----------------------------------|
| Ahmed Osama Helmy    | 9213061  | SegFormer Model, Project Report  |
| Aliaa Abdelaziz      | 9210694  | SegFormer Model, Project Report  |
| Abdallah Ahmed Ali   | 9210652  | DCNet/U-Net, Profiler            |
| Omar Mahmoud         | 9210758  | Preprocessing, DeeplabV3+, Profiler, Submission |

## Model Complexity
- **Input Size**: (1, 4, 512, 512)
- **Operations**: 73.6944 GOps
- **Parameters**: 26.6807M (ResNet-50 backbone)

## References
Detailed citations and additional results are available in the [project report](https://docs.google.com/document/d/11Q3X56FUc2oL_Gz1qmsRUvVb2Vgcqq9yOPvqJ2rDEb8/edit?tab=t.0). Key references include:
1. Semantic Segmentation of Remote Sensing Images Using Deep Learning: A Survey
2. Encoder-Decoder with Atrous Separable Convolution for Semantic Image Segmentation
3. SegFormer: Simple and Efficient Design for Semantic Segmentation with Transformers
