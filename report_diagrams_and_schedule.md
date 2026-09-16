# 📐 THIẾT KẾ KIẾN TRÚC HỆ THỐNG, CƠ SỞ DỮ LIỆU & KẾ HOẠCH BÁO CÁO ĐỒ ÁN
> **Đề tài:** Hệ thống Thương mại Điện tử Bán Sách Trực tuyến **Tổ Sách** (B2C)  
> **Khẩu hiệu:** *"Sách về tổ, tri thức bay xa"*  
> **Chuyên ngành:** Công nghệ Thông tin (Đồ án tốt nghiệp / Đồ án liên ngành)  
> **Tác giả:** Hanh-Dang (hnhngyndng@gmail.com)  
> **Thời gian thực hiện:** 07/09/2026 — 30/10/2026 (8 tuần)

---

> [!IMPORTANT]
> **Tài liệu này được biên soạn theo chuẩn học thuật chuyên ngành CNTT / Kỹ thuật Phần mềm.**  
> Bạn có thể trích xuất trực tiếp các sơ đồ (sử dụng [mermaid.live](https://mermaid.live) hoặc copy mã nguồn) để đưa vào **Quyển Báo cáo Thuyết minh Đồ án** và **Slide Thuyết trình Bảo vệ** trước Hội đồng chấm thi.

---

## 📑 MỤC LỤC
1. [PHẦN 1: Sơ Đồ Kiến Trúc Hệ Thống (System Architecture)](#-phần-1-sơ-đồ-kiến-trúc-hệ-thống)
   - [1.1 Sơ đồ Kiến trúc Phân tầng (N-Tier Architecture)](#11-sơ-đồ-kiến-trúc-phân-tầng-n-tier-architecture)
   - [1.2 Sơ đồ Triển khai Vật lý (Deployment & Cloud Infrastructure)](#12-sơ-đồ-triển-khai-vật-lý-deployment--cloud-infrastructure)
2. [PHẦN 2: Sơ Đồ Thiết Kế Cơ Sở Dữ Liệu (Database Design)](#-phần-2-sơ-đồ-thiết-kế-cơ-sở-dữ-liệu)
   - [2.1 Sơ đồ Thực thể Quan hệ (ERD - 14 Bảng Chuẩn 3NF)](#21-sơ-đồ-thực-thể-quan-hệ-erd---14-bảng-chuẩn-3nf)
   - [2.2 Từ điển Dữ liệu Chi tiết (Data Dictionary)](#22-từ-điển-dữ-liệu-chi-tiết-data-dictionary)
3. [PHẦN 3: Toàn Bộ Sơ Đồ Tiêu Chuẩn Cho Báo Cáo Đồ Án (UML 2.5)](#-phần-3-toàn-bộ-sơ-đồ-tiêu-chuẩn-cho-báo-cáo-đồ-án)
   - [3.1 Sơ đồ Ca Sử Dụng Tổng quan (System Use Case)](#31-sơ-đồ-ca-sử-dụng-tổng-quan-system-use-case)
   - [3.2 Phân rã Use Case theo Nhóm Người dùng](#32-phân-rã-use-case-theo-nhóm-người-dùng)
   - [3.3 Sơ đồ Hoạt động Nghiệp vụ (Activity Diagrams)](#33-sơ-đồ-hoạt-động-nghiệp-vụ-activity-diagrams)
   - [3.4 Sơ đồ Tuần tự Chi tiết (Sequence Diagrams)](#34-sơ-đồ-tuần-tự-chi-tiết-sequence-diagrams)
   - [3.5 Sơ đồ Chuyển Trạng thái (State Machine Diagrams)](#35-sơ-đồ-chuyển-trạng-thái-state-machine-diagrams)
   - [3.6 Sơ đồ Cấu trúc Thông tin & Điều hướng (Site Map / Information Architecture)](#36-sơ-đồ-cấu-trúc-thông-tin--điều-hướng-site-map)
4. [PHẦN 4: Kế Hoạch Báo Cáo Tiến Độ 8 Tuần (07/09 — 30/10/2026)](#-phần-4-kế-hoạch-báo-cáo-tiến-độ-8-tuần)
   - [4.1 Bảng 4 Cột Mốc Báo Cáo Chính (Milestones & Deliverables)](#41-bảng-4-cột-mốc-báo-cáo-chính)
   - [4.2 Bảng Chi tiết Công việc Từng Tuần (Tuần 1 đến Tuần 8)](#42-bảng-chi-tiết-công-việc-từng-tuần)
5. [PHẦN 5: Hướng Dẫn Trích Xuất Sơ Đồ Sang Word / Slide Chất Lượng Cao](#-phần-5-hướng-dẫn-trích-xuất-sơ-đồ)

---

# 🏛️ PHẦN 1: Sơ Đồ Kiến Trúc Hệ Thống

## 1.1 Sơ đồ Kiến trúc Phân tầng (N-Tier Architecture)

> [!NOTE]
> Hệ thống áp dụng mô hình **Modern Fullstack Serverless Architecture** với **Next.js 15 App Router** làm hạt nhân, tách biệt 4 phân tầng độc lập giúp dễ bảo trì, tăng tính bảo mật và đạt điểm tối đa khi bảo vệ trước Hội đồng.

```mermaid
graph TD
    subgraph PresentationTier["1. TẦNG TRÌNH DIỄN (Client / UI Layer)"]
        Browser["Trình duyệt Người dùng (Desktop / Mobile)"]
        UI_Customer["Giao diện Khách hàng (Trang chủ, Catalog, Giỏ hàng, Chi tiết)"]
        UI_Admin["Giao diện Quản trị (Dashboard, Quản lý kho, Phân quyền RBAC)"]
        Browser --> UI_Customer
        Browser --> UI_Admin
    end

    subgraph SecurityGateway["2. TẦNG BẢO MẬT & ĐIỀU HƯỚNG (Next.js Proxy / Edge Middleware)"]
        Middleware["Middleware / Proxy Chặn Lọc"]
        AuthCheck{"Kiểm tra Cookie JWT (HttpOnly)"}
        RBACCheck{"Kiểm tra Vai trò (USER / STAFF / SUPER_ADMIN)"}
        
        Middleware --> AuthCheck
        AuthCheck -->|Hợp lệ| RBACCheck
        AuthCheck -->|Chưa đăng nhập| RedirectLogin["Redirect /auth?redirect=..."]
        RBACCheck -->|Không đủ quyền| RedirectHome["Redirect / (Chặn 403 Forbidden)"]
    end

    subgraph ApplicationTier["3. TẦNG ỨNG DỤNG & NGHIỆP VỤ (Next.js 15 Server)"]
        RSC["React Server Components (RSC)<br/>(Render HTML trên Server, Tối ưu SEO, Query DB Trực tiếp)"]
        RouteHandlers["API Route Handlers (/api/auth, /api/orders, /api/chat)"]
        AuthEngine["Bộ Xác thực Độc lập (jose HS256 + bcryptjs)"]
        GeminiService["Module Trợ lý AI (Google GenAI SDK - Gemini Flash)"]
        
        RouteHandlers --> AuthEngine
        RouteHandlers --> GeminiService
    end

    subgraph DataTier["4. TẦNG DỮ LIỆU & DỊCH VỤ ĐÁM MÂY (Database & Services)"]
        Prisma["Prisma ORM 6 (Type-Safe Query & Transactions)"]
        PgPooler["Supabase Connection Pooler (PgBouncer - Cổng 6543)"]
        PgDirect["Supabase Direct PostgreSQL (Cổng 5432 - Dùng cho Migration)"]
        Storage["Cloud Storage (Lưu trữ ảnh bìa sách & Avatar)"]
        
        Prisma --> PgPooler
        Prisma --> PgDirect
    end

    PresentationTier -->|Gửi Request kèm HttpOnly Cookie| SecurityGateway
    RBACCheck -->|Hợp lệ| ApplicationTier
    ApplicationTier --> Prisma
    GeminiService -.->|Truy xuất danh mục sách| Prisma
```

---

## 1.2 Sơ đồ Triển khai Vật lý (Deployment & Cloud Infrastructure)

```mermaid
graph LR
    ClientDevice["Thiết bị Khách hàng / Admin"]
    
    subgraph CloudflareVercel["Hạ tầng Máy chủ Biên & Điện toán (Vercel Cloud)"]
        EdgeDNS["Vercel Global Edge Network / CDN"]
        NextServerless["Next.js Serverless Functions (Node.js 20.x Runtime)"]
    end

    subgraph SupabaseCloud["Hạ tầng Cơ sở Dữ liệu Đám Mây (Supabase Singapore)"]
        PgBouncer["PgBouncer Connection Pooler (Port 6543)"]
        PostgresInstance[("PostgreSQL 15.x Engine (14 Tables, RLS, Foreign Keys)")]
    end

    subgraph ExternalAPIs["Dịch vụ Đám Mây Bên Ngoài"]
        GoogleAI["Google AI Studio (Gemini 1.5 Flash API)"]
        VietQR["Dịch vụ VietQR (Tự động sinh mã QR chuyển khoản ngân hàng)"]
    end

    ClientDevice -->|HTTPS / TLS 1.3| EdgeDNS
    EdgeDNS --> NextServerless
    NextServerless -->|SSL Encrypted Query| PgBouncer
    PgBouncer --> PostgresInstance
    NextServerless -->|REST API Key| GoogleAI
    ClientDevice -.->|Quét mã thanh toán| VietQR
```

---

# 🗄️ PHẦN 2: Sơ Đồ Thiết Kế Cơ Sở Dữ Liệu

## 2.1 Sơ đồ Thực thể Quan hệ (ERD - 14 Bảng Chuẩn 3NF)

> [!TIP]
> Toàn bộ 14 thực thể được đồng bộ 100% với file [`prisma/schema.prisma`](file:///e:/to-sach-studio/prisma/schema.prisma) đang chạy trực tiếp trên Supabase PostgreSQL.

```mermaid
erDiagram
    USER ||--o| USER_PROFILE : "has (1-1)"
    USER ||--o{ ORDER : "places (1-N)"
    USER ||--o{ REVIEW : "writes (1-N)"
    USER ||--o{ STAFF_PERMISSION : "holds (1-N)"
    USER ||--o{ STAFF_PERMISSION : "assigns (1-N)"
    USER ||--o{ AUDIT_LOG : "executes (1-N)"
    USER ||--o{ BEHAVIOR_EVENT : "triggers (1-N)"

    PERMISSION ||--o{ STAFF_PERMISSION : "granted_in (1-N)"

    CATEGORY ||--o{ CATEGORY : "parent_children (1-N)"
    CATEGORY ||--o{ BOOK_CATEGORY : "contains (1-N)"

    AUTHOR ||--o{ BOOK_AUTHOR : "writes (1-N)"

    BOOK ||--o{ BOOK_AUTHOR : "has_author (1-N)"
    BOOK ||--o{ BOOK_CATEGORY : "has_category (1-N)"
    BOOK ||--o{ ORDER_ITEM : "ordered_in (1-N)"
    BOOK ||--o{ REVIEW : "receives (1-N)"
    BOOK ||--o{ BEHAVIOR_EVENT : "viewed_in (1-N)"

    ORDER ||--o{ ORDER_ITEM : "contains (1-N)"

    USER {
        string id PK
        string email UK
        string password_hash
        string full_name
        string phone UK
        enum role "USER | STAFF | SUPER_ADMIN"
        enum status "ACTIVE | BANNED | UNVERIFIED"
        datetime created_at
    }

    USER_PROFILE {
        string id PK
        string user_id FK
        string address_province
        string address_district
        string address_ward
        string address_detail
        string[] preferred_genres
        string reading_level
    }

    PERMISSION {
        string id PK
        string code UK
        string name
        string description
    }

    STAFF_PERMISSION {
        string id PK
        string staff_id FK
        string permission_id FK
        string assigned_by_id FK
        datetime assigned_at
    }

    CATEGORY {
        string id PK
        string parent_id FK
        string name
        string slug UK
        int level "1: Ngành hàng, 2: Chuyên mục, 3: Chi tiết"
        int sort_order
    }

    AUTHOR {
        string id PK
        string name
        string slug UK
        string bio
    }

    BOOK {
        string id PK
        string isbn UK
        string title
        string slug UK
        int price
        int original_price
        int stock_qty
        int sold_count
        float avg_rating
        int rating_count
        boolean is_active
        boolean is_bestseller
        boolean is_new
    }

    BOOK_AUTHOR {
        string book_id PK,FK
        string author_id PK,FK
    }

    BOOK_CATEGORY {
        string book_id PK,FK
        string category_id PK,FK
    }

    ORDER {
        string id PK
        string order_code UK
        string user_id FK
        string customer_name
        string customer_phone
        string shipping_address
        int subtotal
        int shipping_fee
        int total_amount
        enum payment_method "COD | BANK_TRANSFER"
        enum payment_status "PENDING | PAID | FAILED | REFUNDED"
        enum status "PENDING | CONFIRMED | SHIPPING | DELIVERED | CANCELLED"
        datetime created_at
    }

    ORDER_ITEM {
        string id PK
        string order_id FK
        string book_id FK
        string book_title
        int unit_price
        int quantity
        int total_price
    }

    REVIEW {
        string id PK
        string book_id FK
        string user_id FK
        int rating "1 - 5 Sao"
        string body
        boolean is_verified_purchase
        enum status "PENDING | APPROVED | REJECTED"
        datetime created_at
    }

    AUDIT_LOG {
        string id PK
        string user_id FK
        string action
        string target_type
        string target_id
        json details
        datetime created_at
    }

    BEHAVIOR_EVENT {
        string id PK
        string user_id FK
        string session_id
        string event_type "view | add_to_cart | search | purchase"
        string book_id FK
        string search_query
        datetime created_at
    }
```

---

## 2.2 Từ điển Dữ liệu Chi tiết (Data Dictionary)

| STT | Bảng trong Database | Model Prisma | Mục đích & Nghiệp vụ cốt lõi | Khóa chính (PK) / Khóa ngoại (FK) |
|:---:|---|---|---|---|
| **1** | `users` | `User` | Lưu trữ tài khoản người dùng, email, hash mật khẩu `bcryptjs`, vai trò (`USER`, `STAFF`, `SUPER_ADMIN`). | PK: `id`, UK: `email`, `phone` |
| **2** | `user_profiles` | `UserProfile` | Sổ địa chỉ mặc định, sở thích thể loại sách, cấp độ đọc sách. | PK: `id`, FK: `user_id` $\rightarrow$ `users(id)` |
| **3** | `permissions` | `Permission` | Danh mục quyền quản trị hệ thống (`MANAGE_BOOKS`, `MANAGE_ORDERS`, `MANAGE_USERS`, `MANAGE_CATEGORIES`,...). | PK: `id`, UK: `code` |
| **4** | `staff_permissions` | `StaffPermission` | Cấp phát quyền hạn động cho từng nhân viên, lưu người cấp quyền (`assigned_by_id`). | PK: `id`, FK: `staff_id`, `permission_id`, `assigned_by_id` |
| **5** | `categories` | `Category` | Cây phân cấp thể loại 3 cấp (L1 - L2 - L3) thông qua liên kết đệ quy `parent_id`. | PK: `id`, FK: `parent_id` $\rightarrow$ `categories(id)` |
| **6** | `authors` | `Author` | Danh bạ tác giả, tiểu sử, ảnh đại diện phục vụ tìm kiếm. | PK: `id`, UK: `slug` |
| **7** | `books` | `Book` | Đầu sách: giá bán, giá bìa gốc, số lượng tồn kho, số lượng đã bán, đánh giá sao trung bình, thông số kỹ thuật. | PK: `id`, UK: `slug`, `isbn` |
| **8** | `book_authors` | `BookAuthor` | Bảng quan hệ Nhiều-Nhiều (N-N) giữa Sách và Tác giả. | PK: (`book_id`, `author_id`) |
| **9** | `book_categories` | `BookCategory` | Bảng quan hệ Nhiều-Nhiều (N-N) giữa Sách và Danh mục thể loại. | PK: (`book_id`, `category_id`) |
| **10** | `orders` | `Order` | Đơn hàng với mã định danh `TS-YYYYMMDD-XXXX`, tổng tiền hàng, phí ship, địa chỉ giao nhận, trạng thái vận chuyển và thanh toán. | PK: `id`, UK: `order_code`, FK: `user_id` |
| **11** | `order_items` | `OrderItem` | Chi tiết sách trong đơn hàng, lưu snapshot giá bán tại thời điểm giao dịch. | PK: `id`, FK: `order_id`, `book_id` |
| **12** | `reviews` | `Review` | Đánh giá sao (1-5) và bình luận, cờ xác thực đã mua hàng, trạng thái kiểm duyệt (`APPROVED`, `PENDING`, `REJECTED`). | PK: `id`, FK: `book_id`, `user_id` |
| **13** | `admin_audit_logs` | `AuditLog` | Nhật ký truy vết kiểm toán: ghi lại toàn bộ thao tác thêm/sửa/xóa của Admin/Staff kèm IP và diff dữ liệu. | PK: `id`, FK: `user_id` |
| **14** | `user_behavior_events` | `BehaviorEvent` | Thu thập sự kiện người dùng (xem sách, tìm kiếm từ khóa, thêm giỏ) phục vụ phân tích. | PK: `id`, FK: `user_id`, `book_id` |

---

# 📊 PHẦN 3: Toàn Bộ Sơ Đồ Tiêu Chuẩn Cho Báo Cáo Đồ Án

## 3.1 Sơ đồ Ca Sử Dụng Tổng Quan (System Use Case)

```mermaid
graph LR
    UserGuest["Khách Vãng Lai"]
    UserCustomer["Khách Hàng Thành Viên"]
    UserStaff["Nhân Viên Vận Hành (Staff)"]
    UserAdmin["Quản Trị Viên (Super Admin)"]

    subgraph SystemBoundary["HỆ THỐNG THƯƠNG MẠI ĐIỆN TỬ TỔ SÁCH (B2C)"]
        UC_ViewBooks["Xem & Tìm kiếm sách"]
        UC_FilterCat["Duyệt danh mục 3 cấp"]
        UC_Cart["Quản lý Giỏ hàng"]
        UC_Auth["Đăng ký / Đăng nhập"]
        UC_Checkout["Đặt hàng & Thanh toán (COD / QR Bank)"]
        UC_OrderHistory["Theo dõi lịch sử đơn hàng"]
        UC_Review["Đánh giá & Bình luận sách"]
        UC_Profile["Cập nhật sổ địa chỉ & Hồ sơ"]
        UC_ChatAI["Trò chuyện cùng Trợ lý AI"]

        UC_ManageBooks["Quản lý Kho sách & Tồn kho"]
        UC_ManageOrders["Xử lý & Cập nhật đơn hàng"]
        UC_ModerateReviews["Kiểm duyệt bình luận"]
        UC_ManageCat["Quản lý Cây danh mục"]

        UC_ManageStaff["Quản lý & Cấp quyền Staff (RBAC)"]
        UC_AuditLogs["Xem Nhật ký kiểm toán (Audit Logs)"]
        UC_Dashboard["Xem Thống kê Doanh thu & Tồn kho"]
    end

    UserGuest --> UC_ViewBooks
    UserGuest --> UC_FilterCat
    UserGuest --> UC_Cart
    UserGuest --> UC_Auth
    UserGuest --> UC_Checkout
    UserGuest --> UC_ChatAI

    UserCustomer --> UC_ViewBooks
    UserCustomer --> UC_FilterCat
    UserCustomer --> UC_Cart
    UserCustomer --> UC_Checkout
    UserCustomer --> UC_OrderHistory
    UserCustomer --> UC_Review
    UserCustomer --> UC_Profile
    UserCustomer --> UC_ChatAI

    UserStaff --> UC_ManageBooks
    UserStaff --> UC_ManageOrders
    UserStaff --> UC_ModerateReviews
    UserStaff --> UC_ManageCat

    UserAdmin --> UC_ManageStaff
    UserAdmin --> UC_AuditLogs
    UserAdmin --> UC_Dashboard
    UserAdmin --> UC_ManageBooks
    UserAdmin --> UC_ManageOrders
```

---

## 3.2 Phân Rã Use Case Theo Nhóm Người Dùng

### A. Phân hệ Mua sắm & Khách hàng
```mermaid
graph TD
    ActorUser((Độc giả / Khách hàng))

    subgraph CustomerUseCases["Phân Hệ Trải Nghiệm Khách Hàng"]
        UC_Search["Tìm kiếm sách theo từ khóa"]
        UC_Filter["Lọc theo danh mục / giá / sao"]
        UC_ViewDetail["Xem chi tiết sách & Thông số NXB"]
        UC_AddToCart["Thêm vào giỏ hàng"]
        UC_UpdateCart["Tăng/giảm số lượng, xóa sách"]
        UC_CheckoutFlow["Thực hiện thanh toán"]
        UC_ChoosePayment["Chọn COD hoặc Chuyển khoản QR"]
        UC_TrackOrder["Tra cứu tình trạng vận chuyển"]
        UC_WriteReview["Viết đánh giá sau khi mua"]
    end

    ActorUser --> UC_Search
    ActorUser --> UC_Filter
    ActorUser --> UC_ViewDetail
    ActorUser --> UC_AddToCart
    ActorUser --> UC_UpdateCart
    ActorUser --> UC_CheckoutFlow
    ActorUser --> UC_TrackOrder
    ActorUser --> UC_WriteReview

    UC_CheckoutFlow -.->|<<include>>| UC_ChoosePayment
    UC_AddToCart -.->|<<extend>>| UC_ViewDetail
```

### B. Phân hệ Quản trị & Vận hành
```mermaid
graph TD
    ActorStaff((Nhân Viên Kho / Đơn))
    ActorAdmin((Super Admin))

    subgraph AdminUseCases["Phân Hệ Quản Trị & Vận Hành B2C"]
        UC_BookCRUD["Thêm / Sửa / Khóa sách & Cập nhật tồn kho"]
        UC_CategoryCRUD["Quản lý cấu trúc cây danh mục 3 tầng"]
        UC_OrderFulfillment["Xác nhận đơn, chuyển trạng thái giao hàng"]
        UC_ReviewModeration["Duyệt / Từ chối đánh giá của khách"]
        UC_StaffRBAC["Tạo tài khoản nhân viên & Gán quyền RBAC"]
        UC_AuditViewer["Tra cứu lịch sử thao tác hệ thống"]
        UC_RevenueReports["Báo cáo biểu đồ doanh thu và top bán chạy"]
    end

    ActorStaff --> UC_BookCRUD
    ActorStaff --> UC_OrderFulfillment
    ActorStaff --> UC_ReviewModeration
    ActorStaff --> UC_CategoryCRUD

    ActorAdmin --> UC_StaffRBAC
    ActorAdmin --> UC_AuditViewer
    ActorAdmin --> UC_RevenueReports
    ActorAdmin --> UC_BookCRUD
    ActorAdmin --> UC_OrderFulfillment
```

---

## 3.3 Sơ Đồ Hoạt Động Nghiệp Vụ (Activity Diagrams)

### Luồng 1: Quy trình Đặt hàng & Thanh toán (Checkout Flow)
```mermaid
stateDiagram-v2
    [*] --> DuyetGioHang: Khách hàng vào trang Giỏ hàng (/cart)
    DuyetGioHang --> KiemTraGio: Nhấn nút 'Tiến hành đặt hàng'
    
    state KiemTraGio <<choice>>
    KiemTraGio --> GioRong: Số lượng = 0
    GioRong --> DuyetGioHang: Báo lỗi & Quay lại xem sách
    KiemTraGio --> NhapThongTin: Giỏ hàng hợp lệ
    
    NhapThongTin --> ChonPhuongThuc: Điền Họ tên, SĐT, Địa chỉ nhận hàng
    
    state ChonPhuongThuc <<choice>>
    ChonPhuongThuc --> ThanhToanCOD: Chọn COD (Nhận hàng trả tiền)
    ChonPhuongThuc --> ThanhToanQR: Chọn Chuyển khoản VietQR
    
    ThanhToanQR --> SinhMaQR: Hệ thống sinh mã QR kèm cú pháp [Mã Đơn]
    SinhMaQR --> LuuDonHang: Chờ xác nhận chuyển khoản
    ThanhToanCOD --> LuuDonHang: Xác nhận đơn
    
    LuuDonHang --> TruTonKho: Mở Prisma Transaction: Tạo Order + OrderItems
    TruTonKho --> XoaGioHang: Trừ stockQty trong kho & Tăng soldCount
    XoaGioHang --> TrangThanhCong: Xóa LocalStorage giỏ hàng & Điều hướng /order/success
    TrangThanhCong --> [*]
```

### Luồng 2: Xác thực & Phân quyền RBAC (Middleware Gateway Flow)
```mermaid
stateDiagram-v2
    [*] --> NguoiDungGuiRequest: Gửi Request tới URL (Ví dụ: /admin/books)
    NguoiDungGuiRequest --> MiddlewareKiemTra: Next.js Proxy/Middleware đón chặn

    state KiemTraLoaiRoute <<choice>>
    MiddlewareKiemTra --> RouteCongKhai: Route công khai (/, /catalog, /book/*)
    MiddlewareKiemTra --> RouteYeuCauAuth: Route bảo vệ (/admin/* hoặc /account/*)

    RouteCongKhai --> ChoPhepTruyCap: Render trang bình thường

    state KiemTraToken <<choice>>
    RouteYeuCauAuth --> KiemTraToken: Đọc cookie 'tosach_token'
    KiemTraToken --> KhongCoToken: Token rỗng / Hết hạn
    KhongCoToken --> ChuyenHuongAuth: Redirect về /auth?redirect=URL

    KiemTraToken --> CoTokenHopLe: Giải mã JWT (jose HS256)
    
    state KiemTraVaiTro <<choice>>
    CoTokenHopLe --> KiemTraVaiTro: Kiểm tra payload.role
    KiemTraVaiTro --> RouteAdmin: Yêu cầu quyền Admin (/admin/*)
    KiemTraVaiTro --> RouteUser: Yêu cầu tài khoản cá nhân (/account/*)

    RouteUser --> ChoPhepTruyCap: role in [USER, STAFF, SUPER_ADMIN]
    
    state KiemTraQuyenStaff <<choice>>
    RouteAdmin --> KiemTraQuyenStaff: Kiểm tra role
    KiemTraQuyenStaff --> QuyenStaffAdmin: role là STAFF hoặc SUPER_ADMIN
    KiemTraQuyenStaff --> KhongCoQuyen: role là USER thông thường
    
    KhongCoQuyen --> ChuyenHuongTrangChu: Chặn truy cập (Redirect về /)
    QuyenStaffAdmin --> ChoPhepTruyCap: Cấp quyền truy cập Dashboard

    ChoPhepTruyCap --> [*]
    ChuyenHuongAuth --> [*]
    ChuyenHuongTrangChu --> [*]
```

---

## 3.4 Sơ Đồ Tuần Tự Chi Tiết (Sequence Diagrams)

### Luồng 1: Đăng nhập & Tạo Cookie Bảo Mật HttpOnly JWT
```mermaid
sequenceDiagram
    autonumber
    actor User as Khách hàng / Admin
    participant UI as Form Đăng Nhập (/auth)
    participant API as Auth API (/api/auth/login)
    participant DB as PostgreSQL (Supabase)
    participant Cookie as Trình duyệt (Cookie Jar)

    User->>UI: Nhập Email & Mật khẩu
    UI->>API: POST /api/auth/login { email, password }
    API->>DB: prisma.user.findUnique({ where: { email } })
    DB-->>API: Trả về bản ghi User (kèm passwordHash, role)
    
    alt Không tìm thấy Email hoặc Bị Banned
        API-->>UI: HTTP 401: "Email hoặc mật khẩu không chính xác"
        UI-->>User: Hiển thị thông báo lỗi
    else Email hợp lệ
        API->>API: bcrypt.compare(password, passwordHash)
        alt Mật khẩu sai
            API-->>UI: HTTP 401: "Email hoặc mật khẩu không chính xác"
            UI-->>User: Hiển thị thông báo lỗi
        else Mật khẩu chính xác
            API->>API: Tạo JWT Token (HS256: userId, role, fullName)
            API->>Cookie: Set-Cookie: tosach_token=JWT; HttpOnly; SameSite=Lax; Path=/
            API-->>UI: HTTP 200 { success: true, user: { id, email, role } }
            UI-->>User: Chuyển hướng về Trang chủ hoặc Trang Admin
        end
    end
```

---

### Luồng 2: Khách hàng Đặt hàng & Transaction Cơ Sở Dữ Liệu
```mermaid
sequenceDiagram
    autonumber
    actor Customer as Khách hàng
    participant CheckoutUI as Giao diện Thanh toán (/checkout)
    participant OrderAPI as Order API (/api/orders)
    participant Prisma as Prisma Client
    participant DB as PostgreSQL Database

    Customer->>CheckoutUI: Nhấn "Xác nhận đặt hàng"
    CheckoutUI->>OrderAPI: POST /api/orders { items, customerName, phone, address, paymentMethod }
    
    OrderAPI->>Prisma: prisma.$transaction(async (tx) => { ... })
    Note over Prisma,DB: Bắt đầu Database Transaction an toàn ACID
    
    loop Duyệt từng cuốn sách trong đơn
        Prisma->>DB: tx.book.findUnique({ id, select: { stockQty, price } })
        DB-->>Prisma: Trả về tồn kho thực tế
        alt Tồn kho < Số lượng mua
            Prisma-->>OrderAPI: Rollback & Ném lỗi "Sách X không đủ số lượng tồn"
            OrderAPI-->>CheckoutUI: HTTP 400: Thông báo sách hết hàng
        end
    end

    Prisma->>DB: tx.order.create({ data: { orderCode, customerName, totalAmount, ... } })
    Prisma->>DB: tx.orderItem.createMany({ data: orderItems })
    loop Trừ tồn kho & Tăng số lượng đã bán
        Prisma->>DB: tx.book.update({ where: { id }, data: { stockQty: decrement, soldCount: increment } })
    end
    
    Note over Prisma,DB: Commit Transaction thành công!
    DB-->>Prisma: Trả về bản ghi Order mới hoàn tất
    Prisma-->>OrderAPI: Hoàn thành Transaction
    OrderAPI-->>CheckoutUI: HTTP 201 { success: true, orderCode: "TS-20260916-001" }
    CheckoutUI->>Customer: Hiển thị trang hoàn tất đơn hàng & Chi tiết mã vận đơn
```

---

### Luồng 3: Trợ Lý AI Tư Vấn Sách (Gemini Flash RAG Flow)
```mermaid
sequenceDiagram
    autonumber
    actor Reader as Độc giả
    participant ChatWidget as Floating Chatbot Widget
    participant ChatRoute as Next.js API (/api/chat)
    participant DB as Supabase PostgreSQL
    participant Gemini as Google Gemini 1.5 Flash

    Reader->>ChatWidget: "Gợi ý cho tôi 2 cuốn sách phát triển bản thân dưới 150k"
    ChatWidget->>ChatRoute: POST /api/chat { prompt: "..." }
    
    ChatRoute->>DB: prisma.book.findMany({ where: { isActive: true }, take: 25 })
    DB-->>ChatRoute: Danh sách đầu sách thực tế (Tiêu đề, Giá, Thể loại, Tồn kho)
    
    Note over ChatRoute: RAG Context Assembly:<br/>Ghép Prompt người dùng + Kho dữ liệu sách thực tế
    
    ChatRoute->>Gemini: ai.models.generateContent({ systemInstruction, contents })
    Gemini-->>ChatRoute: Phân tích & Trả về câu trả lời tư vấn kèm liên kết sách
    ChatRoute-->>ChatWidget: HTTP 200 { reply: "..." }
    ChatWidget-->>Reader: Hiển thị tin nhắn mượt mà (kèm nút xem chi tiết cuốn sách)
```

---

## 3.5 Sơ Đồ Chuyển Trạng Thái (State Machine Diagrams)

### A. Vòng đời Trạng thái Đơn hàng (`OrderStatus`)
```mermaid
stateDiagram-v2
    [*] --> PENDING: Khách hoàn tất đặt hàng (Đơn mới tạo)
    
    PENDING --> CONFIRMED: Nhân viên kiểm tra thông tin & Duyệt đơn
    PENDING --> CANCELLED: Khách hủy đơn / Hết hàng
    
    CONFIRMED --> SHIPPING: Đóng gói xong, giao cho đơn vị vận chuyển
    CONFIRMED --> CANCELLED: Khách đổi ý hủy đơn trước khi xuất kho
    
    SHIPPING --> DELIVERED: Khách nhận hàng & Thanh toán thành công (COD)
    SHIPPING --> REFUNDED: Giao hàng thất bại / Khách từ chối nhận (Hoàn hàng)
    
    DELIVERED --> REFUNDED: Khiếu nại đổi trả được phê duyệt
    
    CANCELLED --> [*]
    DELIVERED --> [*]
    REFUNDED --> [*]
```

### B. Vòng đời Trạng thái Đánh giá Độc giả (`ReviewStatus`)
```mermaid
stateDiagram-v2
    [*] --> PENDING: Khách gửi đánh giá sau khi mua
    
    PENDING --> APPROVED: Nhân viên kiểm duyệt nội dung văn minh & hợp lệ
    PENDING --> REJECTED: Phát hiện từ ngữ phản cảm / Spam quảng cáo
    
    APPROVED --> REJECTED: Ẩn đánh giá nếu có tranh chấp phát sinh
    
    APPROVED --> [*]: Hiển thị công khai ở trang chi tiết sách
    REJECTED --> [*]: Không hiển thị ra bên ngoài
```

---

## 3.6 Sơ Đồ Cấu Trúc Thông Tin & Điều Hướng (Site Map)

```mermaid
graph TD
    Root["Trang Chủ Tổ Sách (/)"]

    subgraph PublicRoutes["PHÂN HỆ KHÁCH HÀNG (Public / Customer)"]
        Catalog["Danh Mục & Tìm Kiếm (/catalog)"]
        BookDetail["Chi Tiết Sách (/book/[slug])"]
        Cart["Giỏ Hàng (/cart)"]
        Checkout["Thanh Toán (/checkout)"]
        OrderSuccess["Đặt Hàng Thành Công (/order/success)"]
        Auth["Đăng Nhập / Đăng Ký (/auth)"]
        
        Profile["Hồ Sơ Cá Nhân (/account/profile)"]
        Orders["Lịch Sử Đơn Mua (/account/orders)"]
        OrderDetail["Chi Tiết Đơn Hàng (/account/orders/[id])"]
    end

    subgraph AdminRoutes["PHÂN HỆ QUẢN TRỊ (Chỉ STAFF & SUPER_ADMIN)"]
        AdminRoot["Dashboard Tổng Quan (/admin)"]
        AdminBooks["Quản Lý Kho Sách (/admin/books)"]
        AdminNewBook["Thêm Sách Mới (/admin/books/new)"]
        AdminCategories["Quản Lý Cây Danh Mục (/admin/categories)"]
        AdminOrders["Quản Lý Đơn Hàng (/admin/orders)"]
        AdminReviews["Kiểm Duyệt Đánh Giá (/admin/reviews)"]
        AdminStaff["Quản Lý & Phân Quyền Staff (/admin/staff)"]
        AdminAudit["Nhật Ký Kiểm Toán (/admin/audit-logs)"]
    end

    Root --> Catalog
    Root --> BookDetail
    Root --> Cart
    Root --> Auth
    
    Cart --> Checkout
    Checkout --> OrderSuccess
    
    Auth --> Profile
    Profile --> Orders
    Orders --> OrderDetail

    Root -.->|Kiểm tra quyền Staff/Admin| AdminRoot
    AdminRoot --> AdminBooks
    AdminBooks --> AdminNewBook
    AdminRoot --> AdminCategories
    AdminRoot --> AdminOrders
    AdminRoot --> AdminReviews
    AdminRoot --> AdminStaff
    AdminRoot --> AdminAudit
```

---

# 📅 PHẦN 4: Kế Hoạch Báo Cáo Tiến Độ 8 Tuần

> **Khung thời gian:** Từ **07/09/2026** đến **30/10/2026** (~8 tuần).  
> **Tần suất báo cáo:** Định kỳ **2 tuần một lần** (4 cột mốc lớn nộp cho Giảng viên hướng dẫn).

---

## 4.1 Bảng 4 Cột Mốc Báo Cáo Chính

| Đợt Báo Cáo | Thời điểm | Tên Giai Đoạn | Mục tiêu cốt lõi cần đạt | Sản phẩm bàn giao (Deliverables) |
|:---:|:---:|---|---|---|
| **ĐỢT 1** | **20/09/2026**<br/>*(Cuối Tuần 2)* | **Khởi Tạo Nền Tảng, Thiết Kế UI & Kiến Trúc Dữ Liệu** | • Chốt mô hình B2C chuẩn Proposal.<br/>• Thống nhất toàn bộ thiết kế giao diện web (Design System, Wireframes & Mockups các trang chính).<br/>• Dựng Next.js 15, Prisma ORM 14 bảng, kết nối Supabase Cloud.<br/>• Xây dựng Custom JWT Auth & RBAC Middleware.<br/>• Nạp Seed Data mẫu và dựng Shared Layout (Header/Footer). | 1. Source code nhánh `main` trên GitHub.<br/>2. Database 14 bảng hoạt động trên Supabase.<br/>3. Bản quy chuẩn thiết kế UI/UX & Design System.<br/>4. Tài liệu thiết kế hệ thống đầy đủ.<br/>5. Demo test thành công các API Auth. |
| **ĐỢT 2** | **04/10/2026**<br/>*(Cuối Tuần 4)* | **Trải Nghiệm Khách Hàng Toàn Diện (Storefront)** | • Hoàn thiện Trang chủ (Hero, Bestseller, Danh mục).<br/>• Hoàn thiện Trang Catalog bộ lọc 3 cấp & Tìm kiếm sách.<br/>• Trang Chi tiết sách & Form Đánh giá.<br/>• Hoàn thiện Giỏ hàng & Luồng Thanh toán (COD / Chuyển khoản QR) lưu đơn thật vào DB. | 1. Luồng mua hàng hoạt động từ A-Z trên web.<br/>2. Video demo quy trình đặt hàng và trừ tồn kho tự động.<br/>3. Đơn hàng hiển thị chuẩn trong bảng `orders` trên Supabase. |
| **ĐỢT 3** | **18/10/2026**<br/>*(Cuối Tuần 6)* | **Phân Hệ Quản Trị & Vận Hành (Admin & RBAC)** | • Xây dựng Dashboard thống kê doanh thu/tồn kho.<br/>• Module Quản lý kho sách (CRUD sách, upload ảnh bìa).<br/>• Module Quản lý danh mục 3 tầng & Xử lý đơn hàng.<br/>• Module Kiểm duyệt đánh giá & Phân quyền nhân viên động.<br/>• Nhật ký truy vết Audit Logs bảo mật. | 1. Toàn bộ route `/admin/*` hoạt động chuẩn với từng vai trò.<br/>2. Demo tài khoản Staff Kho không vào được trang Quản lý đơn, Staff Đơn không sửa được sách.<br/>3. Báo cáo kiểm thử bảo mật phân quyền. |
| **ĐỢT 4** | **30/10/2026**<br/>*(Nghiệm Thu)* | **Kiểm Thử Toàn Diện, Triển Khai & Hoàn Tất Đồ Án** | • Tích hợp Trợ lý AI Gemini Flash tư vấn sách.<br/>• Tối ưu Core Web Vitals, Responsive 100% Mobile/Tablet.<br/>• Triển khai Production lên Vercel Cloud (Live URL).<br/>• Hoàn tất Quyển Báo cáo Thuyết minh đồ án và Slide bảo vệ. | 1. Đường link trang web chính thức (Live Production URL).<br/>2. Quyển thuyết minh đồ án hoàn chỉnh (PDF/Word).<br/>3. Slide thuyết trình bảo vệ trước Hội đồng.<br/>4. Biên bản nghiệm thu sản phẩm phần mềm. |

---

## 4.2 Bảng Chi Tiết Công Việc Từng Tuần (Tuần 1 đến Tuần 8)

### Tuần 1: Khởi động dự án, Thiết kế UI thống nhất & Đặc tả kiến trúc (07/09 — 13/09/2026)
* **Mục tiêu:** Định vị mô hình B2C, thiết kế thống nhất toàn bộ giao diện web (Design System, Wireframes & Mockups), loại bỏ các thành phần râu ria của prototype cũ, thiết kế ERD.
* **Đầu việc cụ thể:**
  - [x] **Thiết kế Thống nhất Giao diện Web (Design System & UI Mockups):**
    + Xây dựng bộ quy chuẩn nhận diện thương hiệu số (Design Tokens): Bảng màu chủ đạo (Xanh hải quân sâu `#0B1F3A`, Vàng nghệ `#F5A623`, Nền kem ấm `#FAF7F2`, Xám đá Slate), Typography chuẩn hóa (Geist Sans / Inter), lưới layout 12 cột, hệ thống bo góc và bóng đổ (Elevation).
    + Thiết kế Wireframes & Mockup trực quan toàn bộ các màn hình cốt lõi theo phong cách B2C sang trọng, tinh tế: Trang chủ, Danh mục Catalog 3 tầng, Chi tiết sách, Giỏ hàng, Thanh toán, Theo dõi đơn hàng và Admin Dashboard.
    + Thống nhất bộ thư viện UI Components tái sử dụng: Buttons, Input fields, Dropdowns, BookCard, Badges trạng thái giao hàng, Modal thông báo, Breadcrumbs phân cấp.
  - [x] **Rà soát & Định hình Nghiệp vụ:** Rà soát Proposal đồ án liên ngành, chốt mô hình B2C (loại bỏ dứt điểm các thành phần C2C bán sách cũ, banner tuyển seller, yêu cầu tìm sách hiếm và ô voucher).
  - [x] **Lựa chọn Tech Stack:** Next.js 15 (App Router), TypeScript, Tailwind CSS v4, Prisma ORM 6, PostgreSQL Supabase.
  - [x] **Thiết kế Kiến trúc Dữ liệu ban đầu:** Thiết kế sơ bộ bản vẽ ERD 14 bảng dữ liệu và sơ đồ Use Case tổng quan.
  - [x] **Khởi tạo Quản lý Mã nguồn:** Thiết lập Git repository cục bộ và remote GitHub an toàn, cấu hình `.gitignore` chuẩn bảo mật.

### Tuần 2: Hiện thực hóa nền tảng, Database & Auth (14/09 — 20/09/2026) — *[ĐANG DIỄN RA]*
* **Mục tiêu:** Đưa Database lên Cloud, hoàn thành hệ thống Auth và dựng Shared Layout.
* **Đầu việc cụ thể:**
  - [x] Tạo dự án Next.js 15 tại thư mục gốc; lưu trữ code cũ vào [`legacy/`](file:///e:/to-sach-studio/legacy).
  - [x] Viết [`prisma/schema.prisma`](file:///e:/to-sach-studio/prisma/schema.prisma) 14 models; cấu hình Pooler và Direct URL.
  - [x] Đẩy bảng lên Supabase (`prisma db push`); viết và nạp `prisma/seed.ts` (4 users, 8 sách, 15 danh mục).
  - [x] Viết bộ xác thực Custom JWT (`src/lib/auth.ts`) và RBAC Proxy (`src/middleware.ts`).
  - [ ] Di chuyển Header và Footer B2C từ `legacy/` sang Next.js; tạo `CartContext` LocalStorage.
  - [ ] Dựng giao diện sơ bộ Trang chủ (`src/app/page.tsx`) kết nối Prisma query sách thật.
  - 🎯 **Nhiệm vụ báo cáo:** Nộp báo cáo Đợt 1 cho Giảng viên hướng dẫn (Kiến trúc, Database, UI Design System và Auth).

### Tuần 3: Danh Mục Sách & Trang Chi Tiết (21/09 — 27/09/2026)
* **Mục tiêu:** Xây dựng xong trang duyệt sách theo danh mục và trang xem chi tiết sách.
* **Đầu việc cụ thể:**
  - [ ] Xây dựng trang `/catalog`: Bộ lọc cây danh mục 3 tầng (L1 - L2 - L3), lọc khoảng giá, lọc sao đánh giá.
  - [ ] Tích hợp tính năng sắp xếp: Giá tăng dần, Giá giảm dần, Mới nhất, Bán chạy nhất.
  - [ ] Xây dựng trang `/book/[slug]`: Render thông tin sách, thư viện ảnh bìa, thông số xuất bản (NXB, số trang, kích thước, định dạng).
  - [ ] Hiển thị danh sách đánh giá đã duyệt từ bảng `reviews` và tính toán sao trung bình.
  - [ ] Xử lý nút "Thêm vào giỏ hàng" và "Mua ngay".

### Tuần 4: Giỏ Hàng & Quy Trình Thanh Toán Hoàn Chỉnh (28/09 — 04/10/2026)
* **Mục tiêu:** Khách hàng có thể đặt hàng thành công và hệ thống lưu đơn vào Database thật.
* **Đầu việc cụ thể:**
  - [ ] Xây dựng trang Giỏ hàng `/cart`: Tăng/giảm số lượng, kiểm tra giới hạn tồn kho, xóa sản phẩm, tính tổng tiền.
  - [ ] Xây dựng trang Thanh toán `/checkout`: Form nhập thông tin người nhận (Họ tên, SĐT, Tỉnh/Huyện/Xã/Địa chỉ chi tiết).
  - [ ] Xử lý 2 phương thức thanh toán: COD và Chuyển khoản QR ngân hàng (tự động điền mã đơn).
  - [ ] Viết API `/api/orders` dùng Prisma Transaction: tạo `orders`, tạo `order_items`, tự động trừ `stockQty` và tăng `soldCount`.
  - [ ] Xây dựng trang `/account/orders` hiển thị lịch sử đơn mua của người dùng kèm timeline trạng thái.
  - 🎯 **Nhiệm vụ báo cáo:** Nộp báo cáo Đợt 2 cho Giảng viên hướng dẫn (Toàn bộ luồng Khách hàng hoàn tất).

### Tuần 5: Phân Hệ Admin — Kho Sách & Danh Mục (05/10 — 11/10/2026)
* **Mục tiêu:** Xây dựng các chức năng quản lý danh mục và kho sách cho nhân viên kho.
* **Đầu việc cụ thể:**
  - [ ] Xây dựng layout trang Admin (`src/app/admin/layout.tsx`) với thanh điều hướng phân quyền.
  - [ ] Xây dựng Dashboard (`/admin`): Thẻ thống kê nhanh (Tổng doanh thu, Tổng đơn hàng, Tổng đầu sách, Đơn chờ xử lý).
  - [ ] Xây dựng Quản lý Kho sách (`/admin/books`): Bảng danh sách sách, tìm kiếm, lọc trạng thái kho (Còn hàng / Sắp hết / Hết hàng).
  - [ ] Xây dựng Form Thêm / Sửa sách (`/admin/books/new`, `/admin/books/[id]`): Nhập giá, tải ảnh bìa, chọn tác giả, chọn danh mục.
  - [ ] Xây dựng Quản lý Danh mục (`/admin/categories`): Thêm / sửa / xóa các cấp danh mục trong cây phân cấp.

### Tuần 6: Phân Hệ Admin — Đơn Hàng, Đánh Giá & Phân Quyền RBAC (12/10 — 18/10/2026)
* **Mục tiêu:** Hoàn thiện toàn bộ các chức năng quản trị còn lại của hệ thống.
* **Đầu việc cụ thể:**
  - [ ] Xây dựng Quản lý Đơn hàng (`/admin/orders`): Lọc theo trạng thái đơn (`PENDING`, `CONFIRMED`, `SHIPPING`, `DELIVERED`, `CANCELLED`).
  - [ ] Xây dựng chức năng cập nhật trạng thái đơn và mã vận đơn tracking code.
  - [ ] Xây dựng Kiểm duyệt Đánh giá (`/admin/reviews`): Duyệt (`APPROVED`) hoặc Từ chối (`REJECTED`) bình luận độc giả.
  - [ ] Xây dựng Phân quyền Nhân viên (`/admin/staff`): Chỉ `SUPER_ADMIN` mới được tạo tài khoản Staff và tích chọn cấp quyền.
  - [ ] Xây dựng Trang xem Nhật ký kiểm toán (`/admin/audit-logs`): Xem lại các thao tác nhạy cảm.
  - 🎯 **Nhiệm vụ báo cáo:** Nộp báo cáo Đợt 3 cho Giảng viên hướng dẫn (Phân hệ Admin & RBAC hoàn tất).

### Tuần 7: Tích Hợp Nâng Cao, Bảo Mật & Tối Ưu Hệ Thống (19/10 — 25/10/2026)
* **Mục tiêu:** Nâng cao chất lượng kỹ thuật, thêm tính năng AI và tối ưu hiệu năng.
* **Đầu việc cụ thể:**
  - [ ] Tích hợp Floating AI Chatbot (Gemini 1.5 Flash) qua API `/api/chat` đóng vai trò trợ lý tư vấn sách Tổ Sách.
  - [ ] Tối ưu hóa SEO: Thêm thẻ OpenGraph, Meta description, Schema markup `Product` và `BreadcrumbList` cho từng đầu sách.
  - [ ] Kiểm tra và gia cố bảo mật: Rate limiting chống spam API, validate dữ liệu đầu vào bằng `zod`.
  - [ ] Kiểm thử tự động: Viết kịch bản kiểm thử API (Auth, Order Creation) và kiểm tra tương thích trên màn hình điện thoại (Responsive Mobile).

### Tuần 8: Đóng Gói Triển Khai, Viết Báo Cáo & Chuẩn Bị Bảo Vệ (26/10 — 30/10/2026)
* **Mục tiêu:** Đưa hệ thống lên Production thực tế, hoàn tất hồ sơ nghiệm thu và slide bảo vệ.
* **Đầu việc cụ thể:**
  - [ ] Triển khai ứng dụng chính thức lên Vercel Cloud kết nối Supabase Cloud Production.
  - [ ] Kiểm tra toàn diện lần cuối trên đường link thật (UAT - User Acceptance Testing).
  - [ ] Soạn thảo và hoàn thiện Quyển Thuyết minh Báo cáo Đồ án (chèn toàn bộ sơ đồ ở Phần 1, 2, 3 vào báo cáo).
  - [ ] Thiết kế Slide thuyết trình bảo vệ đồ án (khoảng 20 - 25 slides tinh gọn, trực quan, làm nổi bật kiến trúc kỹ thuật).
  - [ ] Chuẩn bị kịch bản demo trực tiếp trước Hội đồng chấm thi.
  - 🎯 **Nhiệm vụ báo cáo:** Nghiệm thu toàn diện đề tài ngày 30/10/2026.

---

# 🖨️ PHẦN 5: Hướng Dẫn Trích Xuất Sơ Đồ

Để đưa các sơ đồ trong tài liệu này vào file báo cáo Word (`.docx`), PowerPoint hoặc LaTeX mà không bị vỡ nét hay nhòe chữ:

1. **Cách 1: Mermaid Live Editor (Khuyên dùng - Ảnh sắc nét nhất)**
   - Truy cập [mermaid.live](https://mermaid.live).
   - Sao chép toàn bộ nội dung trong khối mã ````mermaid ... ```` của sơ đồ cần dùng.
   - Dán vào khung Code bên trái.
   - Nhấn **Actions** $\rightarrow$ **Download PNG** (hoặc **Download SVG** - dạng vector vô cực) để chèn vào Word.
2. **Cách 2: Chụp trực tiếp từ Markdown Preview Enhanced trên IDE**
   - Bấm chuột phải vào sơ đồ được hiển thị trong Artifact $\rightarrow$ Chọn **Copy Image** hoặc **Save Image**.
