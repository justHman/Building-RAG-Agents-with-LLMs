# 🤖 Building RAG Agents with LLMs

![Python](https://img.shields.io/badge/python-3.10-blue.svg)
![LangChain](https://img.shields.io/badge/langchain-v0.1.0-green)
![License](https://img.shields.io/github/license/yourname/yourrepo)
![Stars](https://img.shields.io/github/stars/yourname/yourrepo?style=social)

---

## 📚 Mục lục

- [🚀 Giới thiệu](#-giới-thiệu)
- [📦 Yêu cầu hệ thống](#-yêu-cầu-hệ-thống)
- [🔧 Cài đặt](#-cài-đặt)
- [📎 Hướng dẫn từng bước](#-hướng-dẫn-từng-bước)
  - [🔹 Step 1: Tìm bài báo trên arXiv](#-step-1-tìm-bài-báo-trên-arxiv)
  - [🔹 Step 2: Vector hoá với notebook 07](#-step-2-vector-hoá-với-notebook-07)
  - [🔹 Step 3: Khởi tạo API với notebook 09](#-step-3-khởi-tạo-api-với-notebook-09)
  - [🔹 Step 4: Giao diện Gradio với notebook 08](#-step-4-giao-diện-gradio-với-notebook-08)
- [✅ Demo](#-demo)
- [📄 Giấy phép](#-giấy-phép)

---

## 🚀 Giới thiệu

Dự án hướng dẫn xây dựng một hệ thống **RAG Agent (Retrieval-Augmented Generation)** kết hợp mô hình ngôn ngữ lớn (LLM) với thông tin từ các bài báo khoa học trên arXiv. Toàn bộ quá trình được thực hiện trong Jupyter Notebook và giao diện được triển khai bằng Gradio.

---

## 📦 Yêu cầu hệ thống

- Python 3.10+
- pip
- Các thư viện:
  - `langchain`, `openai`, `arxiv`, `gradio`, `chromadb`, ...
- Jupyter Notebook
- CUDA (nếu muốn tăng tốc bằng GPU)

---

## 🔧 Cài đặt

```bash
git clone https://github.com/yourname/yourrepo.git
cd yourrepo
pip install -r requirements.txt
