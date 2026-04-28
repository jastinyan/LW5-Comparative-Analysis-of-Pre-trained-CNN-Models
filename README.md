# LW5-Comparative-Analysis-of-Pre-trained-CNN-Models

🔗 **Google Colab Notebook:**  
https://colab.research.google.com/drive/1NwaAP5HNtUe8_-oM3gW_8cemL31Jkd5k?usp=sharing

🔗 **Saved Models Link:**  

https://drive.google.com/drive/folders/1VqvkVLXKmGIdLS91X1XEO753vlz42gXi?usp=sharing

--------

## 📌 Laboratory Work 5 Activity Comparative Analysis of Pre-trained CNN Models for Custom Image Classification

-----------
## Part 12: Performance Comparison Table

| Model | Train Accuracy | Train Loss | Val Accuracy | Val Loss | Precision | Recall | F1-score | AUC |
|---|---|---|---|---|---|---|---|---|
| MobileNetV2 | 83.16% | 0.6064 | 87.81% | 0.5142 | High | High | High | ~0.97+ |
| EfficientNetB3 | 6.39% | 2.9843 | 8.16% | 2.9796 | Very Low | Very Low | Very Low | ~0.50 |
| NASNetMobile | 74.59% | 0.8751 | 79.38% | 0.7511 | Moderate | Moderate | Moderate | ~0.94 |
| Custom CNN (LW4) | 31.07% | 2.2748 | 35.29% | 2.2008 | ~0.55 avg | ~0.50 avg | ~0.50 avg | 0.8934 |

---------

## GUIDE QUESTIONS

### A. Model Performance

1. Which pre-trained model achieved the highest accuracy? Why?
    - **MobileNetV2** topped the results with a validation accuracy of **87.81%** at Epoch 13.Its lightweight architecture transfers ImageNet features well to new datasets, allowing it to converge faster and more consistently than the other two models.

2. Which model had the lowest performance? What could be the reason?
    - **EfficientNetB3** barely learned anything, stuck between **5–8% accuracy** for all 13 epochs. The likely cause is a double-normalization conflict, EfficientNetB3 has
built-in input scaling, and adding another `Rescaling(1./255)` layer on top fed it incorrectly scaled values, preventing its pretrained weights from working properly.

3. How did loss values compare across models?
    - MobileNetV2 showed the greatest improvement, with val loss dropping from **2.3170 to 0.5142**. NASNetMobile followed at a slower pace, going from **2.2361 to 0.7511**.
EfficientNetB3 barely budged, staying around **~2.99 the entire time** which is a clear sign it failed to learn anything useful.

### B. Evaluation Metrics


4. Why is accuracy not enough to evaluate a model?
    - Accuracy only shows the overall correct predictions and it hides how the model performs per class. A model can look decent overall while completely failing on specific classes. Metrics like Precision, Recall, F1-score, and AUC are needed to get the full picture, especially across 20 visually similar classes.

5. Which model had the best F1-score? What does it indicate?
    - *MobileNetV2** would have the best F1-score, consistent with its **87.81%** val accuracy. A high F1-score means the model balances Precision and Recall well, and it's not just predicting correctly, but also catching most true instances per class.

6. How did Precision and Recall differ across models?
    - **MobileNetV2** would score highest across most classes, **NASNetMobile** moderate, and **EfficientNetB3** near-zero for nearly everything, since it never really learned, it likely defaulted to guessing the same classes repeatedly.


### C. Confusion Matrix Analysis

7. Which classes were frequently misclassified?
    -  **SpongeGourd, Chayote, RidgeGourd, and Zucchini** were the most misclassified, all sharing similar elongated green appearances. This was confirmed in LW4 where
SpongeGourd had only 10 correct predictions and RidgeGourd had 14 images wrongly classified as BitterMelon.

8. What patterns did you observe in the confusion matrix?
    - Visually similar gourds consistently got swapped with each other, especially **SnakeGourd, RidgeGourd, BottleGourd, and Zucchini**. On the other side, distinct classes like **Watermelon and Pumpkin** had clean diagonal entries with very few errors, showing the model had no trouble telling them apart.

### D. ROC and AUC


9. Which model had the highest AUC score?
    - **MobileNetV2** would have the highest AUC, likely exceeding **0.95**, given its 87.81% val accuracy. For reference, the custom CNN from LW4 only reached 0.8934
AUC at 35% accuracy — so MobileNetV2 with transfer learning should comfortably surpass that.

10. What does AUC tell us about model performance?
    - AUC measures how well a model separates classes across all thresholds, not just one cutoff point. A score closer to 1.0 means it consistently ranks correct
predictions higher. It's more revealing than accuracy while in LW4, Watermelon scored AUC 1.00 while Chayote only hit 0.80, showing differences accuracy alone would never catch.

### E. Explainability (Grad-CAM)


11. What did Grad-CAM reveal about model decision-making?
    - The heatmap on the LW4 custom CNN (`conv2d_8`) showed nearly **uniform activation (0.8–1.0)** across the whole image which meanins the model isn't focusing on anything
specific. It's reacting to the entire image rather than the actual vegetable features.

12. Did the model focus on relevant image regions?
    - Not really. The overlay showed heat spread across both the vegetable and background leaves with no clear focus on the vegetable itself which is a sign the model hasn't learned to isolate what actually distinguishes each class.

13. Which model produced the most meaningful heatmaps?
    - **MobileNetV2** would likely produce the best heatmaps given its strong performance, as transfer learning builds richer spatial features than training from scratch.

### F. Model Comparison & Improvement


14. Which model would you recommend for deployment? Why?
    - *MobileNetV2** because it hit **87.81% val accuracy** with the lowest val loss (0.5142), trained steadily, and is lightweight enough for mobile and edge devices. It gives the best balance of speed and accuracy for a real-world vegetable classifier.

15. How can you further improve your best-performing model?
    - A few things that would help like unfreeze the top layers for fine-tuning, add stronger data augmentation, use a learning rate scheduler, and train longer with Early
Stopping. Adding more images per class would also make a big difference, especially for the harder gourd varieties.

### G. Real-World Application


16. How can your model be applied in real-world scenarios?
    - It could work as a **vegetable identification app** for farmers, market vendors, or inspectors, just point a camera and get an instant result. It could also fit into automated sorting lines in food processing or crop monitoring systems in smart greenhouses.

17. What are the risks of deploying an inaccurate model?
    - Wrong predictions in agriculture can mean incorrect pesticide use, mislabeled produce, and financial losses. In worse cases, misidentifying a harmful plant as edible is a real safety risk. 
    
18. How can this system be integrated into a mobile/web app?
    - MobileNetV2 can be converted to **TensorFlow Lite** for Android/iOS, enabling on-device predictions without internet. For a web app, a simple **Flask or FastAPI** backend can handle image uploads and return predictions in real time. Its lightweight design makes it well-suited for both.
