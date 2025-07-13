# 🏁 Tugas Akhir (TA) - Final Project

**Nama Mahasiswa**: Dimas Prihady Setyawan

**NRP**: 5025211184

**Judul TA**: PENERAPAN GENERATIVE ADVERSARIAL NETWORKS DALAM GENERASI DATA SINTETIS SEL DARAH PUTIH KASUS LEUKEMIA LIMFOBLASTIK AKUT DAN EVALUASI KINERJA MODEL KLASIFIKASI

**Dosen Pembimbing**: Prof. Dr. Eng. Chastine Fatichah, S.Kom., M.Kom.

**Dosen Ko-pembimbing**: Aldinata Rizky Revanda, S.Kom., M.Kom.

---

## 📺 Demo Aplikasi

Embed video demo di bawah ini (ganti `VIDEO_ID` dengan ID video YouTube Anda):

[![Demo Aplikasi](https://i.ytimg.com/vi/zIfRMTxRaIs/maxresdefault.jpg)](https://www.youtube.com/watch?v=VIDEO_ID)  
_Klik gambar di atas untuk menonton demo_

---

_Konten selanjutnya hanya merupakan contoh awalan yang baik. Anda dapat berimprovisasi bila diperlukan._

## 🛠 Software Installation & Execution Guide

This guide provides instructions for installing the necessary software, setting up the environment, and running the training and image generation processes.

### Requirements

To run this project, you need:

<!-- - Daftar dependensi (contoh):
  - Python 3.10+
  - Node.js v18+
  - MySQL 8.0
  - [Lainnya...] -->

- Docker Desktop

### Step by Step to Run the Training Process

Follow these steps to set up the environment and start the training process:

1. **Clone Repository**

   Clone the project repository to your local machine:

   ```bash
   git clone https://github.com/Informatics-ITS/ta-yaboidimsum.git
   ```

2. **Login to Docker Account**

   Log in to your Docker account via the terminal:

   ```bash
   docker login
   ```

3. **Build the Docker Image**

   Navigate to your project directory and build the Docker image. Replace `<image_name>` and `<tag>` with appropriate names for your image:

   ```bash
   cd [folder-proyek]
   docker build -t <image_name>:<tag> .
   ```

4. **Run the Container**

   Run the Docker container on a specific port. This command uses docker run to start the container, mount the current directory, and map port 6006 for potential access (e.g., if using TensorBoard).

   ```bash
   docker run --gpus all --rm -it -v "${PWD}:/stylegan2-ada-pytorch" -p 6006:6006 <your docker image> /bin/sh
   ```

5. **Navigate Inside the Container**

   Once inside the container, navigate to the mounted folder. You start in `/workspace` and need to move to the stylegan2-ada-pytorch directory:

   ```bash
   cd ..
   cd stylegan2-ada-pytorch
   ```

6. **Install Additional Libraries**

   Install the Optuna library, which is required for hyperparameter tuning:

   ```bash
   pip install optuna
   ```

7. **Run the Training Process**

   Use the appropriate command to run the L1, L2, or L3 training process.

   **Command for Executing L1 Training**

   ```bash
   python train-optuna.py optimize --outdir=./optuna_grid_results --data=./dataset/L1-Converted --resume=./model/ffhq-res256-mirror-paper256-noaug.pkl --gpus=1 --study-name=my_grid_study_L1 --storage=sqlite:///optuna_grid_results/my_grid_L1_study.db --allow-tf32=True --nhwc=True
   ```

   **Command for Executing L2 Training**

   ```bash
   python train-optuna.py optimize --outdir=./optuna_grid_results --data=./dataset/L2-converted.zip --resume=./model/ffhq-res256-mirror-paper256-noaug.pkl --gpus=1 --study-name=my_grid_study_L2 --storage=sqlite:///optuna_grid_results/my_grid_L2_study.db --allow-tf32=True --nhwc=True
   ```

   **Command for Executing L3 Training**

   ```bash
   python train-optuna.py optimize --outdir=./optuna_grid_results --data=./dataset/L3-Converted --resume=./model/ffhq-res256-mirror-paper256-noaug.pkl --gpus=1 --study-name=my_grid_study_L3 --storage=sqlite:///optuna_grid_results/my_grid_L3_study.db --allow-tf32=True --nhwc=True
   ```

### Step by Step to Generate The Images

After the training is complete, follow these steps to generate images using the trained models.

1. **Identify the location of your completed L1, L2, or L3 trained model.**

   Identify the location of your completed L1, L2, or L3 trained model:

   ```bash
   ./model-trained/
   ```

2. **Load Networks and Generate Images**

   Load the networks and generate the images using the commands below.

   **Baseline Networks**

   ```bash
   !python generate.py --outdir=./hasil-output/noaug/L1 --trunc=0.7 --seeds=0-10 \
    --network=./model-trained/model-noaug/L1-noaug-network-snapshot-001000.pkl

   !python generate.py --outdir=./hasil-output/noaug/L2 --trunc=0.7 --seeds=0-10 \
    --network=./model-trained/model-noaug/L2-noaug-network-snapshot-001000.pkl

   !python generate.py --outdir=./hasil-output/noaug/L3 --trunc=0.7 --seeds=0-10 \
    --network=./model-trained/model-noaug/L3-noaug-network-snapshot-001000.pkl

   ```

   **Baseline + ADA Networks**

   ```bash
   !python generate.py --outdir=./hasil-output/ada/L1 --trunc=0.7 --seeds=0-10 \
    --network=./model-trained/model-ada/L1-network-snapshot-001000.pkl

   !python generate.py --outdir=./hasil-output/ada/L2 --trunc=0.7 --seeds=0-10 \
      --network=./model-trained/model-ada/L2-network-snapshot-001000.pkl

   !python generate.py --outdir=./hasil-output/ada/L3 --trunc=0.7 --seeds=0-10 \
      --network=./model-trained/model-ada/L3-network-snapshot-001000.pkl
   ```

   **Finetuning + ADA Networks**

   ```bash
   !python generate.py --outdir=./hasil-output/finetuning/L1 --trunc=0.7 --seeds=0-10 \
    --network=./model-trained/model-finetuning/L1-Finetune-1000.pkl

   !python generate.py --outdir=./hasil-output/finetuning/L2 --trunc=0.7 --seeds=0-10 \
      --network=./model-trained/model-finetuning/L2-Finetune-1000.pkl

   !python generate.py --outdir=./hasil-output/finetuning/L3 --trunc=0.7 --seeds=0-10 \
      --network=./model-trained/model-finetuning/L3-Finetune-1000.pkl
   ```

## 📚 Dokumentasi Tambahan

- [![Dokumentasi API]](docs/api.md)
- [![Diagram Arsitektur]](docs/architecture.png)
- [![Struktur Basis Data]](docs/database_schema.sql)

---

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
- Pembimbing Utama: [email@pembimbing]
