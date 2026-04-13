# Multi-modal-Prediction-Model-for-Source-Rock-Quality
A deep learning-based multimodal (text, image, numerical) data fusion model, specifically designed for the processing and prediction of small-sample, complex-type source rock data. By integrating seismic attribute data, waveform/frequency band images, and relevant geological text descriptions, this project achieves precise classification and assessment of source rock quality.

📑 Project Introduction  
  In geological exploration and source rock analysis, a single data source is often insufficient to fully capture the distribution characteristics of source rocks. To address this, our project constructs a multimodal neural network architecture that integrates:
1.	Bidirectional Encoder Representation from Transformers (BERT)：Extract semantic features from text to describe the fundamental patterns of source rock distribution.
2.	Convolutional Neural Networks (CNN)：Extract visual features from frequency band images
3.	Multi-Layer Perceptron (MLP)：Process numerical features of seismic attributes.
The model incorporates an attention mechanism during the feature fusion stage to adaptively assign weights to image and text features. These are then concatenated with numerical features to perform a five-class classification task of source rock quality (Best, Good, Moderate, Poor, and Sandstone).
________________________________________
📊 Dataset Description  
The dataset consists of four primary task directories (e.g., Task 1 to Task 4), each containing data from the following three modalities:
•	Numerical Data: Extracted from .xlsx files, containing 9 core seismic/well-logging attributes:：Depth, Cosine of Phase, Instantaneous Bandwidth, Instantaneous Frequency, Instantaneous Phase, Instantaneous Quality ($Q$), Root Mean Square (RMS) Amplitude, Relative Acoustic Impedance, and Variance.
•	Visual Data (Image): The data is dynamically read and plotted as waveform images using matplotlib. These images are subsequently processed through grayscale conversion and downsampling to serve as spatial visual inputs for the model.
•	Textual Data (Text): Sourced from .txt geological description files, these are transformed into 64-dimensional word embeddings using the bert-base-chinese model.
•	Target Labels: Source rock quality categories consist of: Best (7,645), Sandstone (3,021), Good (2,065), Medium (1,087), and Bad (430).
________________________________________
🧠 Model Architecture  
The model architecture comprises three independent branch networks and a feature fusion module:
1.	Image Sub-network：
o	Utilizes multiple layers of Conv2D, BatchNormalization, and MaxPooling2D.
o	After flattening via a GlobalAveragePooling2D layer, it connects to a Dense layer to output a 1D feature vector.
2.	Text Sub-network：
o	It accepts the text vectors preprocessed by BERT.
o	Employs a combination of Dense, BatchNormalization, and Dropout layers for non-linear mapping.
3.	Numerical Sub-network：
o	Applies an adaptive Normalization layer to standardize the numerical features.
o	Extracts features through multiple layers of Dense networks with Dropout.
4.	Multi-modal Fusion & Attention：
o	Concatenates image and text features, then computes attention weights using a Dense layer with Softmax activation and L2 regularization.
o	Concatenates the weighted image and text features with the numerical features along the channel dimension.
o	The fused representation is fed into the final classifier, which outputs a probability distribution across the five target classes.
________________________________________
⚙️ Dependencies  
Before running this experiment, please ensure that the following dependencies are installed:
Bash
pip install pandas numpy matplotlib seaborn scikit-learn scikit-image
pip install tensorflow
pip install torch transformers
________________________________________
🚀 Training Configuration and Experimental Details
•	Optimizer: Adam (initial learning rate lr=0.001)
•	Loss Function: Categorical Crossentropy 
•	Evaluation Metric: Accuracy 
•	Epochs: 100
•	Batch Size: 64
•	Callbacks:
o	EarlyStopping: Monitors val_loss, patience=10, automatically restores the best model weights.
o	ReduceLROnPlateau: Monitors val_loss, reduces the learning rate by half (factor=0.5) after 4 epochs of stagnation.
________________________________________
📈 Model Performance and Result Visualization 
Metric	Score
Accuracy	0.7828
Precision	0.7743
Recall	0.7828
F1-Score 	0.7749

Visualization analysis includes：
After executing the code, the following visualization plots will be generated in the current directory:
1.	training_history.png: Records the loss and accuracy curves over 100 epochs, illustrating the model's convergence process.
2.	performance_metrics.png: A bar chart displaying the four core performance evaluation metrics.
3.	roc_curves.png: Contains the ROC curves for all 5 classes and their corresponding AUC (Area Under the Curve) values, demonstrating the classifier's discriminative ability.
4.	t-SNE：
o	t-SNE of Raw Numerical Features.png: A 2D scatter plot of the original numerical features.
o	t-SNE of Model Fusion Features.png: A 2D dimensionality reduction visualization of the deep fused features from the penultimate layer, demonstrating that the multi-modal fused features exhibit significantly better inter-class separability compared to the raw data.
o	multimodal_tsne_comparison.png: A t-SNE comparison plot of the three individual unimodal features: text, image, and numerical.
________________________________________
📂 File Directory Structure
Plaintext
├── 任务一/
│   ├── PY10-6-4.txt
│   ├── PY10-6-4.xlsx
│   └── 代表砂岩的图像/
│       └── PY10-6-4砂岩图像原始数据.xlsx
├── 任务二/ ...
├── 任务三/ ...
├── 任务四/ ...
├── multimodal predictive model.ipynb   # 核心代码文件
├── training_history.png                # 生成的训练曲线
├── roc_curves.png                      # 生成的ROC曲线
└── multimodal_tsne_comparison.png      # 生成的t-SNE可视化
________________________________________
💡 User Guide
1.	Ensure your data is placed in the correct directory. (Please update the current_dir variable before calling get_all_files to match your actual file path).
2.	Ensure a stable internet connection to allow Hugging Face to download the pre-trained bert-base-chinese weights.
3.	Execute all cells in the Jupyter Notebook sequentially; the model will automatically handle data preprocessing, construction, training, and finally output and save the evaluation plots.

