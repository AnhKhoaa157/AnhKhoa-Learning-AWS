---
title: "Blog 3 - Node.js 22 runtime trong AWS Lambda"
date: 2024-11-21
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# Node.js 22 runtime hiện có sẵn trong AWS Lambda

**Tác giả**: Julian Wood và Andrea Amorosi  
**Ngày đăng**: 21/11/2024  
**Danh mục**: Amazon CloudFront, Announcements, AWS CDK, AWS Lambda, AWS SDK for JavaScript, AWS SAM, Lambda@Edge, Serverless

---

## Giới thiệu

Bạn giờ đây có thể phát triển các hàm AWS Lambda sử dụng Node.js 22 runtime, một phiên bản đang trong trạng thái LTS (Long Term Support) sẵn sàng cho sử dụng production. Node.js 22 bao gồm một số bổ sung ngôn ngữ, bao gồm `require()` cho ES modules, cũng như các thay đổi đối với triển khai runtime và thư viện chuẩn.

Bạn có thể phát triển hàm Lambda với Node.js 22 sử dụng AWS Management Console, AWS CLI, AWS SDK for JavaScript, AWS SAM, AWS CDK, và các công cụ infrastructure as code khác.

Để sử dụng phiên bản mới này, chỉ định giá trị tham số runtime là `nodejs22.x` khi tạo hoặc cập nhật hàm, hoặc bằng cách sử dụng container base image phù hợp.

Bạn cũng có thể sử dụng Node.js 22 với **Powertools for AWS Lambda (TypeScript)**, một bộ công cụ phát triển để triển khai các best practices serverless. Bạn cũng có thể sử dụng Node.js 22 với **Lambda@Edge** để tùy chỉnh nội dung độ trễ thấp được phân phối qua Amazon CloudFront.

Bài viết này làm nổi bật các thay đổi quan trọng đối với Node.js runtime, các cập nhật ngôn ngữ Node.js 22 đáng chú ý, và cách bạn có thể sử dụng Node.js 22 runtime mới trong các ứng dụng serverless của mình.

---

## Cập nhật Ngôn ngữ Node.js 22

Node.js 22 giới thiệu một số cập nhật và tính năng ngôn ngữ nhằm nâng cao năng suất của nhà phát triển và cải thiện hiệu suất ứng dụng.

### require() cho ECMAScript Modules

Phiên bản này thêm hỗ trợ cho việc tải ECMAScript modules (ESM) sử dụng `require()`. Bạn có thể kích hoạt tính năng này sử dụng flag `--experimental-require-module` thông qua cấu hình biến môi trường NODE_OPTIONS.

Hỗ trợ cho `require()` cho đồ thị ESM đồng bộ tạo ra một cây cầu giữa CommonJS và ESM, cung cấp sự linh hoạt hơn trong việc tải module. Quan trọng là lưu ý tính năng này hiện đang là thử nghiệm và có thể thay đổi trong các phiên bản tương lai.

### Hỗ trợ WebSocket

Hỗ trợ WebSocket, trước đây có sẵn đằng sau flag `--experimental-websocket`, giờ được kích hoạt mặc định trong Node.js 22. Điều này mang triển khai WebSocket client tương thích trình duyệt đến Node.js mà không cần dependencies bên ngoài. Hỗ trợ native đơn giản hóa việc xây dựng các ứng dụng thời gian thực.

### Cải thiện Hiệu suất

Runtime mới cũng bao gồm các cải thiện hiệu suất cho việc tạo AbortSignal. Điều này làm cho các hoạt động mạng nhanh hơn và hiệu quả hơn cho Fetch API và test runner. Fetch API cũng giờ được coi là ổn định trong Node.js 22.

### Hỗ trợ TypeScript

Đối với người dùng TypeScript, Node.js 22 giới thiệu hỗ trợ thử nghiệm cho việc chuyển đổi cú pháp chỉ dành cho TypeScript thành mã JavaScript. Sử dụng flag `--experimental-transform-types`, bạn có thể kích hoạt tính năng này để hỗ trợ cú pháp TypeScript như Enum và namespace trực tiếp.

Mặc dù bạn có thể kích hoạt tính năng này trong Lambda, entrypoint hàm của bạn (tức là, `index.mjs` hoặc `app.cjs`) hiện tại không thể được viết bằng TypeScript vì runtime mong đợi một tệp với phần mở rộng JavaScript. Bạn có thể sử dụng TypeScript cho bất kỳ module nào khác được import trong codebase của bạn.

---

## Tính năng Thử nghiệm Không Có sẵn

Node.js 22 bao gồm một tính năng thử nghiệm để tự động phát hiện cú pháp module (CommonJS hoặc ES Modules). Tính năng này phải được kích hoạt khi Node.js runtime được biên dịch.

Vì Node.js 22 runtime được cung cấp bởi Lambda được dành cho các workload production, tính năng thử nghiệm này không được kích hoạt trong bản build Lambda và không thể được kích hoạt thông qua các flag thời gian thực thi.

Để sử dụng tính năng này trong Lambda, bạn cần triển khai Node.js runtime của riêng bạn sử dụng custom runtime hoặc container image với tính năng phát hiện cú pháp module thử nghiệm được kích hoạt.

---

## Cân nhắc Hiệu suất

Khi ra mắt, các runtime Lambda mới nhận được ít sử dụng hơn so với các runtime đã thiết lập hiện có. Điều này có thể dẫn đến thời gian cold start lâu hơn do giảm residency cache trong các hệ thống con Lambda nội bộ.

Thời gian cold start thường cải thiện trong các tuần sau khi ra mắt khi mức sử dụng tăng lên. Do đó, **AWS khuyến nghị không rút ra kết luận** từ các so sánh hiệu suất side-by-side với các runtime Lambda khác cho đến khi hiệu suất đã ổn định.

Vì hiệu suất phụ thuộc nhiều vào workload, khách hàng có các workload nhạy cảm với hiệu suất nên tiến hành testing riêng của họ thay vì dựa vào các benchmark test chung.

Các builder nên tiếp tục đo lường và test hiệu suất hàm và tối ưu hóa mã hàm và cấu hình cho bất kỳ tác động nào.

---

## Migration từ Node.js Runtimes Trước đó

### AWS SDK for JavaScript

Cho đến Node.js 16, các runtime Lambda Node.js bao gồm AWS SDK for JavaScript phiên bản 2. Điều này đã được thay thế bởi AWS SDK for JavaScript phiên bản 3, được phát hành vào tháng 12 năm 2022.

Bắt đầu với Node.js 18, và tiếp tục với Node.js 22, các runtime Lambda Node.js bao gồm phiên bản 3. Khi nâng cấp từ Node.js 16 hoặc các runtime cũ hơn và sử dụng phiên bản 2 đã bao gồm, **bạn phải nâng cấp mã của mình để sử dụng v3 SDK**.

Để có hiệu suất tối ưu và có toàn quyền kiểm soát các dependencies mã của bạn, chúng tôi khuyến nghị **bundling và minifying AWS SDK** trong gói triển khai của bạn thay vì sử dụng SDK được bao gồm trong runtime.

### Amazon Linux 2023

Node.js 22 runtime được dựa trên `provided.al2023` runtime, được xây dựng trên Amazon Linux 2023 minimal container image. Amazon Linux 2023 minimal image sử dụng **microdnf** làm package manager, được symlink là `dnf`.

Điều này thay thế trình quản lý gói **yum** được sử dụng trong Node.js 18 và các image dựa trên AL2 trước đó. Nếu bạn triển khai hàm Lambda của mình dưới dạng container image, **bạn phải cập nhật Dockerfile của mình để sử dụng `dnf` thay vì `yum`** khi nâng cấp lên Node.js 22 base image từ Node.js 18 hoặc trước đó.

---

## Kết luận

Node.js 22 runtime mang đến nhiều cải tiến về ngôn ngữ, hiệu suất và công cụ cho các nhà phát triển serverless trên AWS Lambda. Với hỗ trợ cho ES modules, WebSocket native, và TypeScript transformation, Node.js 22 cho phép bạn xây dựng các ứng dụng hiện đại và hiệu quả hơn.

Hãy bắt đầu sử dụng Node.js 22 trong Lambda ngay hôm nay! 