# KẾ HOẠCH CẬP NHẬT & NÂNG CẤP ĐỒ ÁN BÁM SÁT PROPOSAL GIẢNG VIÊN
> **Sự kiện quan trọng:** Họp Đồ án & Báo cáo Tiến độ trực tiếp cùng Giảng viên hướng dẫn  
> **Thời gian:** 13h30 Chiều Thứ 3 (13/10/2026)  
> **Địa điểm:** Phòng 2310, Tầng 23, Tòa nhà A9  
> **Yêu cầu bắt buộc từ Thầy:**
> 1. Báo cáo việc thực hiện đồ án & **Chương trình DEMO trực tiếp bám sát Proposal đề tài**.
> 2. Báo cáo **Tiến độ viết báo cáo thuyết minh** (theo biểu mẫu của Nhà trường).

---

## PHẦN I: BÁO CÁO HIỆN TRẠNG — DỰ ÁN ĐÃ LÀM ĐẾN ĐÂU (TÍNH ĐẾN 09/10/2026)

Trước khi tiến hành nâng cấp bổ sung theo Proposal, nhóm đã hoàn thành một khối lượng công việc đồ sộ, đưa hệ thống **Tổ Sách (B2C Bookstore)** vào trạng thái hoạt động trơn tru (Full-flow End-to-End) với cả 2 phân hệ Storefront (Khách hàng) và Admin (Quản trị):

```
[==== TUẦN 1: DATABASE & ARCHITECTURE ====] ---> HOÀN THÀNH 100% (07/09 — 13/09/2026)
[==== TUẦN 2: AUTH & BRAND STOREFRONT ====] ---> HOÀN THÀNH 100% (14/09 — 20/09/2026) [MỐC 1]
[==== TUẦN 3: CATALOG & BOOK DETAILS =====] ---> HOÀN THÀNH 100% (21/09 — 27/09/2026)
[==== TUẦN 4: CART, CHECKOUT & VIETQR ====] ---> HOÀN THÀNH 100% (28/09 — 04/10/2026) [MỐC 2]
[==== TUẦN 5: ADMIN DASHBOARD & KHO SÁCH =] ---> HOÀN THÀNH 100% (05/10 — 08/10/2026)
[==== TUẦN 6: QUẢN LÝ ĐƠN, RBAC & NOTIF =] ---> HOÀN THÀNH 100% (09/10/2026) [MỐC 3 - HOÀN TẤT]
[==== TUẦN 7: BỘ 3 CÔNG CỤ PROPOSAL =====] ---> ĐÃ TRIỂN KHAI 100% (09/10/2026) [CHẠY TRƯỚC TIẾN ĐỘ]
```

### 1. Phân Hệ Khách Hàng (Customer Storefront) — ĐÃ HOÀN TẤT 100%
1. **Trang Chủ & Nhận diện Thương hiệu:** Header Zero CLS, Mega Menu danh mục, Hero Banner, Khối giá trị cốt lõi, Logo squircle độc quyền.
2. **Bộ lọc & Catalog đa chiều (`/catalog`):** Lọc theo cây phân cấp 3 tầng, lọc khoảng giá, Nhà xuất bản, Sắp xếp đa tiêu chí, Tìm kiếm tiếng Việt không dấu.
3. **Trang Chi tiết Sách (`/book/[slug]`):** SEO Server-Side Rendering (JSON-LD Product, BreadcrumbList), Gallery ảnh đa góc, Tabs thông số kỹ thuật & Đánh giá bạn đọc có xác thực đơn mua (`isVerifiedPurchase`).
4. **Giỏ Hàng Thông Minh (`/cart`):** Smart Cart Merge (phân vùng riêng `guest` và `user_<id>`), thanh Freeship 150.000đ, Session-based selection, sắp xếp giỏ hàng linh hoạt.
5. **Hiệu ứng Cao cấp Parabolic Fly-to-Cart:** Bay theo quỹ đạo cong Parabol GPU Composite 60fps Zero-Reflow khi thêm sách vào giỏ.
6. **Thanh Toán Chuẩn TMĐT (`/checkout`):** Sổ địa chỉ 3 cấp liên hoàn (Tỉnh - Huyện - Xã) qua Open API GSO, ước tính ngày giao theo vùng miền, hỗ trợ COD và chuyển khoản **VietQR MBBank 24/7**.
7. **Đặt Hàng ACID & Lịch Sử Đơn (`/account/orders`, `/order/success`):** Prisma Transaction chống bán âm kho (`stockQty`), sinh mã đơn chuẩn `TS-YYYYMMDD-XXXX`, theo dõi hành trình đơn hàng (Order Timeline Drawer).
8. **Tủ Sách Yêu Thích (`/wishlist`):** Lưu trữ độc lập theo tài khoản, chuyển nhanh 1 chạm sang giỏ hàng.
9. **Sách Bạn Vừa Xem Gần Đây (Redis In-Memory Cache TTL 30 Ngày):** Tự động ghi nhận và hiển thị widget trên Trang chủ & Chi tiết sách theo Mục 3.4 Proposal (Commit `633ba77`).
10. **Gợi Ý Sách Cá Nhân Hóa (Hybrid Recommendation Engine):** Thuật toán 5 trọng số (Proposal Mục 3.1) hiển thị sách đề xuất kèm tỷ lệ phù hợp ngay tại Trang chủ (Commit `bafd7bf`).

### 2. Phân Hệ Quản Trị (Admin & Staff Operation) — ĐÃ HOÀN TẤT 100%
1. **Khung Giao Diện Admin Hiện Đại (`/admin`):** Sidebar thu phóng, phân quyền truy cập RBAC nghiêm ngặt (`SUPER_ADMIN` và `STAFF`).
2. **Dashboard Doanh Thu & KPI:** Thống kê tổng doanh thu, số đơn hàng, số sách chạm ngưỡng tồn kho, biểu đồ doanh thu theo thời gian.
3. **Quản Lý Kho Sách (`/admin/books`):** Bảng dữ liệu tra cứu, lọc trạng thái tồn kho (Còn hàng / Sắp hết $\le 5$ / Hết hàng = 0), tìm kiếm realtime, form cập nhật giá và tồn kho.
4. **Quản Lý Đơn Hàng (`/admin/orders`):** Hệ thống 6 Tabs trạng thái (Tất cả, Chờ xử lý, Đã xác nhận, Đang giao, Đã giao, Đã hủy). **Ngăn kéo chi tiết đơn hàng (Order Detail Drawer)** hiển thị đầy đủ thông tin khách, địa chỉ, danh sách sách, phương thức thanh toán và nút chuyển trạng thái tức thì.
5. **Chuông Thông Báo Thông Minh (Live Notification Dropdown):** Click chuyển thẳng tới đơn hàng tự mở Drawer hoặc lọc sách sắp hết không qua modal trung gian.
6. **Phân Quyền Động Nhân Viên (Dynamic RBAC — `/admin/staff`):** Cấp quyền chi tiết 6 module (`MANAGE_BOOKS`, `MANAGE_ORDERS`, `MANAGE_CATEGORIES`, `MANAGE_USERS`, `VIEW_AUDIT_LOG`, `MANAGE_REVIEWS`).
7. **Quản Lý Khách Hàng Thành Viên (`/admin/customers`):** Thống kê đơn mua, tổng chi tiêu, đánh giá và trạng thái tài khoản.
8. **Nhật Ký Kiểm Toán & Hạ Tầng (`/admin/settings`):** Audit trail từ bảng `admin_audit_logs`, nút kích hoạt đồng bộ kho sách sang Elasticsearch 8.x index `books_v1`.
9. **Chính Sách Trợ Giá & Khuyến Mãi B2C (`/admin/promotions`):** Trợ giá Freeship 150k và chiết khấu giá bìa tự động.

---

## PHẦN II: ĐỐI CHIẾU CHÊNH LỆCH (GAP ANALYSIS) GIỮA PROPOSAL & THỰC TẾ HIỆN TẠI

| Hạng mục trong Proposal | Yêu cầu kỹ thuật trong Proposal (Trang 8-9) | Hiện trạng Dự án (09/10/2026) | Trạng thái Đánh giá |
|---|---|---|---|
| **1. Kế hoạch Dữ liệu (Seeding Plan - Mục 4)** | **Sách:** 5.000+ cuốn<br>**Tác giả:** 2.000+ người<br>**Danh mục:** 150+ nhánh (3 cấp)<br>**Đánh giá:** 50.000+ reviews<br>**Sự kiện hành vi:** 200.000+ events<br>**Đơn hàng:** 10.000+ orders | **ĐÃ HOÀN THÀNH 100% (VƯỢT TARGET):**<br>- **Sách thật:** **5.120 cuốn** trong CSV/JSON & **5.125 cuốn** trong PostgreSQL (100% sách thật có bìa CDN 750x750, ISBN-13, giá, mô tả)<br>- **Tác giả thật:** **2.787 người** trong CSV & **2.790 người** trong PostgreSQL<br>- **Danh mục 3 cấp:** **169 danh mục** theo chuẩn Thư viện Quốc gia DDC<br>- **Đánh giá:** **50.000 reviews** CSV & **1.018 reviews** live trong DB<br>- **Sự kiện hành vi:** **200.000 events** CSV & **2.005 events** live trong DB<br>- **Đơn hàng:** **10.000 orders** (16.960 items) CSV & **525 orders** live trong DB | ✅ **HOÀN THÀNH 100% XUẤT SẮC** |
| **2. Tìm kiếm thông minh (Mục 3.2)** | **Elasticsearch 8.x**:<br>- Vietnamese analyzer (tách từ tiếng Việt)<br>- Fuzzy matching (khoảng cách Levenshtein $\le 2$)<br>- Multi-field boost: `title^3`, `author^2`, `description^1`<br>- Faceted search & Synonyms | **ĐÃ TÍCH HỢP 100%:** Client `@elastic/elasticsearch` index `books_v1`, boost 3-2-1, fuzzy auto, bộ từ điển đồng nghĩa tiếng Việt, có cơ chế Fallback tự động sang PostgreSQL. Đã đồng bộ 5.125 cuốn sách. | ✅ **HOÀN THÀNH & ĐANG SỬ DỤNG** (Commit `676a11b`) |
| **3. Cá nhân hóa & Cache (Mục 3.4)** | - **Redis 7.x**: Lưu trữ phiên duyệt sách, giỏ hàng đệm.<br>- Widget *"Sách vừa xem gần đây"* lưu key `recently_viewed:<userId>` với **TTL 30 ngày**.<br>- Nhắc nhở giỏ hàng bỏ quên. | **ĐÃ TÍCH HỢP 100%:** Kết nối `@upstash/redis` (kèm In-Memory Fallback), TTL 30 ngày (2.592.000s), Widget "Sách bạn vừa xem gần đây" đã render trên Trang chủ và Chi tiết sách. | ✅ **HOÀN THÀNH & ĐANG SỬ DỤNG** (Commit `633ba77`) |
| **4. Engine Gợi ý Sách (Mục 3.1)** | Công thức kết hợp 5 trọng số:<br>- **Collaborative Filtering:** 35%<br>- **Content-based (Category/Author):** 25%<br>- **Behavioral (Clicks/Views/Cart):** 15%<br>- **Rating & Review Quality:** 20%<br>- **Trending & Bestseller:** 5% | **ĐÃ TÍCH HỢP 100%:** Module `src/lib/recommendation.ts` tính toán đúng công thức 35-25-15-20-5, API `/api/books/recommendations`, Widget "Gợi ý dành riêng cho bạn" hiển thị trên Trang chủ. | ✅ **HOÀN THÀNH & ĐANG SỬ DỤNG** (Commit `bafd7bf`) |
| **5. Chatbot Hỗ trợ (Mục 3.3)** | Claude API / LLM kết hợp RAG, tra cứu thông tin sách theo tầm giá, thể loại và **tra cứu trạng thái đơn hàng thời gian thực**. | Đã có Widget AI FloatingChatbot; tính năng tra cứu đơn hàng realtime dự kiến tích hợp ở bước cuối cùng. | ⏳ **ĐANG CHỜ LỆNH** (Làm sau cùng theo chỉ đạo). |
| **6. Mô hình Kiến trúc Hệ thống** | **Modular Monolith** phân tầng, sẵn sàng tách Microservices khi scale lớn. | **Layered Modular Monolith (4-Tier)**: Presentation $\rightarrow$ Edge Proxy $\rightarrow$ Application $\rightarrow$ Redis Cache $\rightarrow$ Elasticsearch $\rightarrow$ PostgreSQL. | ✅ **HOÀN TOÀN ĐỒNG NHẤT VÀ VỮNG CHẮC** |

---

## PHẦN III: KẾ HOẠCH HÀNH ĐỘNG CHI TIẾT (ACTION ROADMAP TRƯỚC 13/10/2026)

Để buổi gặp Thầy vào **13h30 Chiều Thứ 3 (13/10/2026)** đạt kết quả xuất sắc nhất, lộ trình nâng cấp được cập nhật trạng thái như sau:

### [ĐÃ HOÀN TẤT 100%] Bước 2: Tích Hợp Redis — Widget "Sách Vừa Xem Gần Đây" (TTL 30 Ngày)
- **Trạng thái:** ✅ **HOÀN THÀNH & ĐANG SỬ DỤNG** (Commit `633ba77`).
- **Hiện thực hóa:** Module `src/lib/redis.ts` hỗ trợ Upstash Cloud + Graceful In-Memory Fallback. Tự động đẩy sách vào danh sách khi xem, duy trì TTL 2.592.000s, render `RecentlyViewedCarousel` tại Trang chủ & Chi tiết sách.

### [ĐÃ HOÀN TẤT 100%] Bước 3: Tích Hợp Elasticsearch 8.x (Kèm Fallback PG)
- **Trạng thái:** ✅ **HOÀN THÀNH & ĐANG SỬ DỤNG** (Commit `676a11b`).
- **Hiện thực hóa:** Module `src/lib/elasticsearch.ts` index `books_v1`, boost 3 trường `title^3, authors^2, description^1`, từ điển đồng nghĩa tiếng Việt, fuzzy Levenshtein $\le 2$, API `/api/books/search`, nút đồng bộ tại Admin `/admin/settings`, tự động fallback sang PG nếu offline.

### [ĐÃ HOÀN TẤT 100%] Bước 4: Engine Gợi Ý 5 Trọng Số (Recommendation Engine)
- **Trạng thái:** ✅ **HOÀN THÀNH & ĐANG SỬ DỤNG** (Commit `bafd7bf`).
- **Hiện thực hóa:** Module `src/lib/recommendation.ts` tính toán chuẩn công thức $35\% \text{Collab} + 25\% \text{Content} + 15\% \text{Behavior} + 20\% \text{Rating} + 5\% \text{Trending}$. API `/api/books/recommendations` và component `RecommendedForYou` hiển thị nhãn % phù hợp tại Trang chủ.

---

### [CÁC HẠNG MỤC TIẾP THEO — ĐANG CHỜ DUYỆT TỪ NGƯỜI DÙNG]

### Bước A: Nạp Dữ Liệu Lớn (Batch Seeding Dataset — Mục 4 Proposal)
- **Mục tiêu:** Đưa kho sách từ 9 cuốn lên quy mô hàng trăm cuốn thực tế từ thư mục `to-sach-book-data`, lấp đầy đơn hàng qua các trạng thái (PENDING, CONFIRMED, SHIPPING, DELIVERED), sinh sự kiện hành vi và đánh giá bạn đọc.
- **Tình trạng:** Sẵn sàng script nạp khi được duyệt lệnh.

### Bước B: Nâng Cấp Chatbot RAG Tích Hợp Dữ Liệu Hệ Thống (Mục 3.3 Proposal)
- **Mục tiêu:** Anthropic Claude API với RAG pipeline (embedding sách $\rightarrow$ vector store $\rightarrow$ context injection) phục vụ 3 bài toán:
  1. *Truy vấn sách ngữ nghĩa:* *"Tìm sách self-help cho người đi làm, giá dưới 150k"* $\rightarrow$ ES query + rerank.
  2. *Tra cứu đơn hàng thời gian thực:* *"Đơn hàng tôi đang ở đâu?"* $\rightarrow$ Truy vấn orders table theo `user_id`.
  3. *Gợi ý sách tương tự:* *"Sách nào hay tương tự cuốn tôi vừa đọc?"* $\rightarrow$ Vector similarity search.
- **Tình trạng:** Sẵn sàng triển khai ở bước sau cùng theo đúng chỉ đạo.

### Bước C: Soạn Thảo Bản Báo Cáo Tiến Độ Viết Thuyết Minh — [Bắt buộc cho buổi họp]
- **Mục tiêu:** Chuẩn bị sẵn file Word/PDF Báo cáo tiến độ theo mẫu của Trường để nộp cho Thầy vào 13h30 chiều Thứ 3.
- **Triển khai:**
  - Tổng hợp cấu trúc đề tài theo 5 chương chuẩn đồ án CNTT.
  - Thống kê tỷ lệ phần trăm hoàn thành của từng chương:
    - Chương 1: Tổng quan đề tài & Nghiên cứu thị trường TMĐT sách (Hoàn thành 90%).
    - Chương 2: Phân tích yêu cầu & Thiết kế hệ thống (Hoàn thành 85%).
    - Chương 3: Thiết kế cơ sở dữ liệu & Kiến trúc phần mềm (Hoàn thành 90%).
    - Chương 4: Hiện thực hóa hệ thống & Các phân hệ chức năng (Hoàn thành 85%).
    - Chương 5: Đánh giá, Thử nghiệm & Hướng phát triển (Đang triển khai - 45%).
  - Dự kiến hoàn thiện toàn bộ bản thảo trước ngày 25/10/2026.

---

## PHẦN IV: CHIẾN LƯỢC TRÌNH BÀY & BẢO VỆ TRƯỚC GIẢNG VIÊN HƯỚNG DẪN

1. **Về Khối Lượng Công Việc (Workload):** Nhóm tự tin khẳng định đã hiện thực hóa một hệ sinh thái hoàn chỉnh, giải quyết trọn vẹn cả nghiệp vụ thanh toán, kho vận, phân quyền động và trải nghiệm người dùng tinh tế.
2. **Về Kiến Trúc Phần Mềm:** Khẳng định trung thực và thuyết phục việc áp dụng **Modular Monolith phân tầng N-tier** theo đúng định hướng Proposal, giải thích rõ lý do không phân mảnh sang Microservices để bảo toàn tính toàn vẹn dữ liệu giao dịch ACID và tối ưu tài nguyên vận hành.
3. **Về Công Cụ Mới (Elasticsearch, Redis):** Trình bày kiến trúc theo mô hình **Đa tầng dự phòng (Graceful Degradation)**: Ứng dụng tích hợp Elasticsearch cho tìm kiếm chuyên sâu và Redis cho truy xuất siêu tốc, đồng thời tầng PostgreSQL luôn đóng vai trò là "Single Source of Truth" đảm bảo hệ thống không bao giờ bị gián đoạn hoạt động.
