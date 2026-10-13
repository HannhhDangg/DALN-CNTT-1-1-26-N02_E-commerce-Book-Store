# 🛠️ CẨM NANG LỆNH TERMINAL & THAO TÁC DỰ ÁN — TỔ SÁCH
> **Tài liệu nội bộ dành cho Developer / Tác giả đồ án**  
> ⚠️ **TUYỆT ĐỐI KHÔNG PUSH LÊN GIT (Đã cấu hình trong `.gitignore`)**  
> Chứa toàn bộ các câu lệnh terminal cần thiết, phân loại rõ ràng theo từng phân hệ (Database, Server, Kiểm thử, Git, Khắc phục sự cố).

---

## 📑 MỤC LỤC
1. [Phân Hệ 1: Cơ Sở Dữ Liệu & Prisma ORM (Database)](#phân-hệ-1-cơ-sở-dữ-liệu--prisma-orm-database)
2. [Phân Hệ 2: Khởi Động & Vận Hành Ứng Dụng (Next.js Server)](#phân-hệ-2-khởi-động--vận-hành-ứng-dụng-nextjs-server)
3. [Phân Hệ 3: Kiểm Tra Lỗi & Chất Lượng Mã Nguồn (Type Safety & Linter)](#phân-hệ-3-kiểm-tra-lỗi--chất-lượng-mã-nguồn-type-safety--linter)
4. [Phân Hệ 4: Quản Lý Phiên Bản Git (Version Control)](#phân-hệ-4-quản-lý-phiên-bản-git-version-control)
5. [Phân Hệ 5: Khắc Phục Sự Cố & Xóa Cache (Troubleshooting)](#phân-hệ-5-khắc-phục-sự-cố--xóa-cache-troubleshooting)

---

# PHÂN HỆ 1: CƠ SỞ DỮ LIỆU & PRISMA ORM (DATABASE)
*Áp dụng khi: Thao tác với PostgreSQL trên Supabase Cloud, xem dữ liệu, sửa schema, nạp dữ liệu mẫu.*

### 1.1 Khởi chạy giao diện đồ họa quản trị CSDL (Prisma Studio)
* **Lệnh:**
  ```bash
  npx prisma studio
  ```
* **Mục đích:** Mở giao diện web cục bộ tại `http://localhost:5555`.
* **Tính năng:**
  - Hiển thị đầy đủ 14 bảng quan hệ (`User`, `Book`, `Category`, `Order`, `Review`...).
  - Xem dữ liệu dạng bảng tính (như Excel/Airtable).
  - Có thanh **Search** tìm kiếm nhanh và nút **Filter** lọc theo trường dữ liệu.
  - Thêm bản ghi mới (**Add record**), sửa trực tiếp trên ô, xóa dòng dữ liệu.
  - Bấm vào trường quan hệ để nhảy sang xem bảng liên kết.
* **Cách tắt:** Tại cửa sổ terminal đang chạy lệnh, bấm tổ hợp phím `Ctrl + C`.

---

### 1.2 Đồng bộ Schema từ file `schema.prisma` lên Supabase
* **Lệnh:**
  ```bash
  npx prisma db push
  ```
* **Mục đích:** Mỗi khi bạn chỉnh sửa, thêm bảng hoặc thêm cột mới trong file [`prisma/schema.prisma`](file:///e:/to-sach-studio/prisma/schema.prisma), chạy lệnh này để đẩy cấu trúc mới lên PostgreSQL Supabase ngay lập tức mà không làm mất dữ liệu cũ (nếu không có xung đột).

---

### 1.3 Sinh lại mã nguồn Prisma Client (Generate Types)
* **Lệnh:**
  ```bash
  npx prisma generate
  ```
* **Mục đích:** Cập nhật lại thư viện `@prisma/client` và các định nghĩa kiểu TypeScript (`type definitions`) sau khi thay đổi schema, giúp VS Code gợi ý code (IntelliSense) chuẩn xác 100%.

---

### 1.4 Nạp dữ liệu mẫu ban đầu (Seed Data)
* **Lệnh:**
  ```bash
  npx prisma db seed
  ```
* **Mục đích:** Chạy script [`prisma/seed.ts`](file:///e:/to-sach-studio/prisma/seed.ts) để nạp tự động: 6 quyền hạn hệ thống, 4 tài khoản mẫu (`admin`, `staff kho`, `staff đơn`, `customer`), 8 cuốn sách thật, cây thể loại 3 tầng và đơn hàng mẫu.

---

### 1.5 Tự động căn chỉnh & Định dạng file `schema.prisma`
* **Lệnh:**
  ```bash
  npx prisma format
  ```
* **Mục đích:** Tự động căn lề thẳng hàng các cột, kiểm tra cú pháp và tự động thêm các quan hệ nghịch đảo (relation fields) bị thiếu trong file schema.

---

### 1.6 Kéo cấu trúc bảng từ Supabase về máy (Introspection)
* **Lệnh:**
  ```bash
  npx prisma db pull
  ```
* **Mục đích:** Nếu bạn tạo bảng hoặc sửa cột trực tiếp trên giao diện web của Supabase, chạy lệnh này để Prisma tự động kéo cấu trúc đó về cập nhật vào file `schema.prisma` ở local.

---

### 1.7 Mẫu truy vấn SQL nhanh trên Supabase SQL Editor
*Truy cập [Supabase Dashboard](https://supabase.com/dashboard) $\rightarrow$ Vào mục **SQL Editor** $\rightarrow$ Dán các câu lệnh sau để kiểm tra:*

```sql
-- 1. Xem danh sách sách, giá bán và tồn kho
SELECT title, price, original_price, stock_qty, sold_count, is_active FROM books;

-- 2. Xem danh sách tài khoản, vai trò và trạng thái
SELECT email, full_name, role, status, created_at FROM users;

-- 3. Đếm số lượng sách thực tế theo từng ngành hàng chính (Level 1)
SELECT c.name AS the_loai, COUNT(bc.book_id) AS so_luong_sach
FROM categories c
LEFT JOIN book_categories bc ON c.id = bc.category_id
WHERE c.level = 1
GROUP BY c.name
ORDER BY so_luong_sach DESC;

-- 4. Xem danh sách đơn hàng mới nhất
SELECT order_code, customer_name, total_amount, payment_method, payment_status, status, created_at 
FROM orders 
ORDER BY created_at DESC;
```

---

# PHÂN HỆ 2: KHỞI ĐỘNG & VẬN HÀNH ỨNG DỤNG (NEXT.JS SERVER)
*Áp dụng khi: Bật web để xem giao diện, đổi cổng mạng, cài đặt thư viện mới.*

### 2.1 Khởi động môi trường phát triển (Dev Server)
* **Lệnh:**
  ```bash
  npm run dev
  ```
* **Mục đích:** Bật server Next.js 15 ở chế độ Development với tính năng Hot-Reload (sửa code là màn hình tự cập nhật tức thì).
* **Địa chỉ truy cập:** `http://localhost:3000`.

---

### 2.2 Chạy server trên cổng mạng khác (Khi cổng 3000 bị chiếm dụng)
* **Lệnh:**
  ```bash
  npx next dev -p 3001
  ```
* **Mục đích:** Nếu máy tính của bạn đang có ứng dụng khác chạy ở cổng 3000, lệnh này sẽ mở website tại cổng `3001` (`http://localhost:3001`).

---

### 2.3 Build kiểm tra đóng gói Production
* **Lệnh:**
  ```bash
  npm run build
  ```
* **Mục đích:** Giả lập quá trình biên dịch toàn bộ dự án trước khi đưa lên mạng thật (Vercel). Giúp phát hiện các lỗi cú pháp, lỗi render trang tĩnh và kiểm tra dung lượng bundle size.

---

### 2.4 Chạy ứng dụng sau khi đã Build Production
* **Lệnh:**
  ```bash
  npm run start
  ```
* **Mục đích:** Chạy website ở chế độ tối ưu hiệu năng cao nhất (sau khi đã chạy lệnh `npm run build` thành công).

---

### 2.5 Cài đặt thêm thư viện mới
* **Cài thư viện cho ứng dụng (Production Dependencies):**
  ```bash
  npm install <ten-thu-vien>
  ```
  *(Ví dụ: `npm install @google/genai`)*
* **Cài thư viện chỉ dùng lúc lập trình (Dev Dependencies):**
  ```bash
  npm install -D <ten-thu-vien>
  ```
  *(Ví dụ: `npm install -D @types/node`)*

---

# PHÂN HỆ 3: KIỂM TRA LỖI & CHẤT LƯỢNG MÃ NGUỒN (TYPE SAFETY & LINTER)
*Áp dụng khi: Muốn rà soát toàn bộ dự án xem có dòng code nào bị lỗi TypeScript hoặc sai chuẩn không.*

### 3.1 Kiểm tra lỗi toàn bộ dự án TypeScript (Không sinh file rác)
* **Lệnh:**
  ```bash
  npx tsc --noEmit
  ```
* **Mục đích:** Trình biên dịch TypeScript sẽ quét 100% tất cả các file `.ts`, `.tsx` trong dự án:
  - Nếu terminal chạy xong và **không hiện dòng chữ nào (Exit code 0)** $\rightarrow$ Dự án hoàn hảo, **0 lỗi**!
  - Nếu có lỗi, terminal sẽ chỉ rõ chính xác tên file, số dòng và lý do bị lỗi.

---

### 3.2 Quét lỗi cú pháp và quy chuẩn mã nguồn (ESLint)
* **Lệnh:**
  ```bash
  npm run lint
  ```
* **Mục đích:** Kiểm tra các biến khai báo mà không dùng, lỗi cấu trúc React Hook, hoặc lỗi cú pháp HTML.

---

### 3.3 Kiểm tra phiên bản môi trường máy
* **Lệnh:**
  ```bash
  node -v
  npm -v
  ```
* **Mục đích:** Đảm bảo máy tính đang chạy phiên bản Node.js >= 18.x (khuyến nghị 20.x LTS) để tương thích tốt nhất với Next.js 15.

---

# PHÂN HỆ 4: QUẢN LÝ PHIÊN BẢN GIT (VERSION CONTROL)
*Áp dụng khi: Kiểm tra tiến độ file, tạo mốc lưu trữ an toàn, đồng bộ lên GitHub.*

### 4.1 Kiểm tra trạng thái các file đang thay đổi
* **Lệnh ngắn gọn (Khuyên dùng):**
  ```bash
  git status -s
  ```
* **Quy ước ký hiệu hiển thị:**
  - `M`: File đã bị chỉnh sửa (Modified).
  - `??`: File mới tạo chưa được Git theo dõi (Untracked).
  - `D`: File đã bị xóa (Deleted).
  - `A`: File đã được thêm vào vùng chuẩn bị commit (Staged).

---

### 4.2 Xem chi tiết nội dung code vừa sửa trước khi lưu
* **Lệnh:**
  ```bash
  git diff
  ```
* **Mục đích:** Hiển thị từng dòng code: màu đỏ (`-`) là dòng bị xóa, màu xanh (`+`) là dòng mới viết thêm. Bấm phím `q` để thoát xem diff.

---

### 4.3 Đóng gói và lưu tiến độ (Commit)
* **Bước 1 — Đưa các file đã thay đổi vào vùng chờ:**
  ```bash
  git add .
  ```
  *(Lệnh này tuân thủ 100% file `.gitignore`, không bao giờ thêm các file mật khẩu `.env`, code cũ `legacy/` hay tài liệu nội bộ).*
* **Bước 2 — Tạo mốc lưu trữ kèm lời chú thích rõ ràng:**
  ```bash
  git commit -m "feat: hoàn thiện mốc báo cáo tuần 2 chuẩn figma và auth"
  ```

---

### 4.4 Xem lịch sử các mốc đã lưu
* **Lệnh:**
  ```bash
  git log --oneline -n 10
  ```
* **Mục đích:** Xem danh sách 10 commit gần nhất kèm mã hash và lời ghi chú ngắn gọn.

---

### 4.5 Đẩy code lên GitHub Remote
* **Lệnh:**
  ```bash
  git push origin main
  ```
* **Mục đích:** Đồng bộ toàn bộ các commit ở máy local lên kho lưu trữ GitHub chính thức: `https://github.com/Hanh-Dang/DALN-CNTT-1-1-26-N02_To-Sach.git`.

---

### 4.6 Hủy bỏ các thay đổi chưa lưu của một file (Revert File)
* **Lệnh:**
  ```bash
  git checkout -- <duong-dan-file>
  ```
  *Hoặc trên Git mới:*
  ```bash
  git restore <duong-dan-file>
  ```
* **Mục đích:** Khôi phục file về nguyên trạng của commit gần nhất, hủy bỏ các chỉnh sửa nhầm lẫn.

---

# PHÂN HỆ 5: KHẮC PHỤC SỰ CỐ & XÓA CACHE (TROUBLESHOOTING)
*Áp dụng khi: Gặp lỗi lạ về cache, đơ cổng mạng hoặc muốn làm sạch dự án.*

### 5.1 Xóa bộ nhớ đệm (Cache) của Next.js
*Khi gặp hiện tượng sửa code mà giao diện không nhận hoặc bị lỗi Hydration kỳ lạ:*
* **Trên PowerShell:**
  ```powershell
  Remove-Item -Recurse -Force .next
  ```
* **Trên Command Prompt (CMD):**
  ```cmd
  rmdir /s /q .next
  ```
* Sau khi xóa xong, chạy lại `npm run dev` để Next.js biên dịch lại sạch sẽ từ đầu.

---

### 5.2 Tắt cưỡng bức tiến trình đang chiếm cổng mạng (Port Kill)
*Khi báo lỗi `Port 3000 is in use` hoặc `Port 5555 is in use`:*
* **Lệnh tắt cổng 3000 (PowerShell):**
  ```powershell
  Stop-Process -Id (Get-NetTCPConnection -LocalPort 3000).OwningProcess -Force
  ```
* **Lệnh tắt cổng 5555 của Prisma Studio (PowerShell):**
  ```powershell
  Stop-Process -Id (Get-NetTCPConnection -LocalPort 5555).OwningProcess -Force
  ```

---

### 5.3 Cài đặt lại toàn bộ thư viện từ đầu (Clean Install)
*Khi bộ thư viện `node_modules` bị lỗi hỏng file:*
```powershell
Remove-Item -Recurse -Force node_modules
Remove-Item -Force package-lock.json
npm install
npx prisma generate
```
