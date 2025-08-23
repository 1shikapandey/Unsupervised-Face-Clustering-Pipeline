<!DOCTYPE html>
<html lang="en">

<body style="font-family: Arial, sans-serif; line-height: 1.6; margin: 20px;">

  <h1 style="color: #2c3e50;">ML | Unsupervised Face Clustering Pipeline</h1>
  <p><strong>Last Updated:</strong> 04 Jan, 2023</p>

  <h2 style="color: #34495e;">📌 Overview</h2>
  <p>
    Live face-recognition is still a major challenge for automated security systems. 
    With the advancements in Convolutional Neural Networks (CNNs) and Region-CNN techniques, 
    supervised models such as <strong>FaceNet</strong> and <strong>YOLO</strong> can perform real-time recognition. 
    However, supervised training requires large, labeled datasets, which are costly and time-consuming to build. 
    This project introduces an <strong>unsupervised pipeline</strong> to automatically generate datasets 
    with minimal user labeling effort.
  </p>

  <h2 style="color: #34495e;">🚀 Proposed Solution</h2>
  <h3>Introduction</h3>
  <p>
    The system processes a video clip, extracts all faces, and clusters them into distinct groups. 
    Each group represents one unique person, making it easier for a human to assign labels.
  </p>

  <h3>Technical Details</h3>
  <ul>
    <li>Extract frames using <code>opencv</code> (1 frame per second).</li>
    <li>Detect and align faces using <code>face_recognition</code> (backed by <code>dlib</code>).</li>
    <li>Extract facial embeddings and cluster using <code>DBSCAN</code> from <code>scikit-learn</code>.</li>
    <li>Crop, label, and save clustered faces into folders for dataset creation.</li>
  </ul>

  <h3>Challenges</h3>
  <ul>
    <li>CPU implementation is slow (~30s per image). Optimized using <strong>parallel pipelines</strong> with <code>pyPiper</code> and reduced to ~13s/image.</li>
    <li>Implemented <code>tqdm</code> progress visualization.</li>
    <li>Frame resizing for smoother processing.</li>
  </ul>

  <h2 style="color: #34495e;">⚙️ Input / Output</h2>
  <ul>
    <li><strong>Input:</strong> <code>Footage.mp4</code></li>
    <li><strong>Output:</strong> Clustered face images grouped by identity</li>
  </ul>

  <h2 style="color: #34495e;">📦 Dependencies</h2>
  <p>Required Python 3 modules:</p>
  <pre style="background:#f4f4f4; padding:10px; border-radius:5px;">
os, cv2, numpy, tensorflow, json, re, shutil, time, pickle, 
pyPiper, tqdm, imutils, face_recognition, dlib, warnings, sklearn
  </pre>

  <h2 style="color: #34495e;">🧑‍💻 Code Structure</h2>
  <ul>
    <li><strong>FaceClusteringLibrary.py</strong> – Contains all class definitions
      <ul>
        <li><code>ResizeUtils</code> – Resize images while keeping aspect ratio</li>
        <li><code>FramesGenerator</code> – Extract frames from video</li>
        <li><code>FramesProvider</code> – Emit frames into pipeline</li>
        <li><code>FaceEncoder</code> – Encode faces into embeddings</li>
        <li><code>DatastoreManager</code> – Save embeddings as pickle files</li>
        <li><code>PicklesListCollator</code> – Merge pickles into one</li>
        <li><code>FaceClusterUtility</code> – Cluster embeddings with DBSCAN</li>
        <li><code>FaceImageGenerator</code> – Generate cropped faces & YOLO annotations</li>
        <li><code>TqdmUpdate</code> – Visualize pipeline progress</li>
      </ul>
    </li>
    <li><strong>Driver.py</strong> – Main entry point to execute pipeline</li>
  </ul>

  <h2 style="color: #34495e;">🖼️ Output</h2>
  <ul>
    <li>Clustered face folders: <code>ClusteredFaces/Face_X/</code></li>
    <li>YOLO-style annotation files</li>
    <li>Montages of clustered faces</li>
  </ul>

  <h2 style="color: #34495e;">🛠️ Troubleshooting</h2>
  <ul>
    <li><strong>PC Freezes?</strong> → Reduce frame size during extraction. Avoid too small sizes for better clustering.</li>
    <li><strong>High CPU Usage?</strong> → Lower number of threads in pipeline constructor.</li>
    <li><strong>Inaccurate Clustering?</strong> → Ensure video has clear and bright face images. Use longer video clips (2+ minutes).</li>
  </ul>

</body>
</html>
