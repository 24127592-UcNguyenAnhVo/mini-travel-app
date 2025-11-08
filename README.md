# Báo cáo [LAB] Mini-Travel Application

Ứng dụng Streamlit lập kế hoạch du lịch, sử dụng Ollama làm LLM server và Firebase để xác thực người dùng (Login) và lưu lịch sử (Chat History).

---

## 🛑 YÊU CẦU QUAN TRỌNG TRƯỚC KHI CHẠY

Để chạy được code, Giảng viên/TA cần phải có file `chatbot_tour_key.json` (file key bí mật của Firebase). File này không được đưa lên Github vì lý do bảo mật.

Vui lòng thực hiện các bước sau để tạo file key của riêng bạn:

### 1. Tạo Project Firebase
1.  Tạo một dự án mới trên [Firebase Console](https://console.firebase.google.com/).
2.  **Kích hoạt Authentication:** Vào **Build** > **Authentication** > **Get started** > Chọn **Email/Password** và Bật (Enable).
3.  **Kích hoạt Firestore:** Vào **Build** > **Firestore Database** > **Create database** > Chọn **Start in test mode** (Bắt đầu ở chế độ thử nghiệm) và Bật (Enable).
    * *Lưu ý: Nếu chọn "test mode", cần chỉnh sửa Rules để cho phép đọc/ghi (allow read, write: if true;)*

### 2. Tạo File Key
1.  Trong dự án Firebase, đi đến **Project settings** (biểu tượng ⚙️) > **Service accounts**.
2.  Nhấn nút **"Generate new private key"** và tải file `.json` về.
3.  **Đổi tên** file vừa tải về thành chính xác `chatbot_tour_key.json`.

### 3. Tạo Index (Bắt buộc để xem History)
1.  Trong **Firestore Database**, chọn tab **"Indexes"**.
2.  Nhấn **"Create index"**.
3.  **Collection ID:** `trips`
4.  **Fields to index (Thêm 2 trường):**
    * Trường 1: `user` (Order: `Ascending`)
    * Trường 2: `timestamp` (Order: `Descending`)
5.  Nhấn **Create** và chờ cho đến khi Status chuyển thành "Enabled" (việc này mất vài phút).

---

## 🚀 Hướng dẫn chạy Code (Google Colab)

1.  Mở file `.ipynb` (file code chính) trong Google Colab.
2.  Ở thanh công cụ bên trái, chọn **Files (📁)**.
3.  Nhấn nút **"Upload"** (Tải lên) và tải file `chatbot_tour_key.json` (đã tạo ở bước 2) lên.
4.  **Chạy tất cả các cell code** trong file notebook theo thứ tự từ trên xuống dưới (Cell 1 đến 5).
5.  Link web public của ứng dụng (có đuôi `.trycloudflare.com`) sẽ xuất hiện ở output của **Cell 5**.
