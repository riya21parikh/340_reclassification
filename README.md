# Reclassifying Eagles: Enhancing Image Accuracy with Convolutional Neural Networks

### Authors: 
Riya Parikh, Tomas Samuolis  
**Faculty of Computing and Data Sciences**  
DS 340: Intro to Machine Learning/AI  
Instructor: Kevin Gold  
**Date:** December 9, 2024  

---

## Contact

For questions or collaborations, please contact:
Riya Parikh: [riyapar@bu.edu] and 
Tomas Samuolis: [samuolis@bu.edu]

---

## Introduction

This project addresses the challenge of building a convolutional neural network (CNN) architecture capable of identifying mislabeled training data. Specifically, we explore how varying levels of image augmentation impact model performance in the presence of mislabeling and associate confidence levels with labels for potential reclassification.

This problem was inspired by a real-world scenario one of our team members encountered while classifying mislabeled insurance claims. By simulating the problem with images, we explore how CNNs can handle imperfect data and improve classification reliability.

The following modules/libraries have been used to implement this project:

![Conda Badge](https://img.shields.io/badge/conda-342B029.svg?&style=for-the-badge&logo=anaconda&logoColor=white)
![Jupyter Badge](https://img.shields.io/badge/Jupyter-F37626.svg?&style=for-the-badge&logo=Jupyter&logoColor=white)
![Python Badge](https://img.shields.io/badge/Python-FFD43B?style=for-the-badge&logo=python&logoColor=blue)
![Keras Badge](https://img.shields.io/badge/Keras-FF0000?style=for-the-badge&logo=keras&logoColor=white)
![TensorFlow Badge](https://img.shields.io/badge/TensorFlow-FF6F00?style=for-the-badge&logo=tensorflow&logoColor=white)

---

## Methodology

1. **Dataset**:
   - Images of four bird species: American Eagles, Chickadees, Black Grouse, and Goldfinches.
   - 1300 training images and 50 validation images per category.
   - Relabeled as **"Eagle"** (target class, 25%) and **"Non-Eagle"** (other classes, 75%).

2. **Model Architecture**:
   - Used the **VGG16** CNN model with pre-trained ImageNet weights.
   - Excluded top layers and froze internal layers to focus on label performance.

3. **Baseline**:
   - Established baseline performance using correctly labeled data, achieving a validation accuracy of **93.71%** (epoch 6/15).

4. **Handling Mislabeled Data**:
   - Randomly mislabeled **20%** of the training images labeled as eagles.
   - Observed naive model predictions with validation accuracy of 75%, misclassifying eagles as non-eagles.

5. **Image Augmentation**:
   - Tested three levels:
     - **Simple**: Horizontal flip, 15° rotation, 0.1 zoom.
     - **Moderate**: Added brightness adjustment, horizontal/vertical shifts, and 20° rotation.
     - **Advanced**: Additional shearing and random color shifts.
   - **Moderate augmentation** yielded the best performance, surpassing naive classification under mislabeling.

6. **Confidence-Based Reclassification**:
   - Added a secondary confidence input channel to the CNN.
   - Iteratively updated confidence values for predictions, starting with:
     - **1.0** for eagles labeled as eagles.
     - **0.35** for non-eagle labels.
   - Used t-tests to identify statistical differences in predictions between true non-eagles and mislabeled eagles.
   - Iteratively flipped **5% of images** with the lowest confidence.

7. **Final Model**:
   - Reclassified the top **10% of predictions** below 0.5 and retrained the model with updated labels.
   - Used the best VGG16 configuration.

---

## Results

The final model achieved:
- **Validation Accuracy**: **94.8%**
- **Class-Specific Metrics**:
  - Precision:  
    - "Not Eagle": **97%**  
    - "Eagle": **85%**  
  - Recall:  
    - "Not Eagle": **95%**  
    - "Eagle": **92%**  
  - F1-Score:  
    - "Not Eagle": **96%**  
    - "Eagle": **88%**
- **Overall Metrics**:
  - Macro Averages (Precision: 91%, Recall: 93%, F1-Score: 92%)  
  - Weighted Averages (Precision, Recall, F1-Score: **94%**)  

These results highlight the model's robustness and its ability to handle noisy datasets effectively.

---

## Conclusions

Key takeaways:
- **Multi-Input Neural Networks**: Implementing multi-input models with confidence channels was a valuable learning experience.
- **Mislabel Handling**: Moderate augmentation combined with iterative confidence updates significantly mitigated the impact of mislabeled data.
- **Human-In-The-Loop**: While automatic reclassification was not fully feasible, we reduced the scope of manual review to only **400 images** (from 4000), enhancing practical usability.
