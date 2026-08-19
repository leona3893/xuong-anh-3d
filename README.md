# Xưởng Ảnh 3D

Bộ công cụ tĩnh chạy trong trình duyệt, biến ảnh phẳng thành hiệu ứng 3D.
Không cần cài đặt, không cần server, không gửi ảnh ra ngoài — mở file HTML là dùng.

**Bản công khai:** https://leona3893.github.io/xuong-anh-3d/
**Mã nguồn:** https://github.com/leona3893/xuong-anh-3d

## Muốn sửa hay thêm tính năng thì làm gì

Nguồn duy nhất là **`index.html`** ở thư mục này. Sửa xong thì ba lệnh:

```
git add -A
git commit -m "mô tả ngắn việc vừa sửa"
git push
```

Chừng nửa phút sau bản công khai tự cập nhật, link không đổi. Không phải bấm gì trên GitHub nữa
— Pages đã bật sẵn, cứ có commit mới trên nhánh `main` là nó dựng lại.

Đẩy lên bằng SSH (khoá `~/.ssh/id_ed25519` đã nạp lên GitHub), nên `git push` chạy thẳng, không
hỏi tài khoản mật khẩu.

Lỡ sửa hỏng thì git giữ đủ lịch sử: `git log --oneline` xem các mốc, `git revert <mã>` để lùi
đúng một commit, hoặc `git checkout <mã> -- index.html` để lấy lại nguyên file ở mốc cũ.

## Cách chạy

Nhấp đúp vào `index.html`. Chọn công cụ bằng tab trên đầu trang; mở kèm `#ghep` là vào
thẳng tab ghép mảnh.
Nếu muốn chạy qua localhost cho chắc:

```
cd d:\PersonalProject\xuong-anh-3d
python -m http.server 8000
```

rồi mở http://localhost:8000

## Đưa lên mạng (GitHub Pages)

Thư mục này đã là một git repo có sẵn commit đầu tiên, chỉ còn thiếu chỗ để đẩy lên.

1. Tạo một repo rỗng trên GitHub, ví dụ `xuong-anh-3d` — **không** tích thêm README hay
   .gitignore, để nó trống hoàn toàn.
2. Chạy hai lệnh (thay `TEN-GITHUB` bằng tên tài khoản của bạn):

```
git remote add origin https://github.com/TEN-GITHUB/xuong-anh-3d.git
git push -u origin main
```

3. Vào repo trên GitHub → **Settings → Pages** → mục *Build and deployment*, chọn
   Source = **Deploy from a branch**, Branch = **main**, thư mục **/ (root)** → Save.
4. Chờ chừng một phút, link công khai sẽ là:
   `https://TEN-GITHUB.github.io/xuong-anh-3d/`

Sau này sửa gì thì `git add -A && git commit -m "..." && git push` là trang tự cập nhật.

Vài điều đi kèm:

- Thư mục `rieng-le/` bị `.gitignore` bỏ qua nên **không** lên mạng — trang công khai chỉ gồm
  đúng `index.html`.
- Có sẵn file rỗng `.nojekyll` để GitHub khỏi đem trang qua bộ dựng Jekyll.
- Bản chạy trên host tĩnh **tải video thẳng về máy**, không qua hộp thoại xin phép và
  không dính trần 16 MB như bản trong khung claude.ai.
- Trang đã có thẻ `description` và `og:title` / `og:description` để khi bạn dán link vào
  Facebook, Zalo hay Messenger thì hiện đúng tên và mô tả. Muốn có cả ảnh xem trước thì thêm
  `og:image` trỏ tới một ảnh với địa chỉ đầy đủ — phải có link rồi mới điền được.

## Các file

| File | Nội dung |
|---|---|
| `index.html` | **Toàn bộ xưởng** — hai công cụ trong một trang, chuyển bằng tab trên đầu |
| `rieng-le/thi-sai.html` | Bản rời của công cụ thị sai (không còn cập nhật) |
| `rieng-le/ghep-manh.html` | Bản rời của công cụ ghép mảnh (không còn cập nhật) |

Từ nay chỉ sửa `index.html`. Hai file trong `rieng-le/` là ảnh chụp lúc còn tách đôi, giữ lại
phòng khi cần một trang chỉ có đúng một công cụ; xoá cả thư mục cũng không mất gì.

**Hai công cụ chung một trang thế nào:** mỗi bên vẫn là một khối mã độc lập trong một IIFE
riêng, chỉ khác là mọi tra cứu phần tử được thu hẹp vào đúng khối DOM của nó
(`root.querySelector`), nên hai bên trùng tên id vẫn không giẫm chân nhau. Vòng vẽ và phím tắt
của tab đang ẩn thì nghỉ, không tốn máy. Đang quay video thì thanh tab khoá lại, đổi giữa
chừng là hỏng clip.

## 01 · Xưởng Thị Sai

Ước lượng bản đồ độ sâu từ **độ nét cục bộ** của ảnh: vùng nét được coi là ở gần, vùng
mờ ở xa. Shader WebGL dịch toạ độ lấy mẫu theo độ sâu (4 vòng lặp tinh chỉnh) nên vật ở
gần trôi nhanh hơn nền — mắt đọc ra khối 3D.

Chế độ: Thị sai · Thẻ nghiêng · Kính đỏ–lam (anaglyph) · Bản đồ sâu.

### Sóng

Bảng **Sóng** làm mặt ảnh gợn lên, chồng thêm lên hiệu ứng thị sai chứ không thay thế. Sóng
được tính ngay trong shader — chỉ là đẩy lệch toạ độ lấy mẫu — nên chạy mượt ở mọi độ phân
giải và không hề rách mép.

| Kiểu | Hình dung |
|---|---|
| **Sóng ngang** | Sóng chạy từ trái sang, kiểu lá cờ bay |
| **Gợn nước** | Vòng sóng lan từ tâm ra, yếu dần về rìa, như thả sỏi xuống nước |
| **Lụa bay** | Hai lớp sóng chéo nhau, mặt ảnh phập phồng như tấm lụa |

Chỉnh **Biên độ**, **Số lượn sóng** (1–8 lượn), **Tốc độ**; công tắc *Vật ở gần sóng mạnh hơn*
lấy bản đồ độ sâu ra nhân vào biên độ, nên chủ thể nét gợn mạnh còn hậu cảnh chỉ lay nhẹ —
sóng ăn khớp với chiều sâu thay vì trượt lên trên như một tấm nhựa.

**Sóng dày mà ảnh vẫn đọc được.** Hai thứ từng làm ảnh vỡ thành vệt kẻ khi kéo số lượn sóng
lên cao, đều đã xử lý:

- *Biên độ tự cân theo tần số* — `a = biên_độ / n^0.8`. Giữ nguyên biên độ mà tăng tần số thì
  độ dốc của phép đẩy toạ độ vượt 1 và mặt ảnh gấp lên chính nó — đúng chỗ sinh ra những vệt
  rách. Sóng thật cũng vậy: gợn càng dày thì càng nông.
- *Mipmap cho texture ảnh.* WebGL1 chỉ dựng được mipmap khi hai cạnh là luỹ thừa của 2, nên ảnh
  giờ được nạp lên ở kích thước luỹ thừa của 2 (tối đa 2048) rồi `generateMipmap`. Không có
  mipmap thì toạ độ lấy mẫu biến thiên nhanh sinh vân răng cưa — nhìn ra thành đúng những
  đường kẻ ngang mặt ảnh. Việc này còn làm ảnh thu nhỏ nét hơn ở mọi chế độ, không riêng sóng.

Biên độ lớn đẩy mép ảnh ra khỏi khung; phần lộ ra là nền, trông như ảnh trôi bồng bềnh. Không
thích thì kéo **Cắt viền** lên để giấu.

Khi quay ở chế độ **quét khép kín**, pha sóng được ép vào đúng một số nguyên chu kỳ trong độ
dài clip — nên video lặp lại vẫn không thấy mối nối, kể cả phần sóng.

### Đố hình

Bảng **Đố hình** cố tình làm ảnh khó nhận ra, rồi lộ dần — để quay clip đố người xem đoán.
Bốn kiểu che, đều là bóp méo toạ độ lấy mẫu ngay trong shader:

| Kiểu | Cách nó giấu ảnh |
|---|---|
| **Sóng xé** | Sóng dày, biên độ cố tình vượt ngưỡng gấp mặt ảnh — hình bị nhàu thành vân dệt |
| **Kẻ sọc** | Cắt ảnh thành hàng ngang, mỗi hàng trượt đi một quãng khác nhau |
| **Ô lưới xáo** | Chia ô rồi đẩy mỗi ô lệch một hướng ngẫu nhiên nhưng cố định |
| **Pixel hoá** | Gom về ô lớn thành khảm vuông |

**Độ che** quyết định giấu kín tới đâu, **Mật độ** quyết định vân to hay nhỏ.

Màn lộ: mức che luôn được nhân với `(1 − độ lộ)`, nên lộ tới 1 là ảnh gốc y nguyên, không sót
dấu vết. Nút **Che lại** / **Lộ dần**, cùng hai thanh **Giữ kín** và **Lộ trong** để định nhịp.
Bật **Tự lộ khi bắt đầu quay** thì chỉ cần bấm quay: ảnh tự che, giữ kín mấy giây cho người xem
đoán, rồi lộ dần ra — cả clip đố xong trong một lần bấm. **Thời lượng quay tự kéo dài cho đủ**
`giữ kín + lộ dần + 1.2 giây` thở cuối clip, kể cả khi thanh Thời lượng đang đặt ngắn hơn; con
số thật hiện ngay trên thanh đó (`6.0 giây → 8.2`) và trong dòng trạng thái lúc đang quay.

Che chồng được với sóng và thị sai: ảnh vừa bị giấu vừa gợn sóng, khó đoán hơn nhiều.

Mẹo với ảnh macro nền mờ: kéo **Nguồn sâu** về 0 (chỉ dùng độ nét), đặt **Mặt phẳng đứng yên**
vào khoảng 0.6–0.7 để chủ thể đứng im còn hậu cảnh trôi quanh nó.

Hạn chế: độ sâu là ước lượng, không phải đo đạc. Vật nét nằm ở tiền cảnh xa cũng bị coi là "gần".

## 02 · Xưởng Ghép Mảnh

Cắt ảnh theo lưới, mỗi cạnh trong lưới sinh một đường cong mộng jigsaw ngẫu nhiên.
Hai mảnh kề nhau **dùng chung đúng một mảng bezier** (mảnh bên kia đi ngược đường cong đó),
nên lắp xong ảnh khít lại không hở kẽ.

**Ráp xong là ảnh gốc, không còn vết cắt.** Ba thứ tố cáo rằng ảnh từng bị cắt — đường vát
cạnh, bề dày mảnh, và lớp sương mù theo chiều sâu — đều mờ dần theo `homeK` của từng mảnh và
tắt hẳn khi mảnh ngồi đúng ô. Riêng mạch nối giữa hai mảnh thì xử lý ở khâu cắt: sau khi cắt,
mỗi mảnh được **nong mép ra hơn một điểm ảnh bằng chính nội dung ảnh** (tô viền bằng
`createPattern` của đúng tấm ảnh đó), nên hai mảnh kề nhau chồng mép bằng những pixel giống
hệt nhau — không hở kẽ, không viền răng cưa, ở mọi góc nhìn.

Thanh **Đường cắt khi rời** chỉnh độ đậm của vát cạnh lúc mảnh còn bay; kéo về 0 thì mảnh
trông như dán decal, kéo lên cao thì ra chất bìa cứng.

Mỗi mảnh là một canvas riêng có vát cạnh sáng/tối và một mặt sau bìa các-tông.
Khi vẽ, các mảnh được đặt trong không gian (x, y, z), xoay theo góc nhìn của camera,
chiếu phối cảnh rồi sắp xếp theo chiều sâu. Độ nghiêng của mảnh mô phỏng bằng cách co
trục ngang/dọc theo `cos` của góc lật — lật quá 90° thì thấy mặt sau.

Ba trạng thái luân phiên: **Nguyên vẹn → Bung ra → Rời rạc (trôi tự do) → Lắp ráp**.

Điều khiển đáng chú ý:

- **Số cột** — số mảnh (tự tính số hàng theo tỉ lệ ảnh, giới hạn ~240 mảnh)
- **Thứ tự về chỗ** — sóng từ tâm / lần lượt / ngẫu nhiên / đồng loạt
- **Tự lặp vô hạn** — bung ra và lắp lại liên tục
- Rê chuột: xoay góc nhìn. Bấm vào khung hoặc phím `Space`: bung ra / lắp lại.

## Nền video

Cả hai công cụ có bảng **Nền** với bốn lựa chọn:

- **Màu giao diện** — đổi theo chế độ sáng/tối của máy.
- **Màu tự chọn** — hai ô chọn màu (trên / dưới) và công tắc chuyển sắc. Tắt chuyển sắc thì
  chỉ dùng màu trên. Đây là lựa chọn nên dùng khi quay video: màu cố định, không phụ thuộc
  máy ai đang mở.
- **Chính ảnh, làm mờ** — lấy đúng tấm ảnh đang dùng, phóng to và làm mờ làm nền. Đây là kiểu
  quen thuộc nhất khi đưa ảnh ngang lên khung dọc: giữa là ảnh nét, trên dưới là chính nó nhòe đi.
- **Ảnh nền riêng** — chọn một tấm khác từ máy.

Kèm hai thanh **Làm mờ** / **Làm tối** và công tắc cho nền **trôi ngược nhẹ theo góc nhìn**, tạo
thêm một lớp chiều sâu. Bản nền được nướng sẵn một lần (làm mờ mỗi khung hình thì quá nặng),
chỉ nướng lại khi bạn thả tay khỏi thanh trượt.

Khung hình bây giờ **áp dụng ngay lên khung đang xem**, không đợi tới lúc quay — chọn 9:16 là
thấy luôn bố cục dọc, chỉnh nền cho vừa mắt rồi mới bấm quay. Ghép Mảnh có thêm thanh **Cỡ cảnh**
để phóng to cảnh cho đầy khung dọc; Thị Sai có công tắc **Ảnh lấp đầy khung** nếu bạn muốn cắt
ảnh cho kín khung thay vì chừa lề.

## Xuất video

Cả hai công cụ đều có panel **Xuất video**: quay thẳng từ canvas bằng `canvas.captureStream()`
+ `MediaRecorder`, không dựng lại khung hình nên quay bao lâu cũng chỉ tốn đúng thời gian đó.

- Định dạng: tự dò trình duyệt hỗ trợ gì. Chrome/Edge bản mới cho **MP4 (H.264)**, còn lại **WebM**.
  MP4 thì TikTok, YouTube, Facebook, Instagram đều nuốt thẳng và mọi phần mềm dựng đều mở được;
  WebM thì YouTube nhận, còn lại nên chuyển sang MP4 trước khi đăng.
- **Khung hình**: 9:16 (TikTok, Reels, Shorts) · 16:9 (YouTube) · 1:1 (Facebook) · 4:5 (Instagram)
  · hoặc như khung đang xem. Cảnh được đặt vào giữa khung đã chọn, phần thừa lấp bằng đúng màu
  nền của sân khấu — không méo ảnh, không cắt cụt.
- Độ nét: 720p / 1080p / 1440p / như màn hình. Con số này là **cạnh chuẩn**: khung dọc thì nó là
  chiều ngang (1080 → 1080×1920), khung ngang thì là chiều cao (1080 → 1920×1080). Dòng chữ ngay
  trên nút quay luôn hiện kích thước thật sẽ xuất ra.
- Bitrate mặc định 14 Mbps là đủ cho 1080p; lên 1440p nên kéo 20–24 Mbps.
- Ghép Mảnh có chế độ **một vòng đầy đủ**: tự dẫn nguyên vẹn → bung ra → trôi → lắp lại rồi
  dừng, khỏi canh tay.
- Thị Sai có chế độ **quét khép kín**: góc nhìn đi hình số 8 rồi về đúng vị trí và vận tốc
  ban đầu, nên video lặp lại không thấy mối nối.

Quay xong file tự tải về. Bản chạy trên claude.ai xin phép qua hộp thoại lưu của trình xem,
mỗi file tối đa 16 MB; bản mở từ thư mục này thì tải thẳng, không giới hạn. Cách nào cũng hỏng
thì video vẫn nằm trong panel — chuột phải lên nó để lưu tay.

## Trên điện thoại

Dùng được, cả link artifact lẫn file mở thẳng. Bố cục tự xếp thành một cột: khung hình trên,
các bảng điều khiển xếp dưới.

Ba thứ đã chỉnh riêng cho màn hình nhỏ:

- `<meta name="viewport">` — thiếu thẻ này thì điện thoại dựng trang ở 980px rồi thu nhỏ lại,
  chữ li ti.
- `touch-action: pan-y` trên khung vẽ — trước đó để `none`, vuốt dọc trên khung không cuộn được
  trang, người dùng kẹt luôn ở đầu trang. Giờ vuốt dọc thì cuộn, kéo ngang thì đổi góc nhìn.
- Dưới 640px: khung vẽ thấp lại (46% chiều cao màn), lề mỏng đi, nút bấm cao hơn cho vừa ngón tay.

Cách dùng khác một chút: **kéo ngang** trên ảnh để đổi góc nhìn (chạm rồi thả không có
"rê chuột"), **chạm** vào khung để bung/lắp mảnh. Nạp ảnh bằng nút *Chọn ảnh* (mở thư viện hoặc
máy ảnh); kéo thả và `Ctrl+V` thì không có trên điện thoại.

Quay video: Android Chrome chạy tốt. iPhone thì tuỳ đời iOS — trang tự dò, nếu máy không quay
được canvas thì nút quay tự mờ đi kèm lời nhắc, chứ không im lặng hỏng. Máy điện thoại nên để
720p hoặc 1080p và ít mảnh ghép; 1440p với vài trăm mảnh là quá sức.

## Ghi chú kỹ thuật

- Ảnh lớn hơn 1600px được thu nhỏ trước khi cắt, cho nhẹ bộ nhớ.
- Font lấy từ Google Fonts; chạy offline vẫn được, chỉ là rơi về font hệ thống.
- Cả hai trang đều theo sáng/tối của hệ điều hành — và **màu nền video lấy theo màu nền đang
  hiển thị**, nên muốn video nền tối thì để hệ điều hành ở chế độ tối trước khi quay.
- Mở kèm `#demo` (hoặc `#ghep-demo`) là trang tự nạp ảnh mẫu, tiện gửi link khoe.
- Trình duyệt chặn trang tự tải file, nên muốn giữ một khung hình thì chuột phải lên
  ảnh rồi lưu.
