# JavaScript Cookies vs Local Storage vs Session Storage

Local Storage

1. Khả năng lưu trữ vô thời hạn: Có nghĩa là chỉ bị xóa bằng JavaScript, hoặc xóa bộ nhớ trình duyệt, hoặc xóa bằng localStorage API.
2. Lưu trữ được 5MB: Local Storage cho phép bạn lưu trữ thông tin tương đối lớn lên đến 5MB, lưu được lượng thông tin lớn nhất trong 3 loại.
3. Không gửi thông tin lên server như Cookie nên bảo mật tốt hơn.

## Session Storage

1. Lưu trên Client: Cũng giống như localStorage thì sessionStorage cũng dùng để lưu trữ dữ liệu trên trình duyệt của khách truy cập (client).
2. Mất dữ liệu khi đóng tab: Dữ liệu của sessionStorage sẽ mất khi bạn đóng trình duyệt.
3. Dữ liệu không được gửi lên Server
4. Thông tin lưu trữ nhiều hơn cookie (ít nhất 5MB)

**Session Storage** as a specific browser API (like sessionStorage in JavaScript) is a client-side storage mechanism and does not reside on the server.

However, servers do manage "sessions," which are a way for the server to maintain stateful information about a user's interaction across multiple requests. This server-side session data is distinct from the client-side sessionStorage and is typically identified by a session ID, which is often transmitted to the client via a cookie.

## Cookies

1. Thông tin được gửi lên server: Cookie sẽ được truyền từ server tới browser và được lưu trữ trên máy tính của bạn khi bạn truy cập vào ứng dụng, mỗi khi người dùng send HTML request lên server, trình duyệt sẽ gửi kèm cookies. Vì vậy đừng bao giờ lưu trữ những thông tin quan trọng, yêu cầu tính bảo mật cao vào cookie.
2. Cookie chủ yếu là để đọc phía máy chủ (cũng có thể được đọc ở phía máy khách), localStorage và sessionStorage chỉ có thể được đọc ở phía máy khách.
3. Có thời gian sống: Mỗi cookie thường có khoảng thời gian timeout nhất định do lập trình viên xác định trước.
4. Lưu trữ: cho phép lưu trữ tối đa 4KB và vài chục cookie cho một domain.

