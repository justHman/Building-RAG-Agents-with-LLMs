🚀 Building RAG Agents with LLMs
🔹 Step 1: Tìm bài báo mới nhất trong vòng 1 tháng
Tìm một bài báo mới trên arXiv (trong vòng 1 tháng gần đây).

Ví dụ kết quả:

less
Sao chép
Chỉnh sửa
[1] arXiv:2505.00040 [pdf, html, other]
Lưu ý: Ghi lại ID bài báo (ví dụ: 2505.00040) để sử dụng ở bước tiếp theo.




🔹 Step 2: Chạy các cell ở notebook 07_vectorstores.ipynb
Tại Task 1 trong notebook, thêm ID của bài báo đã tìm được ở Step 1 vào dòng sau:

python
Sao chép
Chỉnh sửa
ArxivLoader(query="2505.00040").load(),
👉 Ví dụ:

python
Sao chép
Chỉnh sửa
docs = [
    ArxivLoader(query="2505.00040").load(),
    # các bài báo khác nếu muốn
]


Sau đó chạy toàn bộ các cell còn lại để tạo ra file docstore_index.tgz trong thư mục chứa các notebook và thư mục src.

🔹 Step 3: Vào notebook 09_server_app.ipynb
Tìm phần như hình dưới:



Thay nội dung cell đó bằng đoạn code trong link sau:

📄 Link code thay thế cell

Sau đó chạy tiếp cell để tạo ra file server_app.py:



🔹 Step 4: Vào notebook 08_gradio_ui.ipynb
Chạy cell phía dưới phần như hình để tạo ra frontend với Gradio:



Sau khi chạy xong sẽ có một link 👉 click vào link đó để truy cập frontend.

Bạn có thể Evaluate model, và sau đó bấm Access Task như hình dưới:



✅ Nếu bạn làm xong các bước trên thành công, cho mình một follow nhé 😄!
