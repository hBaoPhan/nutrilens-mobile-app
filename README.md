# AR-NutriLens Mobile Client (React Native + Expo)

Ứng dụng di động **AR-NutriLens** được xây dựng bằng **React Native (Expo SDK 58)**, chuyển đổi và tái hiện toàn bộ giao diện, tính năng và luồng xử lý từ phiên bản Unity AR Client (`unity-ar-client`).

---

## 🌟 Tính Năng Tương Đương Phiên Bản Unity

1. **Trang Chủ (Home Dashboard)**:
   - **Thẻ chào mừng**: Tên người dùng và avatar sinh động.
   - **Calorie Arc Gauge Hero**: Đồng hồ hình bán nguyệt trực quan hiển thị Calo đã tiêu thụ / Mục tiêu hàng ngày, lượng calo còn lại và biểu tượng ngọn lửa.
   - **Chỉ số dinh dưỡng đa lượng (Macros)**: Tinh bột (Carbs - Cam), Chất béo (Fat - Xanh lá), Chất đạm (Protein - Xanh dương) với thanh tiến độ tỷ lệ.
   - **Tổng quan 3 bữa ăn**: Sáng (🌅), Trưa (☀️), Tối (🌇) kèm lượng calo tiêu thụ và nút chuyển nhanh đến nhật ký.
   - **Mẹo dinh dưỡng hàng ngày (Daily Nutrition Tips)**: Thẻ mẹo sức khỏe với phân loại (NƯỚC, ĐẠM, RAU CỦ, THÓI QUEN) cùng nút xáo trộn/đổi mẹo mới.
   - **Gợi ý bữa ăn thông minh (AI RAG Next Meal)**: Phân tích thâm hụt calo trong ngày và gợi ý món ăn phù hợp kèm lý do dinh dưỡng.

2. **Nhật Ký Dinh Dưỡng (Diary & Meal Logs)**:
   - **Date Strip Carousel**: Dải chọn ngày động (14 ngày: các ngày trước, hôm nay, ngày mai) với lịch thứ & ngày.
   - **Bộ lọc bữa ăn**: Tất cả, Bữa Sáng, Bữa Trưa, Bữa Tối.
   - **Thẻ nhật ký bữa ăn**: Chi tiết từng món, khối lượng (g), thể tích (cm³), lượng calo, phân bổ macros, cùng thời gian tập luyện để đốt cháy calo (🏃 phút chạy bộ, 🚶 phút đi bộ) và tính năng xóa bữa ăn.
   - **Modal Điều chỉnh Khẩu phần (Portion Sizing)**: Điều chỉnh từ 50% đến 200%, tự động tính toán lại tức thì Calo, Macros và thời gian chạy bộ.

3. **Quét Dinh Dưỡng & AR Scanner (Camera Viewfinder)**:
   - **Viewfinder chuẩn 1:1**: Khung quét ngắm đĩa thức ăn với 4 góc căn chỉnh (Corner Brackets màu xanh ngọc), tâm ngắm `+` và chỉ dẫn khoảng cách 30-50 cm.
   - **Đèn Flash/Torch & Đổi camera trước/sau**.
   - **Hiệu ứng Flash khi bấm chụp**: Hoạt họa chớp sáng như máy ảnh thật.
   - **Tích hợp Thư viện ảnh (Gallery Picker)**: Cho phép chọn ảnh món ăn từ máy để phân tích.
   - **AI Vision Segmentation**: Kết nối trực tiếp đến FastAPI microservice (`POST /api/v1/vision/segment`) chạy mô hình YOLOv8 instance segmentation.
   - **Phân tích thể tích & dinh dưỡng đa thành phần (Hierarchical Multi-Volumetric Analysis)**: Gọi Spring Boot (`POST /api/v1/nutrition/multi-volumetric-analyze`).
   - **Bảng trượt kết quả (Scan Result Sheet)**: Hiển thị danh sách món/nguyên liệu nhận diện được, điều chỉnh khẩu phần, chọn loại bữa ăn và 1 chạm "✓ Lưu Vào Nhật Ký".

4. **Tài Khoản, Thể Trạng & Cài Đặt (Profile & Settings)**:
   - **Xác thực JWT**: Đăng nhập (`/api/v1/auth/login`) và Đăng ký (`/api/v1/auth/register`).
   - **Cá nhân hóa thể trạng**: Chiều cao, cân nặng, độ tuổi, giới tính, mục tiêu (Giảm cân / Duy trì / Tăng cân), mức độ vận động và mục tiêu Calo/ngày.
   - **Cấu hình mạng LAN / Cloud Server**: Dễ dàng đổi IP máy chủ Host PC (`192.168.1.147` hoặc IP bất kỳ), cổng Spring Boot (`8080`), cổng FastAPI (`8000`) và Custom AI Service URL trực tiếp trên ứng dụng mà không cần build lại mã nguồn.

---

## 🛠️ Cấu Trúc Mã Nguồn

```
mobile-app/
├── src/
│   ├── config/
│   │   └── api.js              # Cấu hình IP/Port, Base URLs, AI Service URL
│   ├── services/
│   │   ├── authService.js      # API Xác thực, JWT Token, Profile
│   │   ├── nutritionService.js # API Món ăn, Nhật ký bữa ăn, Phân tích 3D, Gợi ý AI, Mẹo
│   │   └── visionService.js    # API Gửi ảnh nhận diện YOLOv8-seg (FastAPI)
│   ├── context/
│   │   ├── AuthContext.js      # Quản lý phiên đăng nhập & thông tin người dùng
│   │   ├── NutritionContext.js # Quản lý ngày chọn, nhật ký bữa ăn, tính toán calo
│   │   └── ConfigContext.js    # Quản lý & lưu trữ IP/Port máy chủ
│   ├── components/
│   │   ├── navigation/
│   │   │   └── BottomNavBar.js # Thanh điều hướng dưới cùng & FAB Quét AR nổi
│   │   ├── dashboard/          # CalorieGaugeCard, MacroMetricsRow, MealQuickCards, DailyTipCard, RecommendationCard
│   │   ├── diary/              # DateStrip, MealFilterTabs, MealLogCard, FoodDetailModal
│   │   └── scanner/            # ScanReticle, ScanResultSheet
│   ├── screens/
│   │   ├── HomeScreen.js       # Màn hình Trang Chủ
│   │   ├── DiaryScreen.js      # Màn hình Nhật Ký
│   │   ├── ScannerScreen.js    # Màn hình Camera Quét AR
│   │   └── ProfileScreen.js    # Màn hình Hồ Sơ & Cài Đặt Mạng
│   ├── styles/
│   │   ├── theme.js            # Design tokens, màu sắc, font, shadow
│   │   ├── common.styles.js    # CSS/Style chung
│   │   ├── home.styles.js      # Tách riêng Style cho Home
│   │   ├── diary.styles.js     # Tách riêng Style cho Diary
│   │   ├── scanner.styles.js   # Tách riêng Style cho Scanner
│   │   └── profile.styles.js   # Tách riêng Style cho Profile
│   └── constants/
│       ├── foodsData.js        # Cơ sở dữ liệu 24 món Việt & nguyên liệu đơn
│       └── tipsData.js         # Danh mục mẹo dinh dưỡng
├── App.js                      # Root component bọc Provider & Modal Camera
└── package.json
```

---

## 🚀 Hướng Dẫn Khởi Chạy

### 1. Cài đặt Dependencies (nếu chưa cài)
```bash
cd mobile-app
npm install
```

### 2. Khởi chạy ứng dụng
```bash
# Khởi động Metro Bundler
npx expo start

# Chạy trên thiết bị Android (hoặc Emulator)
npx expo start --android

# Chạy trên iOS Simulator (yêu cầu macOS)
npx expo start --ios

# Chạy trực tiếp trên trình duyệt Web để test nhanh
npx expo start --web
```

### 3. Kết nối với Thiết bị thật qua Expo Go
1. Tải ứng dụng **Expo Go** trên App Store (iOS) hoặc Google Play (Android).
2. Đảm bảo điện thoại và máy tính chạy cùng mạng Wi-Fi LAN.
3. Quét mã QR code xuất hiện trên terminal của lệnh `npx expo start`.
4. Mở tab **Hồ Sơ** -> kéo xuống mục **Cấu hình máy chủ LAN** -> nhập địa chỉ IP Wi-Fi của máy tính (ví dụ: `192.168.1.147`) và nhấn **Lưu Cấu Hình Mạng**.
"# nutrilens-mobile-app" 
