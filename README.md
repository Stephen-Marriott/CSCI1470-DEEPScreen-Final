# Modeling Drug-Target Interactions: Deep Learning Architectures to Classify Drug Activity for Target Proteins

## Team: Stephen Marriott, Wanming He, William Welsh

## Introduction
Our group is reimplementing on the models outlined in the following paper: [DEEPScreen journal article](https://doi.org/10.1039/C9SC03414E). The goal behind this model is to predict active compounds in target proteins using a convolutional neural network. This is an area of promise in deep learning moving forward, and touches on the intersection of chemistry and health, which all team members involved found interesting. This is structured in the form of a binary classification problem, where the model will determine whether the compound is active or inactive for that specific target. Additionally, we propose two alternative architectures: an Alternative CNN and an Attention-based CNN, aiming to improve prediction accuracy.

![poster](https://github.com/Stephen-Marriott/CSCI1470-DEEPScreen-Final/blob/main/DEEPScreen_Poster.png)

## Methodology

The DEEPScreen GitHub repository provided a link to a DropBox containing folders for each target protein, as well as images and labels for the active and inactive component compounds. The team has downloaded the data, although not all of it is uploaded to GitHub due to the size and volume. DEEPScreen provides an architecture for a model to learn the protein composition, but each protein has a separate model trained. Our group has re-implemented the DEEPScreen structure in TensorFlow, rather than the original PyTorch, and created two other architectures in the hopes of improving model performance. The first is an alternative CNN, with a smaller number of deeper convolutional layers than the original. The second aims to incorporate spatial attention into the classification model. At this stage, our models are essentially complete, aside from some minor potential changes to features like the learning rate for the optimizer.

## Results
Our experiments across 15 protein targets show that the Alternative CNN slightly outperforms the original DEEPScreen model in accuracy, while the Spatial Attention CNN exhibits higher variability and generally lower performance. 
| Target Protein | Model Type      | Test Loss       | Test Accuracy | Test Precision | Test Recall  |
|----------------|----------------|-----------------|---------------|----------------|--------------|
| CHEMBL1862     | DEEPScreen     | 0.437165        | 0.804404      | 0.862013       | 0.687824     |
| CHEMBL1862     | Alternative_CNN| 0.364040        | 0.848446      | 0.924494       | 0.650259     |
| CHEMBL1862     | Attention_CNN  | 0.494805        | 0.768135      | 0.599518       | 0.967617     |
| CHEMBL2581     | DEEPScreen     | 0.575983        | 0.688202      | 0.780645       | 0.339888     |
| CHEMBL2581     | Alternative_CNN| 0.579875        | 0.707865      | 0.901639       | 0.154494     |
| CHEMBL2581     | Attention_CNN  | 0.557039        | 0.710674      | 0.903846       | 0.132022     |
| CHEMBL253      | DEEPScreen     | 0.375896        | 0.863579      | 0.881921       | 0.827284     |
| CHEMBL253      | Alternative_CNN| 0.324372        | 0.871089      | 0.925676       | 0.685857     |
| CHEMBL253      | Attention_CNN  | 0.524984        | 0.768461      | 0.848087       | 0.485607     |

## Challenges
Our group has hit a few stumbling blocks along the way. First, a lot of the data processing in the original paper made use of PyTorch, in addition to the original model. This forced us to create new functions to load and split the images based on the provided labels. Finding a plausible way to implement spatial attention was a challenge, and this version borrows from what was originally a model for 3-Dimensional spatial attention. As we’ve built three separate model architectures, that has required a lot of time and effort to tune parameters and change the number and shape of layers. In particular, the two models we designed as alternatives to DeepScreen appeared to rapidly overfit the training data, which necessitated some changes in setup, as well as a lowering of the learning rate for the optimizer. While this has made the alternative CNN we proposed a little better than DEEPScreen in the models we’ve tested so far, we have yet to find a consistently accurate implementation of a model incorporating spatial attention. The attention model also takes a little bit longer to train, and adding more layers or rearranging existing ones has primarily served to slow the training process without adding any accuracy benefits.

## Reflection
* How do you feel your project ultimately turned out? How did you do relative to your base/target/stretch goals?
    Overall, the project was successful in reimplementing DEEPScreen in TensorFlow and proposing two alternative architectures. The Alternative CNN consistently outperformed the original DEEPScreen implementation in accuracy and precision, meeting our base goal of replication and our target goal of improvement. However, the Attention CNN underperformed, struggling with overfitting and inconsistent recall-precision trade-offs, falling short of our goal of a robust attention-based model.
* Did your model work out the way you expected it to?
    * Expected: We hypothesized that both the Alternative CNN and Attention CNN would outperform DEEPScreen. The Alternative CNN met this expectation, but the Attention CNN’s instability was surprising.
    * Unexpected Findings: The Attention CNN’s high recall but low precision suggested it is overfitting. We also found that DEEPScreen’s recall was surprisingly strong for some targets (82.7% for CHEMBL253), making it a tough baseline to beat.
* How did your approach change over time? What kind of pivots did you make, if any? Would you have done differently if you could do your project over again?
    * Initial Plan: Direct PyTorch-to-TensorFlow port of DEEPScreen + two new architectures.
    * Pivots: we rewrote PyTorch-dependent pipelines for TensorFlow. We also adapted a 3D spatial attention design, which may have been suboptimal for 2D structures. For our alternative CNN model, it initially overfit badly. We resolved by reducing layers and tuning learning rates.
    * Lessons: If we tested earlier on diverse proteins, we might have found architecture weaknesses sooner. Spending more time on learning rates and dropout might have helped the Attention CNN.
* What do you think you can further improve on if you had more time?
    We can definitly experiment with different attention variants to improve spatial focus without overfitting. We also could conduct grid searches for optimal learning rates, batch sizes, and regularization. We also would want to augment training data more to reduce overfitting.
* What are your biggest takeaways from this project/what did you learn?
    We learned from the Alternative CNN’s success that well-tuned, deeper convolutions can outperform complex models like attention when data is limited. We also learned that reproducibility is hard as porting PyTorch to TensorFlow introduced lots of bugs. And it is important to document all preprocessing steps and validate intermediate outputs.

## Article

Please find the original DEEPScreen paper here:
<br></br>
Rifaioglu, A. S., Nalbat, E., Atalay, V., Martin, M. J., Cetin-Atalay, R., & Doğan, T. (2020). DEEPScreen: high performance drug–target interaction prediction with convolutional neural networks using 2-D structural compound representations. *Chemical Science, 11*(9), 2531-2557.

