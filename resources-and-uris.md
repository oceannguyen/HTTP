# [Introduction to MIME Types](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_Types)

Kiểu mở rộng thư điện tử đa phương tiện (Multipurpose Internet Mail Extensions - MIME) là một cách chuẩn hóa để chỉ ra bản chất và định dạng của một tài liệu.

Trình duyệt thường dùng kiểu MIME để xác định cách xử lý văn bản, do đó điều quan trọng là các servers được thiết lập để trả về đúng kiểu MIME trong phần phần **header** of đối tượng **response**.

## Syntax
### General structure

> type/subtype

*type* đại diện cho loại, có thẻ là *discrete* hoặc *multipart*
*subtype* là cụ thể của *type*

### Discrete types

```
text/plain
text/html
image/jpeg
image/png
audio/mpeg
audio/ogg
audio/*
video/mp4
application/*
application/json
application/javascript
application/ecmascript
application/octet-stream
```

### Multipart types

> multipart/form-data

> multipart/byteranges

Các kiểu *Multipart* là loại tài liệu được chia thành nhiều dạng khác nhau, có các kiểu MIME khác nhau. Nó là cách để biểu diễn một tài liệu tổng hợp. Ngoại trừ *multipart/form-data*, được dùng tỏng *HTTP Forms* và phương thức POST. 

## Important MIME types for Web developers

### application/octet-stream

Là giá trị mặc định cho file nhị phân. Vì đây là file nhị phân không rõ (unknown), nên trình duyệt thường không thực thi hoặc nếu có thì nó sẽ thực hiện như khi đã thiết lập trong phần header với `Content-Disposition:` có đính kèm theo giá trị và do đó yêu cầu 'Save as'

### text/plain

Đây là giá trị mặc định cho các tệp văn bản. Ngay cả khi nó là tập tin không rõ thì trình duyệt sẽ giả định rằng chúng có thể hiển thị.

### text/css

Bất kỳ file CSS nào phải được thông dịch trong một Web page phải là text/css.

### text/html

Tất cả nội dụng HTML phải được loại này để hiển thị trang web

### Images types

Chỉ có một số ít các loại hình ảnh được công nhận rộng rãi và được xem là an toàn trên web, sẵn sàng để sử dụng trong một trang web.

| MIME type | Image type |
| --- | --- |
| image/gif | GIF images (lossless compression, superseded by PNG) |
| image/jpeg | JPEG images |
| image/png | PNG images |
| image/svg+xml | SVG images (vector images) |

### Audio and video types

Giống như images, HTML không định nghĩa một tập các kiểu hỗ trợ để dùng với thẻ *<audio>* hoặc *<video>*, chỉ có một vài loại được hỗ trợ trên Web.

| MIME type | Audio or video type |
| --- | --- |
| audio/wave |  |
| audio/wav |  |
| audio/x-wav |  |
| audio/x-pn-wav | An audio file in the WAVE container format. The PCM audio codec (WAVE codec "1") is often supported, but other codecs have more limited support (if any).|
| audio/webm | An audio file in the WebM container format. Vorbis and Opus are the most common audio codecs. |
| video/webm | A video file, possibly with audio, in the WebM container format. VP8 and VP9 are the most common video codecs used within it; Vorbis and Opus the most common audio codecs. |
| audio/ogg | An audio file in the OGG container format. Vorbis is the most common audio codec used in such a container. |
| video/ogg | A video file, possibly with audio, in the OGG container format. Theora is the usual video codec used within it; Vorbis is the usual audio codec. |
| application/ogg | An audio or video file using the OGG container format. Theora is the usual video codec used within it; Vorbis is the usual audio codec. |

### multipart/form-data

Kiểu ```multipart/form-data``` được dùng để gửi nội dung của một HTML form từ browser tới server. Vì định dạng là một tài liệu nhiều phần, nên nó chứa nhiều kiểu dữ liệu khác nhau. Mỗi phần là một entity với HTTP headers riêng.

```
Content-Type: multipart/form-data; boundary=aBoundaryString
(other headers associated with the multipart document as a whole)

--aBoundaryString
Content-Disposition: form-data; name="myFile"; filename="img.jpg"
Content-Type: image/jpeg

(data)
--aBoundaryString
Content-Disposition: form-data; name="myField"

(data)
--aBoundaryString
(more subparts)
--aBoundaryString--
```

Ví dụ như form sau:
```html
<form action="http://localhost:8000/" method="post" enctype="multipart/form-data">
  <input type="text" name="myTextField">
  <input type="checkbox" name="myCheckBox">
  <input type="file" name="myFile">
  <button>Send the file</button>
</form>
```
sẽ gửi message: 
```
POST / HTTP/1.1
Host: localhost:8000
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: keep-alive
Upgrade-Insecure-Requests: 1
Content-Type: multipart/form-data; boundary=---------------------------8721656041911415653955004498
Content-Length: 465

-----------------------------8721656041911415653955004498
Content-Disposition: form-data; name="myTextField"

Test
-----------------------------8721656041911415653955004498
Content-Disposition: form-data; name="myCheckBox"

on
-----------------------------8721656041911415653955004498
Content-Disposition: form-data; name="myFile"; filename="test.txt"
Content-Type: text/plain

Simple file.
-----------------------------8721656041911415653955004498--
```

