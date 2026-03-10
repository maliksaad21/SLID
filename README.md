Project:
Language Identification from Audio
This project trains and evaluates a model that can recognize the language spoken in an audio clip.The goal is to identify which language is being spoken based only on the speech signal. The model is trained using the NNti dataset, which contains audio recordings in different Indian languages. A pretrained speech model (mHuBERT) is optimized (fine tuned) so it can classify the language of each audio sample. After training is completed, the model is evaluated and compared with another version of the model.Different visualizations such as confusion matrices and t-SNE plots are used to understand the results.

Dataset
The dataset used in this project is the following:
badrex/nnti-dataset-full
It contains audio recordings of multiple languages. Each sample includes:
1) audio file
2) speaker id
3) language label
Audio is resampled to 16 kHz before being processed.

Model
The base model used in this project is the following:
utter-project/mHuBERT-147
This model is a pretrained speech representation model. It is fine tuned for audio classification, where the output is the predicted language label.
Changes were applied during training:
1) Audio processing limit is extended to 10 seconds.
2) dropout and warmu steps are added to improve generalization and improve accuracy.
3) audio augmentation is applied to the training data.
4) gradient accumulation is used to allow larger effective batch size.

Audio Augmentation
To make the model better and remove speaker bias, the training audio is slightly modified during preprocessing.
The following augmentations are used:
1) pitch shifting
2) gaussian noise
3) time stretching
These changes help the model learn more general speech patterns.

Training Setup
Training is done using the Hugging Face Trainer API. Important settings that are used:
1) learning rate: 0.00002
2) training epochs: 7
3) gradient accumulation steps: 16
4) weight decay: 0.04
Training logs are tracked using Weights & Biases (WandB). Final model is pushed to Hugging Face Hub after training.

Evaluation
After training is completed, the model is evaluated on the validation set.
Two models are compared which are following:
1) Baseline model
2) Improved model
A confusion matrix is used to see how well each language is classified. This helps identify which languages are commonly confused with each other.

Embedding Visualization
Two t-SNE plots are created at the end of code:
1) Baseline Model t-SNE: This plot shows how the original model groups the languages.
2) Improved Model t-SNE:This plot shows how the improved model (Audio augmentation) organizes the embeddings.
Comparing these two plots, it is easier to see if the improved model forms clearer and more separated clusters for each language. If the clusters are more compact and less overlapping, it means the model has learned better language representations.