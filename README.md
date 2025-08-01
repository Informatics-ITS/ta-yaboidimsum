# 🏁 Tugas Akhir (TA) - Final Project

**Nama Mahasiswa**: Dimas Prihady Setyawan

**NRP**: 5025211184

**Judul TA**: PENERAPAN GENERATIVE ADVERSARIAL NETWORKS DALAM GENERASI DATA SINTETIS SEL DARAH PUTIH KASUS LEUKEMIA LIMFOBLASTIK AKUT DAN EVALUASI KINERJA MODEL KLASIFIKASI

**Dosen Pembimbing**: Prof. Dr. Eng. Chastine Fatichah, S.Kom., M.Kom.

**Dosen Ko-pembimbing**: Aldinata Rizky Revanda, S.Kom., M.Kom.

---

## 📺 Demo Aplikasi

<!-- Embed video demo di bawah ini (ganti `VIDEO_ID` dengan ID video YouTube Anda): -->

[![Demo Aplikasi](./thumbnail.png)](https://www.youtube.com/watch?v=jg2PT2XjFbA)  
_Klik gambar di atas untuk menonton demo_

---

<!-- _Konten selanjutnya hanya merupakan contoh awalan yang baik. Anda dapat berimprovisasi bila diperlukan._ -->

## 🛠 Software Installation & Execution Guide

This guide provides instructions for installing the necessary software, setting up the environment, and running the training and image generation processes.

### Requirements

To run this project, you need:

<!-- - Daftar dependensi (contoh):
  - Python 3.10+
  - Node.js v18+
  - MySQL 8.0
  - [Lainnya...] -->

- Docker Desktop 🐳

### Step by Step to Run the Training Process

Follow these steps to set up the environment and start the training process:

1. **Clone Repository 💻**

   Clone the project repository to your local machine:

   ```bash
   git clone https://github.com/Informatics-ITS/ta-yaboidimsum.git
   ```

2. **Download Datasets and Models 📦**

   Download the necessary datasets, pretrained models, and trained models from the provided [Google Drive link](https://drive.google.com/drive/folders/1EsdZt3KsHl_-xL8pzHPgAUGfo_7pl2Kx?usp=sharing). Organize the downloaded files into the following directory structure within your project folder:

   ```bash
   |.
   ├── dataset/ 📊
   │   ├── L1-Converted
   │   ├── L2-converted.zip
   │   └── L3-Converted
   ├── model/ 🧠
   │   └── ffhq-res256-mirror-paper256-noaug.pkl 🤖
   └── model-trained/ 📂
      ├── model-ada/ 🤖
      │   ├── L1-network-snapshot-001000.pkl
      │   ├── L2-network-snapshot-001000.pkl
      │   └── L3-network-snapshot-001000.pkl
      ├── model-finetuning/ 🤖
      │   ├── L1-Finetune-1000.pkl
      │   ├── L2-Finetune-1000.pkl
      │   └── L3-Finetune-1000.pkl
      └── model-noaug/ 🤖
         ├── L1-noaug-network-snapshot-001000.pkl
         ├── L2-noaug-network-snapshot-001000.pkl
         └── L3-noaug-network-snapshot-001000.pkl

   ```

3. **Log in to Docker 🐳**

   Log in to your Docker account via the terminal:

   ```bash
   docker login
   ```

4. **Build the Docker Image 🏗️**

   Navigate to your project directory and build the Docker image. Replace `<image_name>` and `<tag>` with appropriate names for your image:

   ```bash
   cd [folder-proyek]
   docker build -t <image_name>:<tag> .
   ```

5. **Run the Container 🏃**

   Run the Docker container on a specific port. This command uses docker run to start the container, mount the current directory, and map port 6006 for potential access (e.g., if using TensorBoard).

   ```bash
   docker run --gpus all --rm -it -v "${PWD}:/stylegan2-ada-pytorch" -p 6006:6006 <your docker image> /bin/sh
   ```

6. **Navigate Inside the Container 🗺️**

   Once inside the container, navigate to the mounted folder. You start in `/workspace` and need to move to the stylegan2-ada-pytorch directory:

   ```bash
   cd ..
   cd stylegan2-ada-pytorch
   ```

7. **Install Additional Libraries ⚙️**

   Install the Optuna library, which is required for hyperparameter tuning:

   ```bash
   pip install optuna
   ```

8. **Execute the Training Process 💡**

   Use the appropriate command to run the L1, L2, or L3 training process. (Note on Command Syntax: If you encounter an error when executing the command, there might be an invisible character or formatting issue between `optimize` and` --outdir`. Simply delete the space and re-add it to resolve the issue.)

   **Command for Executing L1 Training**

   ```bash
   python train-optuna-TPESampler.py optimize --outdir=./optuna_grid_results --data=./dataset/L1-converted.zip --resume=./model/ffhq-res256-mirror-paper256-noaug.pkl --gpus=1 --study-name=my_grid_study_L1 --storage=sqlite:///optuna_grid_results/L1/my_grid_L1_study.db --allow_tf32=True --nhwc=True
   ```

   **Command for Executing L2 Training**

   ```bash
   python train-optuna-TPESampler.py optimize --outdir=./optuna_grid_results --data=./dataset/L2-converted.zip --resume=./model/ffhq-res256-mirror-paper256-noaug.pkl --gpus=1 --study-name=my_grid_study_L2 --storage=sqlite:///optuna_grid_results/L2/my_grid_L2_study.db --allow_tf32=True --nhwc=True
   ```

   **Command for Executing L3 Training**

   ```bash
   python train-optuna-TPESampler.py optimize --outdir=./optuna_grid_results --data=./dataset/L3-converted.zip --resume=./model/ffhq-res256-mirror-paper256-noaug.pkl --gpus=1 --study-name=my_grid_study_L3 --storage=sqlite:///optuna_grid_results/L3/my_grid_L3_study.db --allow_tf32=True --nhwc=True
   ```

### 🖼️ Step-by-Step Guide to Generate Images

After the training is complete, follow these steps to generate images using the trained models.

1. **Locate the Trained Model 🔍**

   Identify the location of your completed L1, L2, or L3 trained model:

   ```bash
   ./model-trained/
   ```

2. **Generate Images 🎨**

   Load the networks and generate the images using the commands below.

   **Baseline Networks**

   ```bash
   python generate.py --outdir=./hasil-output/noaug/L1 --trunc=0.7 --seeds=0-10 --network=./model-trained/model-noaug/L1-noaug-network-snapshot-001000.pkl

   python generate.py --outdir=./hasil-output/noaug/L2 --trunc=0.7 --seeds=0-10 --network=./model-trained/model-noaug/L2-noaug-network-snapshot-001000.pkl

   python generate.py --outdir=./hasil-output/noaug/L3 --trunc=0.7 --seeds=0-10 --network=./model-trained/model-noaug/L3-noaug-network-snapshot-001000.pkl

   ```

   **Baseline + ADA Networks**

   ```bash
   python generate.py --outdir=./hasil-output/ada/L1 --trunc=0.7 --seeds=0-10 --network=./model-trained/model-ada/L1-network-snapshot-001000.pkl

   python generate.py --outdir=./hasil-output/ada/L2 --trunc=0.7 --seeds=0-10 --network=./model-trained/model-ada/L2-network-snapshot-001000.pkl

   python generate.py --outdir=./hasil-output/ada/L3 --trunc=0.7 --seeds=0-10 --network=./model-trained/model-ada/L3-network-snapshot-001000.pkl
   ```

   **Finetuning + ADA Networks**

   ```bash
   python generate.py --outdir=./hasil-output/finetuning/L1 --trunc=0.7 --seeds=0-10 --network=./model-trained/model-finetuning/L1-Finetune-1000.pkl

   python generate.py --outdir=./hasil-output/finetuning/L2 --trunc=0.7 --seeds=0-10 --network=./model-trained/model-finetuning/L2-Finetune-1000.pkl

   python generate.py --outdir=./hasil-output/finetuning/L3 --trunc=0.7 --seeds=0-10 --network=./model-trained/model-finetuning/L3-Finetune-1000.pkl
   ```

<!-- ## 📚 Dokumentasi Tambahan

- [![Dokumentasi API]](docs/api.md)
- [![Diagram Arsitektur]](docs/architecture.png)
- [![Struktur Basis Data]](docs/database_schema.sql) -->

## 🌐 Website Visual Turing Test

This section provides instructions for running the website visual Turing test.

1. **Go to the Frontend Folder 📁**

   Navigate to the frontend directory from your command line:

   ```bash
   cd frontend
   ```

2. **Install Node Modules 📦**

   Install the necessary node modules for the website. While `npm install` works, we recommend using `pnpm install` for faster installation:

   ```bash
   npm install
   # Recommended:
   pnpm install
   ```

3. **Run the Website Locally 🚀**

   Start the website development server on your local machine using the following command:

   ```bash
   npm run dev
   # Recommended:
   pnpm run dev
   ```

## 📊 Classification Metric

This section guides you on running the classification metric. While you can run the notebook locally, we highly recommend using Kaggle Notebooks for this task. Kaggle provides a pre-configured environment with necessary dependencies and often offers GPU access, making it ideal for running these metrics without complex local setup.

**Recommended: Using Kaggle Notebooks 🚀**

1. **Access the Kaggle Notebook 🔗**

   Open the classification metric notebook directly on Kaggle. Please ensure you select version 14 of the notebook for proper execution:
   [Kaggle Notebook](https://www.kaggle.com/code/cloudimsss/classification-metrics)

2. **Run the Notebook ▶️**

   Once on the Kaggle page, click on "Copy & Edit" to create your own editable version of the notebook. You can then run all cells sequentially to compute the classification metrics.

**Alternative: Running Locally (Advanced) 💻**

If you prefer to run the classification metric notebook on your local machine, follow these steps:

1. **Navigate to the Classification Metric Folder 📁**

   Change your directory to the `classification-metric` folder:

   ```bash
   cd classification-metric
   ```

2. **Set Up and Activate the Virtual Environment 🐍**

   Create a Python virtual environment and activate it.

   ```bash
   # Create the virtual environment (replace <venv_name> with your preferred name)
   python -m venv <venv_name>

   # Activate the virtual environment
   # For macOS/Linux:
   source <venv_name>/bin/activate

   # For Windows (Command Prompt):
   <venv_name>\Scripts\activate

   # For Windows (PowerShell):
   <venv_name>\Scripts\Activate.ps1
   ```

3. **Run the Notebook 📖**

   Install `ipykernel` and `notebook` (or jupyterlab) to run the `.ipynb` file within your newly activated virtual environment. Then, launch the notebook environment.

   ```bash
   # Install ipykernel (to ensure the venv can be used in Jupyter)
   pip install ipykernel

   # Install notebook/Jupyter
   pip install notebook

   # OR, if you prefer JupyterLab:
   # pip install jupyterlab

   # Start the notebook server
   jupyter notebook
   ```

   This command will open the Jupyter interface in your browser, where you can select and run the .ipynb file.

## ✅ Validasi

Pastikan proyek memenuhi kriteria berikut sebelum submit:

- Source code dapat di-build/run tanpa error
- Video demo jelas menampilkan fitur utama
- README lengkap dan terupdate
- Tidak ada data sensitif (password, API key) yang ter-expose

---

## ⁉️ Pertanyaan?

Hubungi:

- Penulis: [dprihadisetiawan@gmail.com]
<!-- - Pembimbing Utama: [email@pembimbing] -->
