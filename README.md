# Báo cáo [LAB] Mini-Travel Application

Ứng dụng Streamlit lập kế hoạch du lịch, sử dụng Ollama làm LLM server và Firebase để xác thực người dùng (Login) và lưu lịch sử (Chat History).

---

## 1. Tổng quan dự án

Bài nộp này là một file Google Colab (`.ipynb`) duy nhất. File này được thiết kế để:
1.  **Cài đặt** tất cả môi trường cần thiết (Ollama, Streamlit, Cloudflared).
2.  **Chạy Backend:** Khởi động một server AI (Ollama) và tải model `mistral`.
3.  **Chạy Frontend:** Viết và khởi động một ứng dụng web (Streamlit).
4.  **Public:** Dùng `cloudflared` để tạo 2 đường link web công cộng: một cho backend và một cho frontend.
5.  **Lưu trữ:** Dùng Firebase (Authentication và Firestore) để quản lý đăng nhập và lưu lịch sử chat.

---

## 2. Cấu trúc file `.ipynb` (Giải thích các cell)

* **Cell 1 - Cài đặt:**
    * Cài đặt server `ollama`.
    * Cài đặt `cloudflared` (để tạo link public).
    * Cài đặt các thư viện Python: `streamlit`, `ollama`, `firebase-admin`.

* **Cell 2 - Chạy Backend (Ollama):**
    * Khởi động server Ollama ở chế độ nền (`ollama serve`).

* **Cell 3 - Tải Model & Public Backend:**
    * Tải model AI `mistral` (`ollama pull mistral`).
    * Chạy `cloudflared` để tạo một link public (ví dụ: `...trycloudflare.com`) trỏ về server Ollama (đang chạy ở `localhost:11434`).
    * Tự động lưu link này vào file `ollama_url.txt` để Cell 4 sử dụng.

* **Cell 4 - Viết Frontend (app.py):**
    * Dùng lệnh `%%writefile app.py` để tạo ra file `app.py`.
    * File này chứa toàn bộ code của giao diện web (Streamlit).
    * Nó xử lý việc đăng nhập/đăng ký (Firebase Auth).
    * Nó đọc link backend từ file `ollama_url.txt`.
    * Nó gửi yêu cầu đến server AI và hiển thị kết quả.
    * Nó lưu/đọc lịch sử từ (Firebase Firestore).

* **Cell 5 - Chạy Frontend & Public:**
    * Chạy `streamlit run app.py` (file vừa tạo ở Cell 4).
    * Chạy `cloudflared` một lần nữa để tạo link public thứ hai, trỏ về app Streamlit (đang chạy ở `localhost:8501`).
    * **Đây là link web cuối cùng để bạn sử dụng ứng dụng.**

---

## 3. 🛑 YÊU CẦU QUAN TRỌNG (Cách chạy cho Giảng viên/TA)

Để chạy được code, Giảng viên/TA cần phải có file `chatbot_tour_key.json` (file key bí mật của Firebase). File này không được đưa lên Github vì lý do bảo mật.

Vui lòng thực hiện các bước sau để tạo file key của riêng bạn:

### 3.1. Tạo Project Firebase
1.  Tạo một dự án mới trên [Firebase Console](https://console.firebase.google.com/).
2.  **Kích hoạt Authentication:** Vào **Build** > **Authentication** > **Get started** > Chọn **Email/Password** và Bật (Enable).
3.  **Kích hoạt Firestore:** Vào **Build** > **Firestore Database** > **Create database** > Chọn **Start in test mode** (Bắt đầu ở chế độ thử nghiệm) và Bật (Enable).

### 3.2. Tạo File Key
1.  Trong dự án Firebase, đi đến **Project settings** (biểu tượng ⚙️) > **Service accounts**.
2.  Nhấn nút **"Generate new private key"** và tải file `.json` về.
3.  **Đổi tên** file vừa tải về thành chính xác `chatbot_tour_key.json`.

### 3.3. Tạo Index (Bắt buộc để xem History)
1.  Trong **Firestore Database**, chọn tab **"Indexes"**.
2.  Nhấn **"Create index"**.
3.  **Collection ID:** `trips`
4.  **Fields to index (Thêm 2 trường):**
    * Trường 1: `user` (Order: `Ascending`)
    * Trường 2: `timestamp` (Order: `Descending`)
5.  Nhấn **Create** và chờ cho đến khi Status chuyển thành "Enabled" (việc này mất vài phút).

---

## 4. 🚀 Hướng dẫn chạy (Google Colab)

Đây là cách chạy chuẩn cho file `.ipynb` này:

1.  Mở file `.ipynb` trong Google Colab.
2.  Ở thanh công cụ bên trái, chọn **Files (📁)**.
3.  Nhấn nút **"Upload"** (Tải lên) và tải file `chatbot_tour_key.json` (đã tạo ở bước 3.2) lên.
4.  **Chạy tất cả các cell code** trong file notebook theo thứ tự từ trên xuống dưới (Cell 1 đến 5).
5.  Link web public của ứng dụng (có đuôi `.trycloudflare.com`) sẽ xuất hiện ở output của **Cell 5**.
