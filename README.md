# 🤖 Building RAG Agents with LLMs

![Python](https://img.shields.io/badge/python-3.10-blue.svg)
![LangChain](https://img.shields.io/badge/langchain-v0.1.0-green)
![License](https://img.shields.io/github/license/yourname/yourrepo)
![Stars](https://img.shields.io/github/stars/yourname/yourrepo?style=social)    

---  

## 📚 Mục lục  

- [🚀 Giới thiệu](#-giới-thiệu) 
- [📎 Hướng dẫn từng bước](#-hướng-dẫn-từng-bước) 
  - [🔹 Step 1: Tìm bài báo trên arXiv](#-step-1-tìm-bài-báo-trên-arxiv)
  - [🔹 Step 2: Vector hoá với notebook 07](#-step-2-vector-hoá-với-notebook-07)
  - [🔹 Step 3: Khởi tạo API với notebook 09](#-step-3-khởi-tạo-api-với-notebook-09)
  - [🔹 Step 4: Giao diện Gradio với notebook 08](#-step-4-giao-diện-gradio-với-notebook-08)

---

## 🚀 Giới thiệu

Dự án hướng dẫn xây dựng một hệ thống **RAG Agent (Retrieval-Augmented Generation)** kết hợp mô hình ngôn ngữ lớn (LLM) với thông tin từ các bài báo khoa học trên arXiv. Toàn bộ quá trình được thực hiện trong Jupyter Notebook và giao diện được triển khai bằng Gradio.


---
## 📎 Hướng dẫn từng bước

# Step 1: Tìm bài báo mới nhất trong vòng 1 tháng!

![image](https://github.com/user-attachments/assets/6b236d2a-d686-430a-8726-d52916b5bb17)
![image](https://github.com/user-attachments/assets/dc79a7f8-c4ce-404e-81c6-ebc3d01d2041)

- Tìm đến phần này và bấm Launch
![image](https://github.com/user-attachments/assets/1fcfe2e5-a153-4c78-9cd2-206ae413699f)


# Step 2: Chạy các cell ở notebook 7 (07_vectorstores.ipynb)
![image](https://github.com/user-attachments/assets/a117e902-937e-4211-b414-a3dd63f048ff)

- Ở task 1: thêm ID của bài báo đã tìm ở step 1 vào cell trong hình
  + Ví dụ: [1] arXiv:2505.00040 [pdf, html, other]
    
        ArxivLoader(query="2505.00040").load(),

- Sau đó chạy các cell còn lại để tạo ra file docstore_index.tgz ở structure bên trái, nơi chứa các files notebook và src

# Step 3: Vào notebook 9 tìm phần hình dưới

![image](https://github.com/user-attachments/assets/08ad2191-076b-457a-aa36-cca008c645c4)

- Thay đổi code trong cell bằng code bên dưới và chạy cell đó 

Link code:
https://docs.google.com/document/d/1zmPgGvXUaWOG6CPtOg_6YRj2URT7hFIpZa8tFExy8Bk/edit?usp=sharing

- Liên hệ để cấp quyền:
  
![image](https://github.com/user-attachments/assets/e821f002-1b9a-4eca-9621-b18c26729e1d)

- Chạy tiếp cell sau để tạo ra file server_app.py:

![image](https://github.com/user-attachments/assets/746a4ffb-0299-4e37-b575-af5da1d41cc4)

# Step 4: Ở notebook 8 chạy cell dưới ảnh để tạo ra link frontend: Gradio

![image](https://github.com/user-attachments/assets/a5a79cd4-be09-4034-b808-b009bac477b9)
![image](https://github.com/user-attachments/assets/1ce08f59-d69e-4501-bfb9-65bbc7e8dbf6)


- Rồi bấm vào link và Evalue model 

![image](https://github.com/user-attachments/assets/ae74fc3a-8008-492b-a1f0-62028d62a199)


- Sau khi chạy xong -> Bấm Acesstask như hình dưới

![image](https://github.com/user-attachments/assets/dca604a1-cd30-4bdf-9f67-c8e700194db8)

---

## Nếu thành công ròi, cho xin 1 follow nhé :>

