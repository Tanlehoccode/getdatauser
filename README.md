// Xác thực người dùng
function authenticateUser() {
  // Lấy thông tin người dùng từ form
  const username = document.querySelector("input[name='username']").value;
  const password = document.querySelector("input[name='password']").value;

  // Tạo đối tượng với thông tin người dùng
  const user = {
    username,
    password
  };

  // Gọi API xác thực người dùng
  fetch("/api/auth/login", {
    method: "POST",
    body: JSON.stringify(user)
  })
    .then(response => response.json())
    .then(data => {
      // Nếu người dùng được xác thực thành công
      if (data.success) {
        // Lấy thông tin người dùng từ API
        const userInfo = data.data;

        // Truyền thông tin người dùng vào cơ sở dữ liệu
        saveUserInfo(userInfo);
      } else {
        // Hiển thị lỗi
        alert("Tên người dùng hoặc mật khẩu không chính xác.");
      }
    });
}

// Lưu thông tin người dùng vào cơ sở dữ liệu
function saveUserInfo(userInfo) {
  // Tạo đối tượng truy vấn
  const query = new URLSearchParams();

  // Thêm thông tin người dùng vào đối tượng truy vấn
  query.append("username", userInfo.username);
  query.append("email", userInfo.email);
  query.append("full_name", userInfo.full_name);

  // Gọi API lưu thông tin người dùng
  fetch("/api/users", {
    method: "POST",
    headers: {
      "Content-Type": "application/x-www-form-urlencoded"
    },
    body: query
  })
    .then(response => response.json())
    .then(data => {
      // Nếu thông tin người dùng được lưu thành công
      if (data.success) {
        // Chuyển hướng người dùng đến trang chủ
        window.location.href = "/";
      } else {
        // Hiển thị lỗi
        alert("Có lỗi xảy ra khi lưu thông tin người dùng.");
      }
    });
}

// Sự kiện đăng nhập
document.querySelector("form").addEventListener("submit", authenticateUser);

