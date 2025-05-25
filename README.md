# 🤖 Building RAG Agents with LLMs

![Python](https://img.shields.io/badge/python-3.10-blue.svg)
![LangChain](https://img.shields.io/badge/langchain-v0.1.0-green)
![License](https://img.shields.io/github/license/yourname/yourrepo)
![Stars](https://img.shields.io/github/stars/yourname/yourrepo?style=social)

---

## 📚 Mục lục

- [🚀 Giới thiệu](#-giới-thiệu
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

---

## 📎 Hướng dẫn từng bước

### 🔹 Step 1: Tìm bài báo trên arXiv

1. Truy cập [https://arxiv.org](https://arxiv.org) và tìm một bài báo mới trong vòng **1 tháng** gần đây.
2. Ghi lại mã số của bài báo (VD: `2505.00040`).

Ví dụ:

📌 **Lưu ý**: Chỉ cần mã số arXiv, không cần tải bài viết.

<img src="https://github.com/user-attachments/assets/6b236d2a-d686-430a-8726-d52916b5bb17" alt="step1" width="500"/>
<img src="https://github.com/user-attachments/assets/dc79a7f8-c4ce-404e-81c6-ebc3d01d2041" alt="step1-2" width="500"/>

---

### 🔹 Step 2: Vector hoá với notebook `07_vectorstores.ipynb`

1. Mở notebook `07_vectorstores.ipynb`.
2. Tìm đoạn code sau:

```python
docs = [
    ArxivLoader(query="2505.00040").load(),
]

### 🔹 Step 3: Khởi tạo API với notebook `09_server_app.ipynb`

1. Mở notebook `09_server_app.ipynb`.

2. Tìm đến cell như bên dưới, nơi khởi tạo chuỗi QA từ index:

   <img src="https://github.com/user-attachments/assets/08ad2191-076b-457a-aa36-cca008c645c4" alt="step3-1" width="500"/>

3. Thay thế toàn bộ nội dung trong cell đó bằng đoạn mã mới từ link:

   📄 [🔗 Link đến đoạn mã cập nhật QAChain](https://docs.google.com/document/d/1zmPgGvXUaWOG6CPtOg_6YRj2URT7hFIpZa8tFExy8Bk/edit?usp=sharing)

4. Chạy cell tiếp theo (như ảnh dưới) để tạo file `server_app.py`:

   <img src="https://github.com/user-attachments/assets/746a4ffb-0299-4e37-b575-af5da1d41cc4" alt="step3-2" width="500"/>

📦 **Kết quả:** Một file `server_app.py` được sinh ra, đóng vai trò backend API cho ứng dụng Gradio ở bước sau.

---

### 🔹 Step 4: Giao diện Gradio với notebook `08_gradio_ui.ipynb`

1. Mở notebook `08_gradio_ui.ipynb`.

2. Chạy cell có chứa mã khởi tạo giao diện Gradio:

   <img src="https://github.com/user-attachments/assets/a5a79cd4-be09-4034-b808-b009bac477b9" alt="step4" width="500"/>

3. Sau khi chạy xong, Gradio sẽ sinh ra một đường dẫn:

   - Bấm vào link để truy cập giao diện người dùng.
   - Bắt đầu tương tác với model bằng cách nhập câu hỏi về bài báo đã nạp.

4. Sau khi thử nghiệm thành công, bấm vào nút **Access Task** như hình dưới để xác nhận hoàn tất task:

   <img src="https://github.com/user-attachments/assets/dca604a1-cd30-4bdf-9f67-c8e700194db8" alt="access-task" width="500"/>

---

🚀 Vậy là bạn đã hoàn thành toàn bộ quy trình xây dựng RAG Agent tích hợp giao diện người dùng. Nếu thấy hữu ích, hãy ⭐ repo hoặc follow tác giả nhé!



