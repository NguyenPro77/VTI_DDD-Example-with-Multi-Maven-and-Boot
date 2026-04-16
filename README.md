# Kiến trúc hệ thống - DDD & Clean Architecture

Source code của project được xây dựng dựa trên tư duy Domain-Driven Design (DDD) kết hợp với Clean Architecture, nhằm mục tiêu tách biệt rõ ràng các tầng, giảm phụ thuộc và đảm bảo tính mở rộng lâu dài.

Hệ thống sử dụng Multi-module Maven cùng với Spring Boot để tạo ra ranh giới vật lý giữa các module. Nhờ đó, các quy tắc phụ thuộc (Dependency Rule) được kiểm soát ngay từ lúc compile.

---

# Tổng quan kiến trúc

Hệ thống được chia thành các module độc lập, mỗi module đảm nhiệm một vai trò riêng biệt:

```text
Application  → Entry point (khởi chạy hệ thống)
Controller   → Tiếp nhận request (REST API)
Service      → Xử lý logic và truy cập dữ liệu
Domain       → Chứa business logic cốt lõi
AuthServer   → Xử lý xác thực và phân quyền
```

---

# Chi tiết từng module

## 1. domain

Vai trò:

* Là trung tâm của hệ thống
* Chứa Entity, Aggregate và các interface

Đặc điểm:

* Không phụ thuộc vào database, UI hay framework bên ngoài
* Giữ logic nghiệp vụ ổn định

---

## 2. controller

Vai trò:

* Nhận request từ client
* Trả response dạng JSON

Thành phần:

* Các lớp @RestController
* Mapping API

Nguyên tắc:

* Không chứa logic phức tạp
* Chỉ gọi vào Service

---

## 3. service

Vai trò:

* Xử lý logic nghiệp vụ
* Tương tác với database

Đặc điểm:

* Implement các interface từ domain
* Là nơi thực thi các use case

---

## 4. authserver

Vai trò:

* Xử lý xác thực và phân quyền
* Quản lý bảo mật hệ thống

Công nghệ:

* Spring Security
* JWT (có thể mở rộng)

---

## 5. application

Vai trò:

* Module khởi chạy chính
* Kết nối tất cả các module

Chức năng:

* Là composition root
* Cho phép Spring inject dependency

---

# Nguyên tắc thiết kế

## Dependency Rule

* Module bên ngoài phụ thuộc vào module bên trong
* Domain không phụ thuộc module nào khác

---

## Dependency Inversion

* Interface đặt ở domain
* Implementation nằm ở service

---

## Tách biệt trách nhiệm

* Controller: giao tiếp
* Service: xử lý
* Domain: logic

---

# Đánh giá kiến trúc

## Ranh giới rõ ràng

* Tách module bằng Maven
* Ngăn chặn truy cập sai layer
* Lỗi được phát hiện ngay khi build

---

## Dễ thay thế

* Có thể thay đổi:

  * REST → GraphQL / gRPC
  * H2 → MySQL / MongoDB

---

## Khả năng mở rộng

* Phù hợp làm việc nhóm
* Có thể phát triển song song các phần

---

# Hướng dẫn sử dụng API

## Database (H2 Console)

```text
http://localhost:8080/h2-console
```

---

## Customer API

### Lấy danh sách khách hàng

```text
GET http://localhost:8080/customers
```

### Lấy chi tiết khách hàng

```text
GET http://localhost:8080/customers/{id}
```

### Tạo khách hàng mới

```text
POST http://localhost:8080/customers?name=<name>&job=<job>
```

### Xóa khách hàng

```text
DELETE http://localhost:8080/customers/{id}
```

---
