# Path Traversal

### Absolute Path và Relative Path

#### 1. Absolute Path là gì?

Đường dẫn tuyệt đối (absolute path) là đường dẫn đầy đủ đến một file hoặc thư mục, bắt đầu từ gốc của hệ thống tệp hoặc tên miền. Đường dẫn tuyệt đối bao gồm tất cả các thông tin cần thiết để định vị tài nguyên mà không phụ thuộc vào vị trí hiện tại của file hoặc thư mục.

Dùng cấu trúc thư mục ở trên, đường dẫn tuyệt đối đến “logo.png” có thể là:
```
<img src=“http://domain.com/images/logo.png” alt=“Logo”>
```

#### 2. Relative Path là gì?

Đường dẫn tương đối (relative path) là đường dẫn đến một file hoặc thư mục dựa trên vị trí hiện tại của file hoặc thư mục chứa nó. Đường dẫn này không bao gồm tên miền hoặc địa chỉ tuyệt đối của tài nguyên, mà chỉ chỉ định đường đi từ vị trí hiện tại đến tài nguyên mong muốn.

Giả sử bạn có cấu trúc thư mục như sau:
```
/website
    /images
        logo.png
    /css
        style.css
    index.html
```
Trong file index.html, để liên kết đến logo.png, bạn có thể sử dụng đường dẫn tương đối như sau:

```
<img src=“images/logo.png” alt=“Logo”>
```

Trong Relative Path, có 2 loại đường dẫn tương đối:

**2.1 Forward Path (Đường dẫn tiến):**

Đường dẫn tiến (forward path) sử dụng để truy cập các tệp hoặc thư mục con (subfolders) nằm bên trong thư mục hiện tại. Điều này hữu ích khi bạn muốn liên kết đến các tài nguyên ở cùng một cấp hoặc trong một thư mục con.

**Ví dụ:** Giả sử bạn có cấu trúc thư mục sau:

```
/website
    /images
        logo.png
    /css
        style.css
    index.html
```

Trong tệp HTML “index.html”, bạn muốn sử dụng hình ảnh “logo.png” trong thư mục “images.” Bạn có thể sử dụng đường dẫn tiến như sau:

```
<img src="images/logo.png">
```

**2.2 Backward Path (Đường dẫn lùi):**

Đường dẫn lùi (backward path) được sử dụng để truy cập các tệp hoặc thư mục cha (parent folders) so với thư mục hiện tại. Điều này hữu ích khi bạn muốn truy cập tài nguyên nằm ở thư mục cha hoặc các mức thư mục cao hơn.

**Ví dụ:** Giả sử bạn có cấu trúc thư mục sau:

```
/website
    /images
        logo.png
    /css
        style.css
        project.html
    index.html
```

Trong tệp “project.html,” bạn muốn sử dụng hình ảnh “logo.png” trong thư mục “images,” nằm ở cùng một cấp với thư mục "css". Bạn có thể sử dụng đường dẫn lùi như sau:

```
<img src="../images/logo.png">
```

Trong trường hợp này, đường dẫn ../ cho biết bạn muốn đi từ tệp “project.html” lên thư mục cha (website) và sau đó vào thư mục “images.”

Tương tự, nếu bạn muốn đi lùi bao nhiêu thư mục bạn sử dụng cú pháp “../” để lùi bấy nhiêu thư mục tương ứng.


### Path traversal là gì?

Lỗ hổng web Path Traversal (còn được gọi là Directory Traversal) là một lỗ hổng bảo mật phổ biến trên các ứng dụng web. Lỗ hổng này cho phép kẻ tấn công truy cập vào các tệp tin và thư mục trên máy chủ web mà không được phép truy cập. Điều này có thể gây ra nhiều nguy hiểm cho hệ thống web và dữ liệu của người dùng.

<p align="center">
<img width="801" alt="Ảnh chụp Màn hình 2025-03-31 lúc 15 12 58" src="https://github.com/user-attachments/assets/80908bfd-52bf-4aad-9b5d-fe9121f9ce5a" />
</p>

Cơ chế hoạt động của lỗ hổng Path Traversal là kẻ tấn công sử dụng ký tự dot-dot-slash (“../”) để truy cập vào các thư mục cha của thư mục hiện tại. Khi kẻ tấn công thành công trong việc truy cập vào các thư mục cha, họ có thể tiếp tục truy cập vào các thư mục khác và thậm chí có thể truy cập vào các tệp tin quan trọng trên máy chủ web.

### Nguyên nhân

Hầu hết lỗ hổng File Path Traversal xuất hiện trong các chức năng liên quan tới việc xử lý các File hoặc Directory. Lập trình viên cho phép người dùng hoặc ứng dụng đưa tên file hoặc đường dẫn mà không loại bỏ các dấu chấm và gạch chéo “/”, “.”. Như đã giải thích ở phía trên, hai dấu này sẽ giúp chúng ta di chuyển qua lại các thư mục.

Một số chức năng trong Web hay xuất hiện lỗ hổng này:

- Download File
- Upload File
- Export
- Load Resources (Images, CSS, JS,..)
- Load Page (?menu=home, ?menu=about, ?page=home, ?page=search)


### Cách thức phát hiện: 

#### 1. Phương pháp thủ công 
**1.1. Trường hợp không có mã nguồn (Black-box Testing)**
**Nhận diện điểm nghi ngờ:**

URL/parameter khả nghi: Những param như ```file=```, ```path=```, ```download=```, ```img=```, ```lang=```, ```doc=```, ```page=```, v.v.

**Dấu hiệu trên giao diện:**

Ứng dụng cho phép tải, xem file, đổi ngôn ngữ, hiển thị template…

Xuất hiện lỗi như: ```file not found```, ```no such file```, ```failed to open```, v.v.

**Kỹ thuật kiểm thử:**
**Gửi payloads:**
```
../../etc/passwd
..%2F..%2Fetc%2Fpasswd
..%c0%af..%c0%afetc%c0%afpasswd
```
Dùng công cụ như Burp Suite (Intruder) để fuzz các param nghi ngờ.

 **Dấu hiệu thành công:**
Trả về nội dung file hệ thống ```(root:x:0:0:...)```

Báo lỗi liên quan đến đường dẫn thực ```(open(/etc/passwd) failed)```

Stack trace chỉ ra vị trí sử dụng hàm file như ```open()```, ```fopen()```...

**1.2. Trường hợp có mã nguồn (White-box Testing)**
**Phân tích luồng dữ liệu:**

 Tìm **Source → Sink**
**Source:** dữ liệu nhập vào từ người dùng (```GET```, ```POST```, ```cookies```, ```headers```...)

**Sink:** nơi sử dụng dữ liệu để thao tác với file

**Ví dụ các hàm nguy hiểm:**

**PHP:** ``fopen()``, ``file_get_contents()``, ``readfile()``, ``include()``

**Python:** ``open()``, ``os.path.join()``, ``send_file()``

**Java:** ``FileInputStream``, ``ServletContext.getResourceAsStream()``

Ví dụ PHP:
```
// source: $_GET['file']
// sink: file_get_contents
file_get_contents($_GET['file']);
```
**Cách nhận diện:**
Dùng **grep** hoặc **Semgrep** tìm nhanh:

```
grep -R "file_get_contents("
semgrep --config "p/php.lang.security.path-traversal" .
```

**Dấu hiệu dễ khai thác:**
Không có ```basename()```, ```realpath()```, kiểm tra whitelist trước khi truy cập file.

Kết hợp trực tiếp dữ liệu người dùng với đường dẫn tương đối hoặc tuyệt đối.

#### 2. Phương pháp tự động
**2.1. Dùng công cụ scan truyền thống**
**Các tool phổ biến:**

- Burp Suite (Scanner hoặc Intruder + Wordlist path traversal)

- OWASP ZAP (Active Scan + Add-on path traversal)

- Nikto

- bWfuzz, ffuf – kết hợp với wordlist payloads như:
```
../../etc/passwd
../windows/win.ini
%2e%2e/, %252e%252e%252f, ..%c1%1c..%c1%1c
```

**Ưu điểm:**
Phát hiện nhanh các điểm yếu phổ biến.

Dễ sử dụng, tích hợp CI/CD.

**Hạn chế:**
Dễ bypass nếu ứng dụng encode, normalize hoặc kiểm tra sâu.

**2.2. Áp dụng AI / Machine Learning**
**Ý tưởng:**
Huấn luyện mô hình để phân loại truy vấn độc hại chứa path traversal dựa trên đặc trưng request.

Đặc trưng có thể sử dụng:
Tần suất xuất hiện: ```../```, ``%2e``, ``%2f``, ký tự đặc biệt.

Độ dài chuỗi.

Loại HTTP method (```GET```/```POST```).

Thời gian phản hồi (response time).

Mã phản hồi HTTP (200, 403, 500…).

Các đặc trưng thống kê hoặc embedding chuỗi URL.

Mô hình áp dụng:
- Supervised:

    Decision Tree, Random Forest, SVM, Logistic Regression.

- Unsupervised:

    Isolation Forest, One-Class SVM để phát hiện bất thường.

- Deep Learning:

    LSTM/Transformer phân tích chuỗi URL và param.

**Dữ liệu huấn luyện:**
Có thể lấy từ:

- OWASP vulnerable dataset (DVWA, WebGoat logs).

- Log thực tế (Apache, Nginx).

- Tự tạo dựa trên mẫu truy vấn hợp lệ + payload.

### Ví dụ khai thác về lỗ hổng Path Traversal:

### Cách thức phòng chống Path Traversal:

 **1. Kiểm tra và lọc đầu vào (Input Validation & Sanitization)**
 
Không bao giờ tin tưởng dữ liệu từ người dùng, kể cả từ cookies, headers hay params.

Loại bỏ hoặc mã hóa các chuỗi ``../``, ``..\``, ``%2e%2e/``, v.v.

Dùng whitelist – chỉ cho phép tên file hợp lệ (ví dụ: ```[a-zA-Z0-9_\-\.]```).

Ví dụ bằng PHP:

```
$filename = basename($_GET['file']); // loại bỏ ../
```
**2. Giới hạn thư mục truy cập (Directory Whitelisting / Chroot Jail)**

Giới hạn việc truy cập tập tin vào một thư mục cụ thể:

```
$base = realpath('/var/www/uploads');
$path = realpath($base . '/' . $_GET['file']);

if (strpos($path, $base) !== 0) {
    die("Access denied!");
}
```
- Dùng realpath() để kiểm tra đường dẫn tuyệt đối thực tế.

**3. Phân quyền hệ thống file (File System Permissions)**

Chạy ứng dụng web bằng user không có quyền root/admin.

Đảm bảo user web (như ```www-data```, ```nginx```, ```apache```) chỉ có quyền đọc/thực thi thư mục cần thiết.

Không cấp quyền ghi/đọc toàn bộ hệ thống file trừ khi bắt buộc.

**4. Sử dụng thư viện xử lý file an toàn**

Tránh ``fopen()``, ``file_get_contents()`` với đầu vào không kiểm soát.

Sử dụng các API nội bộ hoặc frameworks có xử lý an toàn đường dẫn (VD: ``Symfony Filesystem``, ``Node.js``, ``path.join``...).

**5. Tắt liệt kê thư mục (Disable Directory Listing)**

Cấu hình web server:

Apache: ``Options -Indexes``

Nginx: ``autoindex off``;

**6. Bảo vệ file cấu hình và nhạy cảm**

``.htaccess``, ``.env``, ``config.php``, v.v. cần được bảo vệ kỹ:

Không lưu trong thư mục có thể truy cập trực tiếp từ web.

Cấu hình deny trong web server:
```
<FilesMatch "(\.htaccess|\.env|config\.php)">
  Require all denied
</FilesMatch>
```
**7. Ghi log và giám sát**

Ghi lại các truy vấn chứa dấu hiệu như ``../``, ``%2e%2e``, hoặc truy cập file lạ.

Cảnh báo qua IDS hoặc SIEM khi có hành vi bất thường.

**8. Sử dụng WAF / IDS / AI để phát hiện và chặn**\

Sử dụng Web Application Firewall (ModSecurity, AWS WAF, Cloudflare...) để chặn mẫu tấn công phổ biến.

Tích hợp AI để học các mẫu truy cập bình thường và cảnh báo khi có bất thường.

**9. Kiểm thử định kỳ**

Dùng các công cụ như:

- Burp Suite (Repeater + Intruder + Scanner)

- OWASP ZAP

- Nikto

- Linh hoạt fuzz các tham số có thể chứa đường dẫn

- Thường xuyên kiểm tra và cập nhật mã nguồn.

<---------------------------------------------- Nơi render ảnh để lấy đường link chèn vào bài viết---------------------------------------------->

<p align="center">
<img width="801" alt="Ảnh chụp Màn hình 2025-03-31 lúc 15 12 58" src="https://github.com/user-attachments/assets/80908bfd-52bf-4aad-9b5d-fe9121f9ce5a" />
</p>


<p align="center">
<img width="801" alt="Ảnh chụp Màn hình 2025-03-31 lúc 15 12 58" src="https://github.com/user-attachments/assets/ab000b9a-53d0-4d94-80d0-7df7b69e8aec" />
</p>



<p align="center">
<img width="801" alt="Ảnh chụp Màn hình 2025-03-31 lúc 15 12 58" src="https://github.com/user-attachments/assets/78328e39-e06e-45a9-9e37-3e390d930503" />
</p>

<p align="center">
<img width="801" alt="Ảnh chụp Màn hình 2025-03-31 lúc 15 12 58" src="" />
</p>

<p align="center">
<img width="801" alt="Ảnh chụp Màn hình 2025-03-31 lúc 15 12 58" src="" />
</p>

<p align="center">
<img width="801" alt="Ảnh chụp Màn hình 2025-03-31 lúc 15 12 58" src="" />
</p>

<p align="center">
<img width="801" alt="Ảnh chụp Màn hình 2025-03-31 lúc 15 12 58" src="" />
</p>

