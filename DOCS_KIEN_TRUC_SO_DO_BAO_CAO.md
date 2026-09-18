# 📐 THIẾT KẾ KIẾN TRÚC HỆ THỐNG, CƠ SỞ DỮ LIỆU & KẾ HOẠCH BÁO CÁO ĐỒ ÁN
> **Đề tài:** Hệ sinh thái Thương mại Điện tử Bán Sách Trực tuyến **Tổ Sách** (B2C)  
> **Khẩu hiệu:** *"Sách về tổ, tri thức bay xa"*  
> **Chuyên ngành:** Công nghệ Thông tin (Đồ án tốt nghiệp / Đồ án liên ngành)  
> **Tác giả:** Hanh-Dang (hnhngyndng@gmail.com)  
> **Thời gian thực hiện:** 07/09/2026 — 30/10/2026 (8 tuần)

---

## MỤC LỤC TÀI LIỆU
1. [PHẦN 1: Sơ Đồ Kiến Trúc Hệ Thống (System Architecture)](#phần-1-sơ-đồ-kiến-trúc-hệ-thống-system-architecture)
   - 1.1 Sơ đồ Kiến trúc Phân tầng (N-Tier Architecture)
   - 1.2 Sơ đồ Triển khai Vật lý (Deployment & Cloud Infrastructure)
2. [PHẦN 2: Sơ Đồ Thiết Kế Cơ Sở Dữ Liệu (Database Design)](#phần-2-sơ-đồ-thiết-kế-cơ-sở-dữ-liệu-database-design)
   - 2.1 Sơ đồ Thực thể Quan hệ (Entity-Relationship Diagram - ERD)
   - 2.2 Từ điển Dữ liệu (Data Dictionary - 14 Tables)
3. [PHẦN 3: Toàn Bộ Sơ Đồ Tiêu Chuẩn Cho Báo Cáo Đồ Án (UML Standard Diagrams)](#phần-3-toàn-bộ-sơ-đồ-tiêu-chuẩn-cho-báo-cáo-đồ-án-uml-standard-diagrams)
   - 3.1 Sơ đồ Ca Sử Dụng Tổng quan (System Use Case Diagram)
   - 3.2 Phân rã Use Case theo Phân hệ (Khách hàng, Nhân viên, Quản trị viên)
   - 3.3 Sơ đồ Hoạt động Nghiệp vụ (Activity Diagrams: Mua hàng, Xác thực & RBAC, Vận hành đơn)
   - 3.4 Sơ đồ Tuần tự Chi tiết (Sequence Diagrams: Đăng nhập JWT, Đặt hàng Transaction, AI Chatbot)
   - 3.5 Sơ đồ Chuyển Trạng thái (State Machine Diagrams: Đơn hàng, Đánh giá)
   - 3.6 Sơ đồ Cấu trúc Thông tin & Điều hướng (Site Map / Information Architecture)
4. [PHẦN 4: Kế Hoạch Báo Cáo Tiến Độ 8 Tuần (Weekly & Bi-Weekly Progress Plan)](#phần-4-kế-hoạch-báo-cáo-tiến-độ-8-tuần-weekly--bi-weekly-progress-plan)
   - 4.1 Lộ trình 4 Chặng Báo cáo (Milestones & Deliverables)
   - 4.2 Bảng Chi tiết Công việc từng Tuần (Tuần 1 đến Tuần 8)
5. [PHẦN 5: Hướng Dẫn Trích Xuất Sơ Đồ Sang Word / PDF / LaTeX](#phần-5-hướng-dẫn-trích-xuất-sơ-đồ-sang-word--pdf--latex)

---

# PHẦN 1: Sơ Đồ Kiến Trúc Hệ Thống (System Architecture)

## 1.1 Sơ đồ Kiến trúc Phân tầng (N-Tier Architecture)
Hệ thống được xây dựng theo mô hình **Hiện đại (Modern Fullstack Serverless Architecture)** với Next.js 15 App Router làm trung tâm, chia làm 4 tầng độc lập:

```mermaid
graph TD
    subgraph PresentationTier["1. TẦNG TRÌNH DIỄN (Client / UI Layer)"]
        Browser["Trình duyệt Người dùng (Desktop / Mobile)"]
        UI_Customer["Giao diện Khách hàng (Catalog, Giỏ hàng, Chi tiết sách)"]
        UI_Admin["Giao diện Quản trị (Dashboard, Quản lý kho, Phân quyền)"]
        Browser --> UI_Customer
        Browser --> UI_Admin
    end

    subgraph SecurityGateway["2. TẦNG CỔNG BẢO MẬT & ĐIỀU HƯỚNG (Edge / Middleware)"]
        Middleware["Next.js Proxy / Middleware"]
        AuthCheck{"Kiểm tra Cookie JWT (HttpOnly)"}
        RBACCheck{"Kiểm tra Vai trò (USER / STAFF / SUPER_ADMIN)"}
        
        Middleware --> AuthCheck
        AuthCheck -->|Hợp lệ| RBACCheck
        AuthCheck -->|Chưa đăng nhập| RedirectLogin["Redirect /auth"]
        RBACCheck -->|Không đủ quyền| RedirectHome["Redirect / (Chặn 403)"]
    end

    subgraph ApplicationTier["3. TẦNG ỨNG DỤNG & NGHIỆP VỤ (Next.js 15 Server)"]
        RSC["React Server Components (RSC)<br/>(Render HTML tĩnh, tối ưu SEO, Query trực tiếp DB)"]
        RouteHandlers["API Route Handlers (/api/auth, /api/orders, /api/chat)"]
        ServerActions["Server Actions (Mutation, Form handling)"]
        AuthModule["Custom JWT Engine (jose + bcryptjs)"]
        GeminiService["Trợ lý AI Gemini Flash (Google GenAI SDK)"]
        
        RouteHandlers --> AuthModule
        RouteHandlers --> GeminiService
    end

    subgraph DataTier["4. TẦNG DỮ LIỆU & DỊCH VỤ NGOÀI (Database & Services)"]
        Prisma["Prisma ORM 6 (Type-Safe Query Builder)"]
        PgPooler["Supabase Connection Pooler (PgBouncer - Cổng 6543)"]
        PgDirect["Supabase Direct PostgreSQL (Session - Cổng 5432)"]
        Storage["Cloud Storage (Lưu trữ ảnh bìa sách / Avatars)"]
        
        Prisma --> PgPooler
        Prisma --> PgDirect
    end

    PresentationTier -->|Gửi Request kèm HttpOnly Cookie| SecurityGateway
    RBACCheck -->|Hợp lệ| ApplicationTier
    ApplicationTier --> Prisma
    GeminiService -.->|Truy vấn context| Prisma
```

---

## 1.2 Sơ đồ Triển khai Vật lý (Deployment & Cloud Infrastructure)
Mô tả cơ sở hạ tầng thực tế khi hệ thống vận hành trên môi trường đám mây:

```mermaid
graph LR
    ClientDevice["Thiết bị Khách hàng / Admin"]
    
    subgraph CloudflareVercel["Hạ tầng Edge & Compute (Vercel Cloud)"]
        EdgeDNS["Vercel Global Edge Network / DNS"]
        NextServerless["Next.js Serverless Functions (Node.js 20.x Runtime)"]
    end

    subgraph SupabaseCloud["Hạ tầng Cơ sở Dữ liệu (Supabase Singapore)"]
        PgBouncer["PgBouncer Connection Pooler (Port 6543)"]
        PostgresInstance[("PostgreSQL 15.x Engine (14 Tables, RLS, Foreign Keys)")]
    end

    subgraph ExternalAPIs["Dịch vụ Đám Mây Mở Rộng"]
        GoogleAI["Google AI Studio (Gemini 1.5 Flash API)"]
        VietQR["Cổng sinh mã VietQR (Thanh toán chuyển khoản tự động)"]
    end

    ClientDevice -->|HTTPS / TLS 1.3| EdgeDNS
    EdgeDNS --> NextServerless
    NextServerless -->|SSL Encrypted Query| PgBouncer
    PgBouncer --> PostgresInstance
    NextServerless -->|REST API Key| GoogleAI
    ClientDevice -.->|Quét mã thanh toán| VietQR
```

---

# PHẦN 2: Sơ Đồ Thiết Kế Cơ Sở Dữ Liệu (Database Design)

## 2.1 Sơ đồ Thực thể Quan hệ (Entity-Relationship Diagram - ERD)
Toàn bộ 14 bảng quan hệ được thiết kế chuẩn dạng chuẩn 3 (3NF), tuân thủ 100% `prisma/schema.prisma`:

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

## 2.2 Từ điển Dữ liệu (Data Dictionary - 14 Tables)

| STT | Tên Bảng (Table Name) | Tên Model Prisma | Chức năng nghiệp vụ chính | Khóa chính / Khóa ngoại liên kết |
|:---:|---|---|---|---|
| 1 | `users` | `User` | Lưu trữ tài khoản người dùng, email, hash mật khẩu, phân quyền 3 vai trò (`USER`, `STAFF`, `SUPER_ADMIN`). | PK: `id` |
| 2 | `user_profiles` | `UserProfile` | Thông tin hồ sơ mở rộng, sổ địa chỉ nhận hàng, sở thích thể loại, cấp độ đọc sách. | PK: `id`, FK: `user_id` $\rightarrow$ `users(id)` |
| 3 | `permissions` | `Permission` | Danh mục mã quyền hệ thống (`MANAGE_BOOKS`, `MANAGE_ORDERS`, `MANAGE_USERS`,...). | PK: `id`, UK: `code` |
| 4 | `staff_permissions` | `StaffPermission` | Bảng phân quyền chi tiết cho từng nhân viên, ghi nhận ai là người cấp quyền (`assigned_by_id`). | PK: `id`, FK: `staff_id`, `permission_id`, `assigned_by_id` |
| 5 | `categories` | `Category` | Cây phân cấp thể loại 3 tầng (L1 Ngành hàng, L2 Chuyên mục, L3 Chi tiết) với quan hệ tự tham chiếu. | PK: `id`, FK: `parent_id` $\rightarrow$ `categories(id)` |
| 6 | `authors` | `Author` | Hồ sơ tác giả, tiểu sử, ảnh chân dung phục vụ tra cứu. | PK: `id`, UK: `slug` |
| 7 | `books` | `Book` | Thông tin đầu sách: giá bìa, giá bán, tồn kho, đã bán, thông số kỹ thuật, đánh giá trung bình. | PK: `id`, UK: `slug`, `isbn` |
| 8 | `book_authors` | `BookAuthor` | Bảng liên kết Nhiều-Nhiều (N-N) giữa Sách và Tác giả. | PK: (`book_id`, `author_id`) |
| 9 | `book_categories` | `BookCategory` | Bảng liên kết Nhiều-Nhiều (N-N) giữa Sách và Danh mục thể loại. | PK: (`book_id`, `category_id`) |
| 10 | `orders` | `Order` | Đơn hàng với mã định danh `TS-YYYYMMDD-XXXX`, địa chỉ, tiền hàng, phí ship, trạng thái đơn và thanh toán. | PK: `id`, UK: `order_code`, FK: `user_id` |
| 11 | `order_items` | `OrderItem` | Chi tiết từng cuốn sách trong đơn hàng, lưu snapshot giá và tên sách tại thời điểm mua. | PK: `id`, FK: `order_id`, `book_id` |
| 12 | `reviews` | `Review` | Đánh giá sao (1-5) và nhận xét của khách hàng, có cờ xác thực đã mua hàng và trạng thái duyệt. | PK: `id`, FK: `book_id`, `user_id` |
| 13 | `admin_audit_logs` | `AuditLog` | Nhật ký kiểm toán bảo mật: ghi nhận mọi thao tác thêm/sửa/xóa nhạy cảm của Admin và Staff. | PK: `id`, FK: `user_id` |
| 14 | `user_behavior_events` | `BehaviorEvent` | Thu thập nhật ký hành vi người dùng (xem sách, tìm kiếm, thêm giỏ) phục vụ thống kê & gợi ý AI. | PK: `id`, FK: `user_id`, `book_id` |

---

# PHẦN 3: Toàn Bộ Sơ Đồ Tiêu Chuẩn Cho Báo Cáo Đồ Án

## 3.1 Sơ đồ Ca Sử Dụng Tổng Quan (System Use Case Overview)

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

        UC_ManageBooks["Quản lý Kho sách & Giá bán"]
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

## 3.2 Phân Rã Use Case Theo Từng Phân Hệ

### A. Phân hệ Khách hàng (Customer Use Cases)
```mermaid
graph TD
    ActorUser((Độc giả / Khách hàng))

    subgraph UC_Customer_Group["Phân Hệ Mua Sắm & Trải Nghiệm Khách Hàng"]
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

### B. Phân hệ Quản trị & Vận hành (Staff & Admin Use Cases)
```mermaid
graph TD
    ActorStaff((Nhân Viên Kho / Đơn))
    ActorAdmin((Super Admin))

    subgraph UC_Admin_Group["Phân Hệ Quản Trị & Vận Hành B2C"]
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

### Luồng 1: Quy trình Đặt hàng & Thanh toán Chuẩn TMĐT B2C (Checkout Flow)
```mermaid
stateDiagram-v2
    [*] --> DuyetGioHang: Khách hàng vào trang Giỏ hàng (/cart)
    DuyetGioHang --> KiemTraGio: Nhấn nút 'Tiến hành đặt hàng'
    
    state KiemTraGio <<choice>>
    KiemTraGio --> GioRong: Số lượng = 0
    GioRong --> DuyetGioHang: Báo lỗi và Quay lại xem sách
    KiemTraGio --> KiemTraAuth: Giỏ hàng có sách (>= 1)

    state KiemTraAuth <<choice>>
    KiemTraAuth --> YeuCauDangNhap: Chưa đăng nhập (Khách vãng lai)
    YeuCauDangNhap --> TrangAuth: Chuyển hướng sang /auth?redirect=/checkout
    TrangAuth --> NhapThongTin: Đăng nhập hoặc Đăng ký thành công
    KiemTraAuth --> NhapThongTin: Đã có tài khoản đăng nhập
    
    NhapThongTin --> ChonPhuongThuc: Điền Họ tên, SĐT, Địa chỉ nhận hàng
    
    state ChonPhuongThuc <<choice>>
    ChonPhuongThuc --> ThanhToanCOD: Chọn COD (Nhận hàng trả tiền)
    ChonPhuongThuc --> ThanhToanQR: Chọn Chuyển khoản VietQR
    
    ThanhToanQR --> SinhMaQR: Hệ thống sinh mã QR kèm cú pháp Mã Đơn
    SinhMaQR --> LuuDonHang: Chờ xác nhận chuyển khoản
    ThanhToanCOD --> LuuDonHang: Xác nhận đơn
    
    LuuDonHang --> TruTonKho: Mở Prisma Transaction tạo Order gắn userId và OrderItems
    TruTonKho --> XoaGioHang: Trừ stockQty trong kho và Tăng soldCount
    XoaGioHang --> TrangThanhCong: Xóa LocalStorage giỏ hàng và Điều hướng /order/success
    TrangThanhCong --> [*]
```

### Luồng 2: Quy trình Xác thực & Phân quyền RBAC (Middleware Gateway Flow)
```mermaid
stateDiagram-v2
    [*] --> NguoiDungGuiRequest: Gửi Request tới URL
    NguoiDungGuiRequest --> MiddlewareKiemTra: Next.js Proxy hoặc Middleware đón chặn

    state KiemTraLoaiRoute <<choice>>
    MiddlewareKiemTra --> RouteCongKhai: Route công khai (/, /catalog, /book/*, /cart)
    MiddlewareKiemTra --> RouteYeuCauAuth: Route bảo vệ (/checkout, /account/*, /admin/*)

    RouteCongKhai --> ChoPhepTruyCap: Render trang bình thường

    state KiemTraToken <<choice>>
    RouteYeuCauAuth --> KiemTraToken: Đọc cookie tosach_token
    KiemTraToken --> KhongCoToken: Token rỗng hoặc Hết hạn
    KhongCoToken --> ChuyenHuongAuth: Redirect về /auth?redirect=URL

    KiemTraToken --> CoTokenHopLe: Giải mã JWT (jose HS256)
    
    state KiemTraVaiTro <<choice>>
    CoTokenHopLe --> KiemTraVaiTro: Kiểm tra payload.role
    KiemTraVaiTro --> RouteAdmin: Yêu cầu quyền Admin (/admin/*)
    KiemTraVaiTro --> RouteKhachHang: Yêu cầu thanh toán hoặc hồ sơ (/checkout, /account)

    RouteKhachHang --> ChoPhepTruyCap: role in [USER, STAFF, SUPER_ADMIN]
    
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

### Luồng 1: Đăng nhập Hệ thống & Thiết lập HttpOnly Cookie JWT
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
        API-->>UI: HTTP 401 - Email hoặc mật khẩu không chính xác
        UI-->>User: Hiển thị thông báo lỗi
    else Email hợp lệ
        API->>API: bcrypt.compare(password, passwordHash)
        alt Mật khẩu sai
            API-->>UI: HTTP 401 - Email hoặc mật khẩu không chính xác
            UI-->>User: Hiển thị thông báo lỗi
        else Mật khẩu chính xác
            API->>API: Tạo JWT Token HS256 (userId, role, fullName)
            API->>Cookie: Set-Cookie tosach_token=JWT, HttpOnly, SameSite=Lax, Path=/
            API-->>UI: HTTP 200 { success: true, user: { id, email, role } }
            UI-->>User: Chuyển hướng về Trang chủ hoặc Trang Admin
        end
    end
```

---

### Luồng 2: Khách hàng Đặt hàng & Xử lý Transaction Cơ Sở Dữ Liệu
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

## 3.6 Sơ Đồ Cấu Trúc Thông Tin & Điều Hướng (Site Map / Information Architecture)

```mermaid
graph TD
    Root["Trang Chủ Tổ Sách (/)"]

    subgraph PublicRoutes["PHÂN HỆ KHÁCH HÀNG (Public / Customer)"]
        Catalog["Danh Mục & Tìm Kiếm (/catalog)"]
        BookDetail["Chi Tiết Sách (/book/[slug])"]
        Wishlist["Tủ Sách Yêu Thích (/wishlist)"]
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
    Root --> Wishlist
    Root --> Cart
    Root --> Auth
    
    Wishlist -.->|Chuyển vào giỏ| Cart
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

# PHẦN 4: Kế Hoạch Báo Cáo Tiến Độ 8 Tuần

> **Khung thời gian:** Từ **07/09/2026** đến **30/10/2026** (~8 tuần).  
> **Quy định báo cáo:** Báo cáo định kỳ **2 tuần một lần** (4 cột mốc lớn) kèm nhật ký công việc chi tiết từng tuần.

```
[TUẦN 1 - 2] =============> 🎯 BÁO CÁO ĐỢT 1 (20/09/2026): Nền tảng, Database & Auth [HOÀN THÀNH 100%]
[TUẦN 3 - 4] =============> 🎯 BÁO CÁO ĐỢT 2 (04/10/2026): Storefront Khách Hàng (Catalog, Cart, Checkout)
[TUẦN 5 - 6] =============> 🎯 BÁO CÁO ĐỢT 3 (18/10/2026): Phân Hệ Admin & Phân Quyền RBAC
[TUẦN 7 - 8] =============> 🎯 BÁO CÁO ĐỢT 4 (30/10/2026): Testing UAT, Deploy Production & Bảo Vệ
```

---

## 4.1 Lộ Trình 4 Chặng Báo Cáo (Milestones & Deliverables)

| Đợt Báo Cáo | Thời điểm | Tên Giai Đoạn | Mục tiêu cốt lõi cần đạt | Sản phẩm bàn giao (Deliverables) |
|:---:|:---:|---|---|---|
| **ĐỢT 1** | **20/09/2026**<br/>*(Cuối Tuần 2)* | **Khởi Tạo Nền Tảng, Thiết Kế UI & Kiến Trúc Dữ Liệu** | • Chốt mô hình B2C chuẩn Proposal.<br/>• Thống nhất toàn bộ thiết kế giao diện web (Design System, Wireframes & Mockups các trang chính).<br/>• Dựng Next.js 15, Prisma ORM 14 bảng, kết nối Supabase Cloud.<br/>• Xây dựng Custom JWT Auth & RBAC Middleware.<br/>• Nạp Seed Data mẫu và dựng Shared Layout (Header/Footer). | 1. Source code nhánh `main` trên GitHub.<br/>2. Database 14 bảng hoạt động trên Supabase.<br/>3. Bản quy chuẩn thiết kế UI/UX & Design System.<br/>4. Tài liệu thiết kế hệ thống (`DOCS_KIEN_TRUC_SO_DO_BAO_CAO.md`).<br/>5. Demo test thành công các API Auth. |
| **ĐỢT 2** | **04/10/2026**<br/>*(Cuối Tuần 4)* | **Trải Nghiệm Khách Hàng Toàn Diện (Storefront)** | • Hoàn thiện Trang chủ (Hero, Bestseller, Danh mục).<br/>• Hoàn thiện Trang Catalog bộ lọc 3 cấp & Tìm kiếm sách.<br/>• Trang Chi tiết sách & Form Đánh giá.<br/>• Hoàn thiện Giỏ hàng & Luồng Thanh toán (COD / Chuyển khoản QR) lưu đơn thật vào DB. | 1. Luồng mua hàng hoạt động từ A-Z trên web.<br/>2. Video demo quy trình đặt hàng và trừ tồn kho tự động.<br/>3. Đơn hàng hiển thị chuẩn trong bảng `orders` trên Supabase. |
| **ĐỢT 3** | **18/10/2026**<br/>*(Cuối Tuần 6)* | **Phân Hệ Quản Trị & Vận Hành (Admin & RBAC)** | • Xây dựng Dashboard thống kê doanh thu/tồn kho.<br/>• Module Quản lý kho sách (CRUD sách, upload ảnh bìa).<br/>• Module Quản lý danh mục 3 tầng & Xử lý đơn hàng.<br/>• Module Kiểm duyệt đánh giá & Phân quyền nhân viên động.<br/>• Nhật ký truy vết Audit Logs bảo mật. | 1. Toàn bộ route `/admin/*` hoạt động chuẩn với từng vai trò.<br/>2. Demo tài khoản Staff Kho không vào được trang Quản lý đơn, Staff Đơn không sửa được sách.<br/>3. Báo cáo kiểm thử bảo mật phân quyền. |
| **ĐỢT 4** | **30/10/2026**<br/>*(Nghiệm Thu)* | **Kiểm Thử Toàn Diện, Triển Khai & Hoàn Tất Đồ Án** | • Tích hợp Trợ lý AI Gemini Flash tư vấn sách.<br/>• Tối ưu Core Web Vitals, Responsive 100% Mobile/Tablet.<br/>• Triển khai Production lên Vercel Cloud (Live URL).<br/>• Hoàn tất Quyển Báo cáo Thuyết minh đồ án và Slide bảo vệ. | 1. Đường link trang web chính thức (Live Production URL).<br/>2. Quyển thuyết minh đồ án hoàn chỉnh (PDF/Word).<br/>3. Slide thuyết trình bảo vệ trước Hội đồng.<br/>4. Biên bản nghiệm thu sản phẩm phần mềm. |

---

## 4.2 Bảng Chi Tiết Công Việc Từng Tuần (Tuần 1 đến Tuần 8)

### Tuần 1: Khởi động dự án & Đặc tả kiến trúc (07/09 — 13/09/2026)
*Trạng thái: **ĐÃ HOÀN THÀNH 100%***
* **Mục tiêu:** Định vị mô hình B2C, loại bỏ các thành phần râu ria của prototype cũ, thiết kế ERD.
* **Đầu việc cụ thể:**
  - [x] Rà soát Proposal đồ án liên ngành, chốt mô hình B2C (bỏ bán sách cũ C2C, bỏ voucher, bỏ feed rao bán cá nhân).
  - [x] Lựa chọn Tech Stack: Next.js 15 (App Router), TypeScript, Tailwind CSS v4, Prisma ORM 6, PostgreSQL Supabase.
  - [x] Thiết kế sơ bộ bản vẽ ERD 14 bảng dữ liệu và sơ đồ Use Case tổng quan.
  - [x] Thiết lập Git repository cục bộ và remote GitHub an toàn.

### Tuần 2: Hiện thực hóa nền tảng, Database & Auth (14/09 — 20/09/2026)
*Trạng thái: **ĐÃ HOÀN THÀNH 100% (Đạt Mốc Báo Cáo 1)***
* **Mục tiêu:** Đưa Database lên Cloud, hoàn thành hệ thống Auth và dựng Shared Layout.
* **Đầu việc cụ thể:**
  - [x] Tạo dự án Next.js 15 tại thư mục gốc; lưu trữ code cũ vào [`legacy/`](file:///e:/to-sach-studio/legacy).
  - [x] Viết [`prisma/schema.prisma`](file:///e:/to-sach-studio/prisma/schema.prisma) 14 models; cấu hình Pooler và Direct URL.
  - [x] Đẩy bảng lên Supabase (`prisma db push`); viết và nạp `prisma/seed.ts` (4 users, 8 sách, 15 danh mục).
  - [x] Viết bộ xác thực Custom JWT (`src/lib/auth.ts`) và RBAC Proxy (`src/middleware.ts`).
  - [x] Dựng Header chuẩn Figma: Bố cục 3 cột, triệt tiêu Zero CLS, Top Bar marquee chạy từ trái sang phải, Mega Menu danh mục.
  - [x] Chuẩn hóa Typography: Font `Plus Jakarta Sans` hỗ trợ trọn vẹn dải ký tự và dấu tiếng Việt.
  - [x] Bộ nhận diện thương hiệu: Logo Tổ Sách từ `UI/logo - icon/Group.svg` và favicon tab trình duyệt `src/app/icon.svg`.
  - [x] Xây dựng CartContext với cơ chế Smart Cart Merge (cách ly user/guest, bảo mật đăng xuất).
  - [x] Dựng giao diện Trang chủ (`src/app/page.tsx`) kết nối Prisma query sách thật từ Supabase.
  - [x] Xây dựng Engine Tủ sách Yêu thích (`WishlistContext.tsx`) và trang `/wishlist`.
  - [x] Kết nối Catalog dữ liệu thật 100% từ Database Supabase, đếm đúng số sách theo thể loại/NXB.
  - [x] Kiểm thử biên dịch & Type Safety: `npx tsc --noEmit` đạt 0 lỗi, các trang render HTTP 200.
  - 🎯 **Nhiệm vụ báo cáo:** Đạt Mốc Báo Cáo 1 (Kiến trúc, Database, Auth và Brand Identity).

### Tuần 3: Danh Mục Sách & Trang Chi Tiết (21/09 — 27/09/2026)
*Trạng thái: **ĐANG THỰC HIỆN / TIẾP TỤC HOÀN THIỆN***
* **Mục tiêu:** Xây dựng xong trang duyệt sách theo danh mục và trang xem chi tiết sách.
* **Đầu việc cụ thể:**
  - [ ] Nâng cấp bộ lọc Catalog: Bộ lọc cây danh mục 3 tầng (L1 - L2 - L3), lọc khoảng giá, lọc NXB, lọc tình trạng kho.
  - [ ] Tìm kiếm sách toàn văn theo từ khóa hỗ trợ tiếng Việt không dấu.
  - [ ] Tích hợp tính năng sắp xếp: Giá tăng dần, Giá giảm dần, Mới nhất, Bán chạy nhất.
  - [ ] Xây dựng trang `/book/[slug]`: Render thông tin sách, thư viện ảnh bìa, thông số xuất bản (NXB, số trang, kích thước, định dạng).
  - [ ] Hiển thị danh sách đánh giá đã duyệt từ bảng `reviews` và tính toán sao trung bình.
  - [ ] Xử lý nút "Thêm vào giỏ hàng" và "Mua ngay" (chuyển tiếp tới `/cart` hoặc `/checkout`).

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

# PHẦN 5: Hướng Dẫn Trích Xuất Sơ Đồ Sang Word / PDF / LaTeX

Để đưa các sơ đồ trong tài liệu này vào file báo cáo Word (`.docx`), PDF hoặc LaTeX mà không bị vỡ nét hay nhòe chữ, bạn áp dụng 2 cách cực kỳ nhanh sau:

### Cách 1: Sử dụng Mermaid Live Editor (Khuyên dùng để lấy ảnh sắc nét nhất)
1. Truy cập trang web miễn phí: [mermaid.live](https://mermaid.live).
2. Sao chép đoạn mã của sơ đồ bạn cần (toàn bộ khối mã `mermaid`).
3. Dán vào khung soạn thảo bên trái.
4. Ở góc dưới bên phải màn hình preview, bấm nút **Actions** $\rightarrow$ Chọn **Download PNG (High Res)** hoặc **Download SVG** (Vector phóng to không bao giờ vỡ nét).
5. Chèn trực tiếp file ảnh tải về vào tài liệu Word báo cáo.

### Cách 2: Chụp trực tiếp từ VS Code / Antigravity IDE
1. Cài đặt tiện ích mở rộng **Markdown Preview Enhanced** trên VS Code.
2. Mở file tài liệu này và bấm `Ctrl + Shift + V` để mở bản xem trước.
3. Nhấp chuột phải vào bất kỳ sơ đồ nào $\rightarrow$ Chọn **Save as Image (PNG)**.
