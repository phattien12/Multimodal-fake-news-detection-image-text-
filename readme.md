<h1 align="center">🚀 Multimodal Fake News Detection (Image + Text)</h1>

<p align="center">
A deep learning pipeline for detecting fake news using both <b>text and image</b> modalities.
</p>

<hr>

<h2>📌 1. Overview</h2>

<p>
This project implements a <b>multimodal fake news detection system</b> using:
</p>

<ul>
<li>🧠 Text: DistilBERT</li>
<li>🖼️ Image: ResNet18</li>
<li>🔗 Fusion: Concatenation + Fully Connected Layer</li>
</ul>

<p>
Fake news detection is a critical task due to the rapid spread of misinformation on social media. 
Unlike traditional approaches that rely only on text, this project leverages both textual and visual information to improve performance. 
Multimodal approaches have been shown to outperform text-only models (e.g., ~87% vs ~78% accuracy). :contentReference[oaicite:0]{index=0}
</p>

<hr>

<h2>📂 2. Dataset</h2>

<p>
Dataset used: <b>MediaEval 2015 Image Verification Corpus</b>
</p>

<ul>
<li>📄 tweets.txt → tweet content</li>
<li>🖼️ images.txt → image metadata</li>
<li>📊 tweet_features.csv → tweet features</li>
<li>👤 user_features.csv → user metadata</li>
</ul>

<p>
Each sample contains:
</p>

<ul>
<li>Tweet text</li>
<li>Image ID</li>
<li>User info</li>
<li>Label (REAL / FAKE)</li>
</ul>

<p>
This dataset includes thousands of tweets with associated images and binary labels (real/fake), widely used in multimodal misinformation research. :contentReference[oaicite:1]{index=1}
</p>

<hr>

<h2>⚙️ 3. Pipeline</h2>

<h3>3.1 Data Processing</h3>

<ul>
<li>Load <code>tweets.txt</code> and <code>images.txt</code></li>
<li>Clean and normalize columns</li>
<li>Extract <code>image_id</code></li>
<li>Merge text + image metadata</li>
<li>Convert labels:
    <ul>
        <li>REAL → 0</li>
        <li>FAKE → 1</li>
    </ul>
</li>
</ul>

<h3>3.2 Text Processing</h3>

<ul>
<li>Tokenizer: <b>DistilBERT</b></li>
<li>Max length: 128</li>
<li>Padding + truncation</li>
</ul>

<h3>3.3 Image Processing</h3>

<ul>
<li>Resize: 224x224</li>
<li>Normalize → Tensor</li>
<li>Fallback: zero tensor if image missing</li>
</ul>

<h3>3.4 Dataset Class</h3>

<ul>
<li>Custom PyTorch Dataset</li>
<li>Returns:
    <ul>
        <li>input_ids</li>
        <li>attention_mask</li>
        <li>image tensor</li>
        <li>label</li>
    </ul>
</li>
</ul>

<hr>

<h2>🧠 4. Model Architecture</h2>

<p><b>Multimodal Fusion Model</b></p>

<ul>
<li>Text Encoder → DistilBERT (768 dim)</li>
<li>Image Encoder → ResNet18 (512 dim)</li>
<li>Fusion → Concatenate (1280 dim)</li>
<li>Classifier → Fully Connected Layers</li>
</ul>

<p>
Final pipeline:
</p>

<pre>
Text → DistilBERT ┐
                  ├── concat → FC → Prediction
Image → ResNet18 ┘
</pre>

<hr>

<h2>⚡ 5. Training Setup</h2>

<ul>
<li>GPU: Colab T4</li>
<li>Batch size: 8</li>
<li>Optimizer: Adam (lr=2e-4)</li>
<li>Loss: CrossEntropyLoss</li>
<li>Epochs: 10</li>
<li>Mixed precision (FP16)</li>
</ul>

<p><b>Optimization strategies (anti-OOM):</b></p>

<ul>
<li>Freeze backbone models</li>
<li>Use DistilBERT (lighter than BERT)</li>
<li>Use ResNet18 (lighter than ResNet50)</li>
<li>Use AMP (FP16)</li>
</ul>

<hr>

<h2>📊 6. Results</h2>

<h3>6.1 Training Performance</h3>

<pre>
Final Training Loss ≈ 0.47
</pre>

<h3>6.2 Test Metrics</h3>

<ul>
<li><b>Accuracy:</b> 0.8079 (~80.8%)</li>
<li><b>Precision:</b> 0.8468</li>
<li><b>Recall:</b> 0.8154</li>
<li><b>F1-score:</b> 0.8308</li>
</ul>

<h3>6.3 Confusion Matrix</h3>

<pre>
[[ 785  199]
 [ 249 1100]]
</pre>

<p>
Interpretation:
</p>

<ul>
<li>True Negative (REAL → REAL): 785</li>
<li>True Positive (FAKE → FAKE): 1100</li>
<li>False Positive: 199</li>
<li>False Negative: 249 ⚠️</li>
</ul>

<hr>

<h2>🔍 7. Sample Predictions</h2>

<pre>
TEXT: Buenos y lluviosos dias...
TRUE: FAKE
PRED: FAKE

TEXT: Amazing cover from New York Magazine...
TRUE: REAL
PRED: REAL
</pre>

<p>
Model predictions align well with ground truth, indicating good generalization.
</p>

<hr>

<h2>📈 8. Analysis</h2>

<p>
✔ Model achieves ~80% accuracy → <b>strong baseline</b><br>
✔ Multimodal fusion improves performance over text-only models<br>
</p>

<p>
However:
</p>

<ul>
<li>❌ Still below SOTA (~85–90%)</li>
<li>❌ Dataset is noisy</li>
<li>❌ Backbone is frozen (limits learning)</li>
</ul>

<hr>

<h2>🚀 9. Future Improvements</h2>

<ul>
<li>Unfreeze last layers of BERT</li>
<li>Use CLIP (joint image-text embedding)</li>
<li>Incorporate tweet/user features</li>
<li>Data cleaning & augmentation</li>
<li>Attention-based fusion</li>
</ul>

<hr>

<h2>🛠️ 10. Tech Stack</h2>

<ul>
<li>Python</li>
<li>PyTorch</li>
<li>Transformers (HuggingFace)</li>
<li>Torchvision</li>
<li>Scikit-learn</li>
</ul>

<hr>

<h2>📌 11. Key Takeaways</h2>

<ul>
<li>Multimodal learning significantly boosts fake news detection</li>
<li>Image-text inconsistency is a strong signal</li>
<li>Lightweight models can still achieve solid performance (~80%)</li>
</ul>

<hr>

<h2>👨‍💻 Author</h2>

<p>
Phat – A passionate researcher in <b>Computer Vision</b>, <b>Artificial Intelligence</b>, and <b>Multimodal Image Processing</b>.
</p>


<p>
Project implemented on Google Colab using GPU T4.<br>
</p>

<p align="center">
🔥 If you find this useful, consider improving it with CLIP or advanced fusion!
</p>