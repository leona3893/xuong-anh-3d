# Xưởng Ảnh 3D

Bộ công cụ tĩnh chạy trong trình duyệt: biến ảnh phẳng thành hiệu ứng 3D, và ghép bộ ảnh
infographic với một file giọng đọc thành video có tiếng, có phụ đề.
Không cần cài đặt, không cần server, ảnh và tiếng không rời khỏi máy — mở file HTML là dùng.

Một ngoại lệ về mạng: công cụ thuyết minh phải **tải mô hình về** lần đầu (37–237 MB, từ jsDelivr
và Hugging Face) để nghe lời đọc hoặc để tự đọc hộ. Chiều tải là một chiều — mô hình đi vào máy,
tiếng của bạn không đi ra. Hai công cụ còn lại vẫn chạy được khi rút mạng.

**Bản công khai:** https://leona3893.github.io/xuong-anh-3d/
**Mã nguồn:** https://github.com/leona3893/xuong-anh-3d

## Muốn sửa hay thêm tính năng thì làm gì

Bản thân xưởng nằm trọn trong **`xuong/index.html`** — sửa hiệu ứng, thêm tính năng là vào file
đó. Còn **`index.html`** ngoài cùng là trang giới thiệu, chỉ chữ nghĩa và ảnh chụp. Sửa xong thì
ba lệnh:

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

## Giao diện

Mặc định là **nền sáng**: kem ấm, các bảng điều khiển trắng, bo góc rộng, thanh trượt và công tắc
to cho dễ kéo. Nút **Nền sáng / Nền tối** ở đầu trang đổi qua lại, lựa chọn được nhớ trong
`localStorage` (khoá `xuong-giao-dien`). Trước đây trang chạy theo chế độ của hệ điều hành nên
máy để tối là xưởng tối om, không đổi được.

Hai điều cần nhớ khi sửa giao diện:

- Lớp kiểu mới nằm cuối khối `<style>` dưới tiêu đề *lớp phủ*, đè lên các quy tắc gốc phía trên
  mà không phải sửa từng chỗ. Muốn đổi hình khối thì sửa ở đó.
- Màu `--stage` vừa là nền sân khấu vừa là **nền mặc định của video xuất ra** khi chọn
  *Màu giao diện*. Đổi nó là đổi luôn màu nền clip.

Có khai báo `color-scheme` cho cả hai nền, và sau khi bấm đổi nền thì ép trình duyệt tính lại
kiểu một nhịp — thiếu nhịp đó, vài phiên bản Chrome không cập nhật màu chữ thừa kế của thẻ
`<button>`, chữ trên nút chìm hẳn vào nền tối.

## Bố cục cho đỡ phải cuộn

Mỗi tab có 6–10 bảng điều khiển, xếp dọc một cột thì phải cuộn cả nghìn pixel và cuộn tới đâu
mất hình tới đó. Bốn cách xử lý, đều chỉ áp dụng từ 941px trở lên, còn điện thoại giữ nguyên
kiểu xếp dọc:

- **Khung ảnh dính lại** khi cuộn, nên hình luôn nằm trong tầm mắt.
- **Cột điều khiển cuộn riêng** trong đúng chiều cao màn hình — trang gần như không cuộn nữa
  (cao 1155px thay vì chạy theo cột điều khiển, có tab từng dài hơn 4200px).
- **Từ 1500px trở lên cột điều khiển tách làm hai**, chiều cao giảm khoảng một phần ba.
- **Bấm tiêu đề để gập bảng lại**, nhớ lựa chọn cho lần sau trong  (khoá
  ). Nút *Thu gọn tất cả* ở góc trên gập hết một lượt: cột từ 2769px xuống 623px,
  tức là toàn bộ tiêu đề nằm gọn trong một màn hình, mở đúng bảng nào cần dùng.

## Cách chạy

Nhấp đúp vào `index.html` để xem trang giới thiệu, hoặc mở thẳng `xuong/index.html` để vào
xưởng. Chọn công cụ bằng tab trên đầu trang; mở kèm `#ghep` hay `#noi` là vào thẳng tab ghép
mảnh hoặc tab thuyết minh.
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
| `index.html` | Trang giới thiệu: mở đầu, bốn hiệu ứng, công cụ thuyết minh, ba bước, bảng khung hình |
| `xuong/index.html` | **Toàn bộ xưởng** — ba công cụ trong một trang, chuyển bằng tab trên đầu |
| `anh/` | Ảnh chụp dùng cho trang giới thiệu và cho thẻ xem trước khi dán link |
| `favicon.svg` | Biểu tượng tab: ba lớp ảnh xếp so le thành chiều sâu |
| `rieng-le/` | Bản rời của hai công cụ đầu hồi còn tách đôi (không còn cập nhật, không lên mạng) |

Ảnh trong `anh/` đều chụp thẳng từ chính công cụ bằng trình duyệt ẩn, không phải dựng tay —
muốn làm mới thì mở xưởng, chỉnh cho ưng rồi chụp lại.

**Ba công cụ chung một trang thế nào:** mỗi bên vẫn là một khối mã độc lập trong một IIFE
riêng, chỉ khác là mọi tra cứu phần tử được thu hẹp vào đúng khối DOM của nó
(`root.querySelector`), nên các bên trùng tên id vẫn không giẫm chân nhau. Chỗ này phải làm
triệt để: cả những tra cứu có bộ chọn nằm trong biến (các nhóm nút chọn) cũng phải đi qua
`root` — sót một chỗ là tab này bấm nút lại đổi cài đặt của tab kia. Tab thuyết minh đi thêm một nước
nữa cho chắc: mọi id của nó đều mang tiền tố `n`. Vòng vẽ và phím tắt của tab đang ẩn thì nghỉ,
không tốn máy. Đang quay video thì thanh tab khoá lại, đổi giữa chừng là hỏng clip.

Trang này **không có `<meta charset>`** cho tới gần đây, nên khi mở qua một máy chủ không gửi kèm
`charset=utf-8` (chẳng hạn `python -m http.server`) thì trình duyệt đọc UTF-8 thành Windows-1252 và
toàn bộ chữ Việt vỡ thành `Ná»n & khung`. Thẻ đã được thêm vào dòng đầu file — đừng bỏ đi.

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

## 03 · Xưởng Thuyết Minh

Nhận **nhiều ảnh** (mỗi tấm là một cảnh, xếp theo tên file) cộng **một giọng đọc**, rồi dựng ra một
video có tiếng và có phụ đề. Đây là công cụ duy nhất trong xưởng cần mạng ở lần chạy đầu.

### Nguồn hình: bộ ảnh, hay một video có sẵn

Bảng **Nguồn hình** nhận thêm một **video không lời** — thứ bạn đã dựng ở chỗ khác và chỉ còn thiếu
tiếng. Nạp vào rồi chọn một trong hai cách dùng:

- **Video có sẵn** — giữ nguyên đoạn phim, kể cả chuyển động, chỉ đặt giọng đọc lên trên. Thanh
  *Giọng đọc vào sau* dịch tiếng để khớp với hình. Độ dài clip lấy theo cái nào kết thúc muộn hơn:
  phim ngắn hơn tiếng thì khung cuối đứng lại chờ, tiếng ngắn hơn phim thì đoạn cuối im.
  Ở chế độ này **đồng hồ chạy riêng** (`scrub` nuôi bằng `requestAnimationFrame`), rồi hai thẻ
  `<video>` và `<audio>` bám theo nó; lệch quá 0,3 giây mới nắn, nên không giật.
- **Cắt thành bộ ảnh** — dò chỗ phim đổi cảnh, lấy mỗi cảnh một khung hình rồi đổ thẳng vào bộ ảnh.
  Mất chuyển động, đổi lại được nguyên bộ máy khớp lời: từ khoá, kéo mốc, phụ đề, chuyển cảnh.
  Với video infographic — vốn gần như ảnh tĩnh nối nhau — đây mới là đường cho ra clip khớp từng câu.

**Ngưỡng cắt cảnh phải tự cân theo chính video đó.** Lần đầu tôi đặt một ngưỡng cứng cho độ lệch
trung bình giữa hai khung hình, và nó hụt sạch: ảnh infographic dùng chung một nền kem thì hai cảnh
khác hẳn nhau vẫn chỉ lệch vài phần trăm. Giờ dò hết một lượt, lấy **cú đổi cảnh mạnh nhất trong
video làm mốc**, rồi cắt ở những chỗ vượt một phần của mốc đó (thanh *Độ nhạy* chỉnh phần ấy từ
0,69 xuống 0,15 lần). Có thêm luật hai cảnh phải cách nhau quá 0,8 giây, để hoạt hình trong cảnh
không bị đếm thành cảnh mới.

Giọng đọc lấy từ đâu cũng được, và hai đường đi ngược chiều nhau:

| Đường | Có sẵn gì | Mốc đổi ảnh tính thế nào |
|---|---|---|
| **Có tiếng trước** — nạp file hoặc thu mic | sóng âm | Whisper nghe ra chữ *và* mốc thời gian (mục *Nghe hiểu lời*) |
| **Có chữ trước** — dán lời cho máy đọc | bản chữ | biết trước từng câu dài bao nhiêu giây, **khỏi cần Whisper** (mục *Máy đọc hộ*) |

Đường thứ hai chính xác hơn hẳn: không phải đoán mốc, không phải nắn gì cả.

Ô nạp tiếng nhận mọi thứ trình duyệt giải mã được — mp3, m4a, wav, ogg, opus, flac — **và cả file
video** (mp4, mov, webm): kéo vào thì `decodeAudioData` rút lấy track tiếng, phần hình bỏ qua. Chỗ
này từng hỏng vì thẻ `accept` để đúng `audio/*`: file `.mp4` mang nhãn `video/mp4` nên hộp thoại
chọn file làm mờ nó đi, còn `.m4a` thì có máy báo `audio/mp4`, có máy để trống hẳn. Bài học: **đừng
lọc file theo nhãn MIME.** Giờ thứ gì không phải ảnh đều được đưa xuống giải mã, hỏng thì báo hỏng
kèm cách chữa, chứ không chặn từ cửa.

### Ba thứ để trả lời "không nghe thấy gì"

Câu hỏi khó nhất khi gỡ rối âm thanh là *lỗi ở trang hay ở máy người dùng*, và ngồi đoán thì đoán
cả buổi. Nên bảng **Giọng đọc** có sẵn ba thứ trả lời hộ:

- **Trình phát của chính trình duyệt.** Một thẻ `<audio controls>` trỏ vào đúng file vừa nạp, không
  đi qua một dòng mã nào của tôi. Bấm phát ở đó mà im luôn thì lỗi nằm ở file hoặc ở máy, khỏi ngờ
  cho xưởng. Nó là một thẻ riêng, không phải cái thẻ đã bị `createMediaElementSource` bắt mất.
- **Đo biên độ tuyệt đối.** Sóng âm vẽ ra đã chuẩn hoá theo đỉnh, nên một file im phăng phắc trông
  vẫn có sóng như thường — nhìn hình không thể biết. Giờ đo thẳng biên độ; dưới 0,005 thì báo
  *"File này gần như không có tiếng"* kèm con số đo được.
- **Vạch mức tiếng** cạnh nút *Nghe thử*, lấy từ `AnalyserNode` cắm vào đúng nhánh chạy ra loa.
  Vạch nhảy mà tai không nghe gì thì lỗi ở loa, ở tab bị tắt tiếng, hoặc ở bộ trộn âm của hệ điều
  hành — chứ tiếng đã ra khỏi trang rồi.

Nhân tiện ghi lại cách đo đường ra loa từ bên ngoài, vì mã nằm trong IIFE nên không với tới biến
được: vá `AudioContext.prototype.createMediaElementSource` bằng `addInitScript` **trước khi trang
chạy** để giữ lại nút nguồn, rồi cắm `AnalyserNode` vào đó mà đọc biên độ. Đo được đỉnh 0,0965 với
một file lành, cùng `ctx.state === "running"` và ba mối nối đúng như mong đợi.

### Ba tầng nạp tiếng

`decodeAudioData` là đường nhanh nhất nhưng kén: mp4 phân mảnh, hộp `moov` nằm cuối file, hay codec
lạ nhét trong hộp mp4 đều làm nó từ chối, dù thẻ `<audio>` phát tuốt. Nên có ba tầng:

1. **Giải mã thẳng.** Ra ngay sóng âm, chỗ ngắt hơi, bóc chữ được. Đường thường ngày.
2. **Phát được nhưng không giải mã được.** Lấy độ dài từ chính thẻ `<audio>`, ảnh chia đều, quay
   video vẫn có tiếng — chỉ thiếu sóng âm và chưa bóc chữ được. Hiện thêm nút **Dựng sóng âm**.
3. **Dựng sóng âm** — cho file chạy qua đồ thị Web Audio một lượt và hứng mẫu bằng
   `ScriptProcessor`. `playbackRate = 2` cùng `preservesPitch = false` nghĩa là tín hiệu chỉ bị đổi
   tỉ lệ thời gian chứ không méo, nên quét xong vẫn bóc chữ được, mà chỉ mất nửa thời lượng.
   **Tốc độ ×2 chứ không phải ×3**, và đây là con số đo chứ không phải chọn bừa: ở ×3 tiếng gốc bị
   đẩy lên sát tần số Nyquist nên bộ lọc chống răng cưa cắt mất dải cao (số lần đổi dấu của tín hiệu
   tụt 45%), tỉ lệ lỗi từ nhảy từ 13% lên 20%. Ở ×2 thì bóc chữ **đúng y hệt bản giải mã thẳng**.
   Tần số lấy mẫu thật được suy ngược từ **số mẫu hứng được chia cho `el.duration`** chứ không phải
   từ `ac.sampleRate / 3` — vài khung hình rơi rụng lúc máy bận cũng không làm lệch mốc thời gian.

Khi cả ba tầng đều hỏng thì lời báo lỗi ghi đủ **tên lỗi thật, cỡ file và nhãn MIME**, vì không nhìn
thấy ba con số đó thì đoán mò cả buổi. File 0 MB hay gặp nhất: file còn nằm trên OneDrive hoặc
Google Drive, chưa tải thật về máy.

**Một cái bẫy đã sập một lần:** đường giải mã cũ chỉ gọi `ac.close()` khi thành công. Mỗi lần nạp
hỏng là bỏ lại một `AudioContext` sống. Chrome chỉ cho mỗi trang **sáu cái**, nên nạp hỏng vài lần
là đến lượt thứ bảy `new AudioContext()` ném lỗi — và mọi file sau đó đều hỏng, kể cả file lành.
Triệu chứng nhìn y hệt "tool không đọc được định dạng này". Giờ đóng ở cả hai nhánh.

### Máy đọc hộ

Dùng `Xenova/mms-tts-vie` — bản MMS-TTS tiếng Việt của Meta, kiến trúc VITS, chạy qua chính luồng
phụ đã dựng sẵn cho Whisper. Mỗi câu được đọc riêng thành một đoạn sóng, rồi nối lại kèm khoảng
lặng ở giữa; vì thế **mốc bắt đầu của từng câu là con số đo được, không phải ước lượng**.

Ba điều đo được khi làm, đều ngược với trực giác nên ghi lại kẻo lần sau lại chọn nhầm:

- **Bản không nén chạy nhanh gấp 6 lần bản nén.** `fp32` (109 MB) dựng tiếng ở tốc độ ~1,0 lần thời
  gian thật; `q8` (37 MB) mất ~6,1 lần. Nén xong thì mấy phép tính của VITS rơi vào đường chậm của
  onnxruntime. Nên mặc định là bản nặng; bản nhẹ để dành cho ai mạng yếu.
- **WebGPU không chạy được mô hình này.** Nhân `GatherND` trong bộ đoán độ dài âm dùng kiểu số mà
  backend WebGPU chưa nhận (`Unsupported data type: 7`). Đừng phí công bật lên, nó ném lỗi ngay.
  Bản `fp16` cũng hỏng, vì `RandomNormalLike` trả về float32 không khớp.
- **Bảng chữ cái của mô hình chỉ có 95 ký tự** — chữ cái tiếng Việt, dấu nháy, gạch nối. **Không có
  chữ số, không có dấu chấm phẩy.** Bộ tách từ vứt lặng lẽ mọi thứ ngoài bảng đó, nên viết `68%` là
  máy đọc thành khoảng trống. Vì vậy phần đổi số ra chữ (`68%` → *sáu mươi tám phần trăm*, `9:16` →
  *chín trên mười sáu*, `1.250.000` → *một triệu hai trăm năm mươi nghìn*) là **bắt buộc**, không
  phải tuỳ chọn cho vui.

Một hệ quả dễ chịu của chuyện đó: **lời đưa cho máy đọc và lời hiện lên phụ đề là hai bản khác
nhau.** Máy nghe *sáu mươi tám phần trăm*, người xem đọc `68%`.

Còn thứ mô hình đọc sai mà không cách nào sửa được: **chữ nước ngoài**. Nó đọc theo luật chính tả
tiếng Việt, nên *server*, *video*, *YouTube* ra thành âm lạ. Trang tự soi trước và mách ra bằng một
bộ kiểm âm tiết tiếng Việt — phụ âm đầu trong bộ đóng, nguyên âm, phụ âm cuối chỉ tám cái
(`c ch m n ng nh p t`) — chữ nào không lọt qua khuôn đó thì báo *Nuốt mất: server, video*. Cách
chữa là viết theo âm: *sơ-vơ*, *vi-đê-ô*, *phây-búc*.

Ô **Máy sẽ đọc là:** ngay dưới khung nhập luôn hiện đúng chuỗi chữ sắp đưa vào mô hình, nên không
phải nghe xong mới biết hỏng chỗ nào.

### Whisper nghe lời đọc thế nào

Whisper chạy thẳng trong trình duyệt qua [transformers.js](https://github.com/huggingface/transformers.js)
3.8.1 + onnxruntime-web, nạp từ jsDelivr bằng một `<script type="module">` dựng trong Web Worker
(mã worker nằm ngay trong file, đóng thành Blob rồi `new Worker(url, {type:"module"})`). Không dựng
được worker thì tự lùi về chạy trên luồng chính — giật, nhưng vẫn ra kết quả.

**Chọn mô hình là chỗ quyết định tất cả.** Đo trên cùng một đoạn tiếng Việt, cùng cách nạp:

| Mô hình | Nặng | Tỉ lệ lỗi từ | Lâu |
|---|---|---|---|
| `onnx-community/PhoWhisper-base-ONNX` | ~174 MB | **13%** | 21 s |
| `onnx-community/whisper-small` | ~237 MB | 39% | 48 s |
| `onnx-community/whisper-base` | ~73 MB | 48% | 19 s |

Bản tinh chỉnh riêng cho tiếng Việt **đúng gấp gần bốn lần** bản đa ngữ nhỏ, mà lại nhẹ hơn và
nhanh gấp đôi bản đa ngữ lớn. Ban đầu tôi để `whisper-base` làm mặc định vì nó nhẹ nhất — sai lầm
đó khiến khâu bóc chữ **sai gần một nửa số từ**, nhìn ra như thể chức năng hỏng. Giờ mặc định là
PhoWhisper, và chọn *Tiếng Anh* thì tự nhảy sang bản đa ngữ (PhoWhisper chỉ biết tiếng Việt).

Số đo trên lấy từ giọng máy đọc, không phải giọng người thật, nên con số tuyệt đối sẽ khác; nhưng
thứ hạng giữa ba mô hình thì không đổi.

**Mốc thời gian phải tự dựng lấy.** Đổi lại chữ đúng, PhoWhisper cho mốc rất thô — có khi trả về
đúng *một* đoạn ôm trọn cả file, và gần như không chấm câu (bốn chục chữ liền một mạch, một dấu
chấm cuối). Cả hai cách chia hiển nhiên đều hụt: chia theo dấu câu ra hai mảnh lê thê, mà tin mốc
của mô hình thì mốc đổi ảnh vô nghĩa. Nên `xeTheoNgatHoi()` lấy **quãng lặng đo được từ sóng âm**
làm ranh giới — đó là bằng chứng vật lý về chỗ người ta thật sự ngừng, còn dấu chấm chỉ là phỏng
đoán của mô hình — rồi rải chữ vào từng mảnh theo tỉ lệ thời gian, mỗi mảnh chừng 11 chữ cho vừa
một dòng phụ đề. Chỉ khi dấu chấm rải đều thật (số câu không dưới 0,7 lần số mảnh mong muốn) mới
tin theo dấu câu.

Whisper và mô hình đọc **dùng chung một luồng phụ**, nạp transformers.js một lần cho cả hai. Chỗ
gộp này từng làm hỏng một việc: `worker.onerror` viết thành `function(){ worker = null; }`, nuốt
sạch lỗi. Luồng phụ chết — nạp mô-đun hỏng, hết bộ nhớ, bị chặn — thì **không báo gì, không lùi về
đường dự phòng, và cờ `hearing` kẹt ở `true` vĩnh viễn**, nên nút đứng nguyên ở "Đang nghe…" và bấm
tiếp không ăn thua. Nhìn từ ngoài y hệt "chức năng này không chạy". Giờ `onerror` kêu lên, dọn cờ,
và chạy lại việc đang dở trên luồng chính.

Kèm theo đó là hai thứ lẽ ra phải có từ đầu, vì bóc chữ mất khoảng **gấp rưỡi thời lượng** và suốt
lúc ấy màn hình không nhúc nhích:

- **Đồng hồ đếm** ngay trong dòng trạng thái (`đã chạy 1:05`), cùng số đoạn 30 giây đã xong. Không
  có nó thì không phân biệt nổi "đang chạy" với "chết rồi".
- **Bấm lại nút là dừng.** Trước đây `if(hearing) return;` làm nút thành cục gạch.

Mô hình tải một lần rồi nằm trong Cache API của trình duyệt, lần sau bấm là chạy ngay. (Whisper
chỉ cần khi bạn mang tiếng từ ngoài vào — máy tự đọc thì bỏ qua hẳn mục này.) Mặc định
chạy bằng WebAssembly một luồng — GitHub Pages không đặt được hai thẻ `COOP`/`COEP` nên không có
`SharedArrayBuffer`, không có wasm đa luồng. Máy nào có WebGPU thì bật công tắc **Tăng tốc bằng
WebGPU**, nhanh hơn nhiều lần (khi đó dùng bản `fp16`/`q4` thay cho `q8`).

**Mở thẳng file từ thư mục thì khâu này hỏng.** Trang `file://` có origin `null`, trình duyệt chặn
nạp mô-đun và dựng worker. Muốn bóc chữ thì chạy qua `python -m http.server` rồi vào localhost,
hoặc dùng bản công khai. Trang có bắt lỗi này và nói đúng câu đó, chứ không im lặng hỏng.

### Ảnh được canh vào lời ra sao

Bản bóc chữ trả về từng câu kèm mốc thời gian. Ba đường sinh mốc đổi ảnh, chồng lên nhau:

- **Chia theo câu.** Số câu chia đều cho số ảnh — bản nháp tự động, có ngay sau khi bóc chữ xong.
- **Từ khoá.** Gõ một cụm có trong lời đọc vào ô dưới mỗi tấm ảnh, ảnh nhảy tới đúng câu chứa cụm
  đó và những ảnh không có từ khoá tự chia đều trong quãng còn lại. So chữ bỏ dấu, nên `dau tu`
  khớp với *Đầu Tư*.
- **Kéo tay.** Kéo mốc màu cam trên thanh thời gian.

Mốc nào cũng được **nắn về khoảng lặng gần nhất** trong phạm vi 1,1 giây (tắt được). Khoảng lặng
tìm bằng năng lượng RMS từng khung 20 ms: chuỗi khung dưới ngưỡng kéo dài quá 0,28 giây thì tính là
một chỗ ngắt hơi. Whisper cho mốc theo câu nhưng hay lệch vài phần mười giây; nắn vào chỗ ngắt hơi
thì ảnh đổi đúng lúc người đọc ngừng lấy hơi thay vì cắt ngang một chữ. Sau cùng có một lượt ép
**mỗi ảnh giữ ít nhất** mấy giây, để một câu ngắn không làm ảnh loé qua rồi biến mất.

Thanh **Nắn mốc** và **Mỗi ảnh giữ ít nhất** đều nằm ở bảng *Cách khớp ảnh*.

### Hình và chữ trên khung

- **Chuyển cảnh**: mờ dần · trượt ngang · đẩy tới · cắt thẳng.
- **Ảnh trôi và phóng nhẹ.** Ảnh chỉ lớn dần *về* mức vừa khung chứ không bao giờ vượt quá — với
  infographic thì cắt mất mép là mất chữ, nên hiệu ứng đi hướng ngược với kiểu Ken Burns thường
  thấy. Cảnh chẵn phóng vào, cảnh lẻ lùi ra, cho đỡ đều đều.
- **Phụ đề** lấy thẳng từ bản bóc chữ và **sửa được ngay trên trang** — mỗi câu là một ô
  `contenteditable`, sửa xong khung hình đổi theo liền. Chọn trên hay dưới khung, cỡ chữ tính theo
  phần nghìn chiều cao khung (nên đổi độ nét không làm chữ to nhỏ theo), có nền mờ sau chữ hoặc
  viền tối quanh chữ.
- **Nền** làm bằng cách thu ảnh xuống thật nhỏ rồi phóng to lại, chứ không dùng `filter: blur()` —
  rẻ hơn nhiều khi phải vẽ lại 30 lần mỗi giây.

### Xuất

Khác hai công cụ kia ở đúng một chỗ: luồng quay có thêm **track tiếng**. Thẻ `<audio>` đi qua
`createMediaElementSource` → `MediaStreamDestination`, rồi track tiếng đó ghép với track hình của
`canvas.captureStream()` thành một `MediaStream` đưa cho `MediaRecorder`. Định dạng vì thế phải khai
cả hai codec: `video/mp4;codecs=avc1.42E01E,mp4a.40.2` hoặc `video/webm;codecs=vp9,opus`.

Thời lượng clip chính là thời lượng giọng đọc, và **quay chạy thời gian thật** — voice hai phút thì
ngồi chờ hai phút. Quay xong tự dừng. Đừng chuyển sang tab khác của trình duyệt trong lúc quay:
`requestAnimationFrame` bị bóp lại thì khung hình rơi rụng.

Chưa có tiếng vẫn xem thử và quay được — khi đó mỗi ảnh giữ 4 giây và đồng hồ chạy bằng
`requestAnimationFrame`.

Tiếng do máy đọc được đóng thành WAV 16-bit đơn kênh 16 kHz ngay trong trang rồi đi vào đúng đường
mà một file tải lên sẽ đi: giải mã, dựng sóng, dò khoảng lặng, chia mốc. Nhờ vậy không có nhánh mã
riêng nào cho tiếng máy — thanh thời gian, phụ đề, quay video đều không cần biết tiếng ở đâu ra.

## Nền video

Hai công cụ đầu có bảng **Nền** với bốn lựa chọn (tab thuyết minh chỉ giữ hai kiểu đầu bảng —
*chính ảnh làm mờ* và *màu tự chọn* — vì đó là hai kiểu duy nhất hợp với ảnh infographic):

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

Cả ba công cụ đều có panel **Xuất video**: quay thẳng từ canvas bằng `canvas.captureStream()`
+ `MediaRecorder`, không dựng lại khung hình nên quay bao lâu cũng chỉ tốn đúng thời gian đó.
Riêng tab thuyết minh ghép thêm track tiếng vào luồng — xem mục *03* ở trên.

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

**Tab thuyết minh thì nên làm trên máy tính.** Bóc chữ bằng WebAssembly một luồng đã chậm sẵn trên
máy bàn; trên điện thoại thì mô hình *Kỹ* (237 MB) gần như chắc chắn hết bộ nhớ. Nếu buộc phải làm
trên máy, chọn mô hình *Nhanh* và giọng đọc dưới một phút. Phần còn lại — nạp ảnh, kéo mốc, quay —
vẫn chạy bình thường.

## Ghi chú kỹ thuật

- Ảnh lớn hơn 1600px được thu nhỏ trước khi cắt, cho nhẹ bộ nhớ.
- Font lấy từ Google Fonts; chạy offline vẫn được, chỉ là rơi về font hệ thống.
- Cả hai trang đều theo sáng/tối của hệ điều hành — và ở hai công cụ đầu, **màu nền video lấy
  theo màu nền đang hiển thị**, nên muốn video nền tối thì để hệ điều hành ở chế độ tối trước khi
  quay. Tab thuyết minh không dính chuyện này: nền của nó luôn là màu bạn chọn hoặc chính tấm ảnh.
- Mở kèm `#demo` (hoặc `#ghep-demo`) là trang tự nạp ảnh mẫu, tiện gửi link khoe.
- Trình duyệt chặn trang tự tải file, nên muốn giữ một khung hình thì chuột phải lên
  ảnh rồi lưu.
