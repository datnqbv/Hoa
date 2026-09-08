Đóng vai chuyên gia Frontend (HTML5/Canvas/JS) và Thiết kế học liệu điện tử tương tác. Viết một file HTML duy nhất chứa toàn bộ CSS và JS theo kịch bản bên dưới, tuân thủ nghiêm ngặt design system "Haugomat editorial flat" của Aiducation LMS.

________________________________________
TRIẾT LÝ THIẾT KẾ (đọc trước khi viết code)

Phong cách Tom Haugomat — flat editorial: **phẳng, tiết chế, sang**. Không gradient màu mè trong UI, không shadow nặng, không bo góc quá tròn kiểu "app trẻ con". Ưu tiên khoảng trắng rộng, bố cục như một trang tạp chí biên tập: có nhịp, có điểm nghỉ mắt, phân cấp rõ ràng bằng khoảng trắng + cỡ chữ (không bằng nhiều box màu). Mục tiêu cảm giác: bình tĩnh, đáng tin, gọn gàng — đây là công cụ học tập cho học sinh THPT, sạch sẽ và dễ đọc quan trọng hơn hoa mỹ.

________________________________________
TYPOGRAPHY & FONT

•	Toàn bộ chữ UI (heading, body, label, nút...) dùng MỘT font duy nhất: **Be Vietnam Pro** — import từ Google Fonts, BẮT BUỘC kèm subset `vietnamese` (weights: 400, 600, 700, 800)
•	Font stack: `'Be Vietnam Pro', system-ui, sans-serif`
•	**Ngoại lệ — số liệu động & nhãn trên canvas dùng JetBrains Mono, KHÔNG dùng Be Vietnam Pro:** tên hóa chất/nhãn/số liệu vẽ bằng `ctx.fillText` (trong `drawTestTube`, `drawDropper`, mọi hàm vẽ dụng cụ khác...) dùng font phụ **JetBrains Mono** — monospace nên không "giật" độ rộng khi số liệu đổi mỗi frame, hỗ trợ tiếng Việt đầy đủ, tải qua Google Fonts nên hiển thị đồng nhất mọi thiết bị (khác `Courier New`/`Playfair Display` là font không đảm bảo có sẵn/đồng nhất). Import cùng lúc với Be Vietnam Pro:
```css
@import url('https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@500;700&family=Be+Vietnam+Pro:ital,wght@0,400;0,500;0,600;0,700;0,800;1,400&display=swap');
```
Trong canvas: `ctx.font = "bold 12px 'JetBrains Mono', monospace"` cho MỌI nhãn/số liệu — thay hẳn cho `"Playfair Display", serif` (font đó không được phép dùng nữa, chưa từng được import ở đâu trong file).
•	Phân cấp bằng size + weight, KHÔNG đổi font (trừ ngoại lệ canvas ở trên):
    o	Heading / số lớn: font-weight: 700–800
    o	Nhấn mạnh / label: font-weight: 600
    o	Body: font-weight: 400, line-height: 1.7, độ rộng dòng tối đa ~64ch
•	Body text mặc định **17px** (Be Vietnam Pro nét mảnh, tiếng Việt nhiều dấu dễ bể khi chữ nhỏ). Không bao giờ dùng weight dưới 400.
•	Label nhỏ/badge: font-size tối thiểu **12px**, font-weight: 600, text-transform: uppercase, letter-spacing: 0.6px
•	Type scale (responsive): display `clamp(2.25rem,5vw,3.5rem)` · h1 `clamp(1.9rem,4vw,2.75rem)` · h2 `clamp(1.5rem,3vw,2rem)` · h3 `1.18rem` · body `17px` · small `0.86rem`. Với simulation dạng cột đơn: text thanh Athena khoảng `0.95rem`, button khoảng `0.82rem`, nội dung bảng khoảng `0.86rem` để giao diện gọn nhưng vẫn dễ đọc.
•	Trích dẫn có thể dùng italic

________________________________________
ICON

•	Nhúng Tabler Icons webfont: https://cdn.jsdelivr.net/npm/@tabler/icons-webfont/tabler-icons.min.css (dạng line icon nét đều — đúng tinh thần design system)
•	Dùng class `<i class="ti ti-[tên-icon]">` thay cho emoji ở tất cả button, label, badge
•	Màu icon: `--ink` hoặc `--jade` (icon trong nút jade thì màu `--cream`)
•	Không dùng emoji làm icon ở bất kỳ chỗ nào; không clipart

________________________________________
MÀU SẮC & DESIGN SYSTEM

Khai báo đúng các CSS variables sau và CHỈ dùng màu trong bảng này cho UI — không tự chế màu lạ:

```css
:root {
    --cream:      #FAF7F0;  /* nền trang chính */
    --cream-2:    #F0EADD;  /* nền phụ / card / section */
    --paper-line: #E5DECF;  /* đường kẻ, viền nhạt */
    --ink:        #1A1A1A;  /* chữ chính, heading (matte black) */
    --ink-2:      #514C44;  /* chữ phụ, caption, chú thích */
    --jade:       #3CA57A;  /* hành động chính: nút, link, nhấn */
    --jade-deep:  #2D8B6F;  /* hover / pressed */
    --jade-text:  #1B5E48;  /* chữ jade đậm trên nền sáng */
    --jade-pale:  #DCEAE1;  /* nền nhạt, chip, nền call-out */
    --sage:       #A8C9B8;  /* phụ trợ, mid-tone */
    --sage-pale:  #E1ECE4;  /* nền phụ trợ rất nhạt */
    --accent:     #E8A24A;  /* điểm nhấn ấm — dùng RẤT tiết chế, tối đa 1–2 chỗ mỗi màn */
    --correct:    #2D8B6F;  --correct-bg: #DCEAE1;  /* feedback đúng */
    --wrong:      #C15F3C;  --wrong-bg:   #F3E2D6;  /* feedback sai (ấm, không đỏ gắt) */
    --warning:    #C58A2E;  --warning-bg: #F5E7CB;  /* cảnh báo/lưu ý an toàn — dùng cho việc này, KHÔNG dùng --wrong */
    --info:       #4E7F92;  --info-bg:    #DCE7EB;  /* thông tin trung tính */
}
```

•	Nền body: `var(--cream)` — chữ: `var(--ink)`
•	Card/surface: `var(--cream-2)` hoặc `var(--cream)`, border: 1px solid `var(--paper-line)`, border-radius: 10–12px, padding 20–28px, shadow nếu có thì CỰC nhẹ (khuếch tán rộng, độ mờ thấp) — kiểu editorial, không "material design"
•	Button chính (active/primary): nền `var(--jade)`, chữ `var(--cream)`, border-radius: **8–10px (KHÔNG pill tròn)**, hover đổi `var(--jade-deep)`, transition 150–200ms, shadow rất nhỏ hoặc không có
•	Button thường/phụ: viền `var(--jade)` 1.5px, chữ `var(--jade-text)`, nền trong suốt; hover nền `var(--jade-pale)`
•	Call-out: `border-left: 4px solid var(--jade)` trên nền `var(--jade-pale)` — không tô cả khối màu đậm
•	KHÔNG gradient trong UI (CSS), KHÔNG shadow nặng, KHÔNG emoji
•	**Ngoại lệ gradient:** quy tắc "KHÔNG gradient" áp dụng cho UI/chrome chung (nền trang, card, nút, badge...). Các phần tử gắn liền với khu vực canvas/thí nghiệm — ví dụ viền phát sáng `.canvas-glow-wrap` bao quanh canvas (xem mục LAYOUT TỔNG THỂ) — được PHÉP dùng gradient/animation nhẹ để nhấn khu vực tương tác chính, cùng nhóm với ngoại lệ "màu bên trong Canvas" bên dưới.

**NGOẠI LỆ QUAN TRỌNG — màu bên trong Canvas:** màu của hóa chất, dung dịch, kết tủa, chỉ thị, ngọn lửa, dây điện... trong canvas mô phỏng PHẢI theo màu thực tế của hiện tượng hóa học (ví dụ: quỳ/phenolphtalein hồng, Cu(OH)₂ xanh lam, Fe(OH)₃ nâu đỏ, dây điện đỏ/xanh theo quy ước cực), KHÔNG ép theo bảng token. Bảng token chỉ áp dụng cho UI (nền, nút, card, chữ) và các phần trang trí của canvas (lưới nền, nhãn, giá đỡ).

________________________________________
LAYOUT TỔNG THỂ (quan trọng)

•	body: `display: flex; flex-direction: column; align-items: center; gap: 16px; padding: 24px clamp(20px, 2.5vw, 48px)` — padding 2 bên co giãn theo bề rộng màn hình (tối thiểu 20px trên mobile)
•	Độ rộng của MỌI khối lớn (`.briefing`, `.lab-wrapper`): **100%** (`width: 100%`), `margin: 0 auto`. Toàn trang đọc như 1 cột đơn canh giữa (không còn dashboard 3 cột rộng lấp màn hình).
•	Bọc toàn bộ nội dung trong `.app-shell { width:100%; min-width:0; max-width:100%; margin:0 auto; }` để nội dung dùng hết chiều rộng khả dụng của iframe khi tải lên LMS, không tạo khoảng trống dư hai bên. Các khối con vẫn `width:100%` trong cột này.
•	Thứ tự các khối từ trên xuống: ① Thanh briefing gộp mục tiêu + mô tả ngắn, có thể thu gọn → ② `.lab-wrapper` (cột đơn)
•	**Bố cục CỘT ĐƠN (không còn 3 cột/dashboard, không còn sideLeft/sideRight)** — mọi nội dung xếp dọc theo đúng 1 thứ tự duy nhất: thanh hướng dẫn AI (`guide`) → hàng nút tương tác (`controls`) → canvas (`canvas`) → card "Bảng quan sát" → card "Kết luận & trắc nghiệm". Không còn khái niệm "khớp chiều cao 3 cột", không cần JS đo/ép chiều cao nào cả — mỗi card cao tự nhiên theo nội dung, cuộn theo TRANG (không dùng `overflow-y:auto` nội bộ):

```css
.lab-wrapper {
    display: flex;
    flex-direction: column;
    gap: 16px;
    width: 100%;
    margin: 0 auto;
}

.canvas-card {
    width: 100%;
    padding: 4px;
    display: flex;
    justify-content: center;
}

/* Wrapper viền sáng quanh canvas — xoay liên tục, chỉ 1 màu jade, ôm sát đúng khung canvas thật */
.canvas-glow-wrap {
    position: relative;
    width: 100%;
    height: clamp(320px, 42vw, 540px); /* desktop: đủ lớn nhưng không kéo trang quá dài */
    padding: 2px; /* độ dày viền — chỉnh nhỏ/to tại đây */
    border-radius: 10px;
    overflow: hidden;
    display: flex;
}
.canvas-glow-wrap::before {
    content: '';
    position: absolute;
    inset: -80%;
    background: conic-gradient(from 0deg, transparent 0deg, transparent 170deg, var(--jade) 220deg, var(--jade) 340deg, transparent 360deg);
    animation: canvasBorderSpin 4.5s linear infinite;
    z-index: 0;
}
@keyframes canvasBorderSpin { to { transform: rotate(360deg); } }

#labCanvas {
    position: relative;
    z-index: 1;
    background-color: var(--cream);
    border-radius: 8px;
    width: 100%;
    height: 100%;
    display: block;
    touch-action: none;
    flex: 1; /* lấp đầy chiều cao responsive của wrapper; buffer pixel vẫn được JS đồng bộ */
}
```

HTML: canvas PHẢI được bọc trong `.canvas-glow-wrap` (không đặt `<canvas>` trực tiếp làm con của `.canvas-card`):
```html
<div class="card canvas-card">
    <div class="canvas-glow-wrap">
        <canvas id="labCanvas" width="760" height="380" role="img" aria-label="..."></canvas>
    </div>
</div>
```

Thứ tự 5 card con trực tiếp trong `.lab-wrapper` (tất cả full-width 100%, xếp dọc bằng flex, không dùng grid/area):
1. `.ai-guide` — thanh hướng dẫn Athena
2. `.controls-card` — hàng nút tương tác (Kiểu A/B/C)
3. `canvas-card` — canvas thí nghiệm (bọc `.canvas-glow-wrap`)
4. Card "Bảng quan sát"
5. Card "Kết luận & trắc nghiệm"

**Tối ưu mobile (BẮT BUỘC):**

1.	**Ghim nút hành động chính xuống đáy màn hình khi ≤767px** — vì `controls-card` đứng ở vị trí thứ 2 (ngay trên canvas), nhưng học sinh thường cuộn xuống đọc "Kết luận & trắc nghiệm" TRƯỚC khi bấm tiếp — lúc đó phải cuộn ngược lên mới bấm được nút, gây khó chịu. Ghim cố định card `controls` xuống đáy màn hình trong trường hợp này:
```css
@media (max-width: 767px) {
  .canvas-glow-wrap {
    height: clamp(300px, 48vh, 420px); /* mobile: ưu tiên vùng quan sát theo chiều cao màn hình */
  }
  body { gap: 9px; padding: 12px 12px 82px; }
  .app-shell, .lab-wrapper { gap: 9px; }
  .briefing { padding: 7px 10px; }
  .briefing-details { font-size: 12.5px; line-height: 1.4; }
  .goal-label { gap: 4px; font-size: 10px; letter-spacing: 0.45px; }
  .goal-text { font-size: 13px; line-height: 1.4; }
  .ai-guide { align-items: flex-start; gap: 8px; padding: 8px 10px; }
  .ai-avatar { flex-basis: 32px; width: 32px; height: 32px; border-width: 1.5px; }
  .guide-text { display: -webkit-box; overflow: hidden; -webkit-box-orient: vertical; -webkit-line-clamp: 2; line-clamp: 2; font-size: 12.5px; line-height: 1.4; }
  .controls-card {
    position: fixed !important; left: 0; right: 0; bottom: 0; z-index: 50;
    background: var(--cream-2); border-top: 1px solid var(--paper-line);
    box-shadow: 0 -4px 16px rgba(0,0,0,0.10);
    padding: 8px 14px calc(8px + env(safe-area-inset-bottom, 0px));
    margin: 0 !important;
  }
}
```

Trên mobile, briefing mặc định thu gọn; hướng dẫn Athena theo bước rút còn 1–2 câu ngắn. Khi học sinh bắt đầu thí nghiệm, briefing tự thu gọn để canvas tiến lên gần đầu màn hình.

2.	**Không cuộn ngang bất ngờ ở 375px** — test thực tế ở đúng 375px (không chỉ 390/414px), đặc biệt hàng `controls` có nhiều nút hoặc nhiều nhóm biến: phải cuộn NGANG được bên trong chính hàng đó (`overflow-x:auto`), tuyệt đối không để tràn ra ngoài viewport đẩy cả trang cuộn ngang.

Các `select`, `input` và `textarea` trên mobile bắt buộc dùng `font-size:16px`, `width:100%`, `min-width:0`, `max-width:100%`. Mốc 16px ngăn Safari/iOS tự phóng trang khi mở dropdown; `min-width:0` ngăn field làm giãn card. Dùng cùng một custom select trên cả desktop và mobile để giao diện nhất quán; không hiển thị popup native của `<select>`. Hãy giữ `<select>` ẩn làm nguồn dữ liệu và dựng một custom select accessible (`button[aria-haspopup="listbox"]` + menu `role="listbox"`) có chiều rộng đúng `100%` của field. Option dài được wrap, menu có `max-height` và tự mở lên trên nếu không đủ chỗ phía dưới. Khi chọn custom option phải cập nhật giá trị select gốc rồi phát sự kiện `change` để không phá logic bài học.

3.	Layout đã LUÔN là cột đơn ở mọi kích thước màn hình (không riêng mobile) — không cần media query đổi số cột hay reset chiều cao sidebar như bản 3-cột trước, vì không còn sidebar nào cả. Trang phải chạy tốt từ 360px (mobile) tới desktop chỉ với 1 bộ CSS.

________________________________________
BRIEFING GỘP MỤC TIÊU + MÔ TẢ (thay hoàn toàn cho header banner — BẮT BUỘC)

Không dựng `<header>` trang trí. Gộp mục tiêu học tập và đoạn dẫn giải vào một thanh `.briefing` duy nhất để giảm chiều cao phần đầu. Hàng chính luôn hiển thị label MỤC TIÊU, câu giả thuyết và nút Thu gọn/Xem thêm; mô tả 1 câu đặt trong `.briefing-details`. Trên mobile mặc định thu gọn, trên desktop mặc định mở; sau khi bấm bắt đầu thì tự thu gọn.

```html
<section class="briefing" id="briefing">
  <div class="briefing-main">
    <span class="goal-label"><i class="ti ti-target"></i>Mục tiêu</span>
    <span class="goal-divider"></span>
    <span class="goal-text">Kiểm chứng giả thuyết: ...</span>
    <button class="briefing-toggle" id="briefingToggle" aria-expanded="true">Thu gọn</button>
  </div>
  <p class="briefing-details" id="briefingDetails">Bạn sẽ [1 câu mô tả ngắn].</p>
</section>
```
```css
.briefing { width:100%; padding:10px 16px; background:var(--jade-pale); border:1px solid var(--sage); border-left:4px solid var(--jade); border-radius:10px; }
.briefing-main { display:flex; align-items:center; gap:10px; }
.briefing-details { margin:7px 0 0; color:var(--ink-2); line-height:1.5; }
.briefing.is-collapsed .briefing-details { display:none; }
.briefing-toggle { margin-left:auto; flex-shrink:0; background:transparent; border:0; color:var(--jade-text); cursor:pointer; }
```

**File cũ đang patch/sửa lỗi (đã có sẵn header cũ, không build lại từ đầu):** KHÔNG xoá `<header>`/JS
liên quan — chỉ ẩn bằng CSS (`.app-header, header { display: none !important; }`), giữ nguyên node
để tránh vỡ code khác đang trỏ vào id/class bên trong nó, rồi thêm `.briefing` mới ngay sau vị trí
header cũ (đã ẩn), trước phần thân chính.

________________________________________
THÀNH PHẦN MỤC TIÊU TRONG BRIEFING

•	CHỈ gồm 1 khối duy nhất: **Mục tiêu học tập** — dạng THANH NGANG MỎNG trong hàng chính của briefing (không phải card to nhiều dòng)
•	`display: flex; align-items: center; gap: 10px; width: 100%`. Label, divider và text đều canh giữa theo chiều dọc của hàng.
•	Style kiểu call-out: background: `var(--jade-pale)`; border-left: 4px solid `var(--jade)`; border-radius: 10px; padding: 10px 16px
•	Bên trái: label "MỤC TIÊU" = `<span>` riêng chứa icon Tabler (ti-target) + chữ, style `display: flex; align-items: center; gap: 6px; flex-shrink: 0` — uppercase 12px weight 600 màu `var(--jade-text)`, không xuống dòng
•	Ngay sau label là **1 gạch chia dọc** (`<span class="goal-divider">`) ngăn cách label với text mục tiêu: `flex-shrink: 0; align-self: stretch; width: 1px; margin: 3px 0; background-color: var(--sage)`. Dùng `align-self: stretch` + `margin` dọc thay vì `height` cố định — để gạch luôn TỰ ĐỘNG co giãn theo đúng chiều cao thật của hàng (kể cả khi text mục tiêu dài xuống 2–3 dòng) nhưng vẫn cách 2 mép trên/dưới của khung một chút (không chạm sát viền), không được nối thẳng từ mép trên xuống mép dưới khung.
•	Tiếp theo là text mục tiêu (`.goal-text`): 17px, màu `var(--ink)`, cho phép wrap nhiều dòng bình thường (không giới hạn 1 dòng); trên mobile giảm xuống ~15px
•	KHÔNG có khối "Hướng dẫn thao tác" — việc hướng dẫn từng bước do thanh AI trong lab-wrapper đảm nhiệm

________________________________________
THANH HƯỚNG DẪN AI (`.ai-guide`, đứng đầu tiên trong `.lab-wrapper`)

Đây là "người dẫn đường" của thí nghiệm — thay thế hoàn toàn khối hướng dẫn tĩnh:
•	Nhân vật dẫn dắt LUÔN có tên riêng **"Athena"** — không gọi là "AI"/"Robot" ở bất kỳ đâu học sinh nhìn thấy (text hướng dẫn, alt ảnh, comment code liên quan...), kể cả khi kịch bản gốc còn ghi tên khác.
•	Card ngang `.ai-guide`: `display: flex; align-items: center; gap: 12px`; background: `var(--jade-pale)`; border: 1px solid `var(--sage)`; border-radius: 12px; padding: 12px 16px
•	Bên TRÁI: avatar Athena, class `.ai-avatar` — dùng ảnh thật, KHÔNG dùng icon Tabler nữa:
```html
<div class="ai-avatar"><img src="https://www.aiducation.edu.vn/athena/athena_idle.webp" alt="Athena AI"></div>
```
```css
.ai-avatar {
  flex-shrink: 0; width: 44px; height: 44px; border-radius: 50%;
  overflow: hidden; background-color: transparent; border: 2px solid var(--sage);
}
.ai-avatar img { width: 100%; height: 100%; object-fit: cover; display: block; }
```
`object-fit: cover` giữ ảnh tròn trịa, không bị bóp méo tỷ lệ dù khung `.ai-avatar` là hình tròn còn ảnh WebP gốc là hình chữ nhật/vuông.
•	Ở giữa: text hướng dẫn theo ngữ cảnh (`flex: 1`) — cỡ ~1rem, weight 500–600, màu `var(--ink)`, line-height 1.5. Nội dung do hàm `updateAIGuide()` cập nhật theo bước/hành động/kết quả. Vùng text đặt `aria-live="polite"` để screen reader đọc được diễn biến.
•	Khi text đổi: fade transition (opacity 0→1 + translateY 4px, ~200ms)
•	Giọng văn AI: thân thiện, chủ động, ngắn gọn 1–2 câu — nêu việc vừa xảy ra + việc cần làm tiếp (ví dụ: "Đã dùng ngón tay bịt miệng ống nghiệm. Tiếp theo úp ngược ống nghiệm vào cốc nước.")

________________________________________
HÀNG NÚT TƯƠNG TÁC (`.controls-card`, đứng thứ 2 trong `.lab-wrapper` — giữa thanh AI và canvas)

Có 3 kiểu controls — phần KỊCH BẢN bên dưới sẽ chỉ định dùng kiểu nào. Cả 3 kiểu đều nằm ở CÙNG vị trí này (1 card ngang ngay dưới thanh AI, phía trên canvas), chỉ khác nội dung bên trong.

**QUY TẮC BẮT BUỘC — CHỈ 1 DÒNG:** card controls PHẢI nén gọn trong ĐÚNG 1 hàng ngang duy nhất (`display: flex; flex-wrap: nowrap; align-items: center;`), TUYỆT ĐỐI không được xuống dòng thứ 2 dù ở kiểu nào. Chiều cao card cố định thấp (~60–64px kể cả padding — đủ chứa nút cao tối thiểu 44px, xem "Style chung tất cả kiểu" bên dưới). Lý do: phần chiều cao tiết kiệm được từ việc nén controls xuống 1 dòng sẽ CHUYỂN THẲNG sang tăng chiều cao canvas — xem mục YÊU CẦU CANVAS. Nếu nội dung một kiểu rộng hơn bề ngang card ở màn hình hẹp: cho phép cuộn NGANG bên trong chính hàng đó (`overflow-x: auto; overflow-y: hidden;`) — không bao giờ wrap xuống dòng.

### Kiểu A — Chọn biến + Nút hành động
Dùng cho thí nghiệm so sánh nhiều tổ hợp (chọn hóa chất A, chọn chỉ thị B, bấm "Nhỏ 1 giọt").
•	TOÀN BỘ nằm trên 1 hàng ngang duy nhất, các nhóm biến nối tiếp nhau theo chiều ngang (không còn xếp mỗi nhóm 1 hàng như kiểu cũ): `[label ngắn/icon] [chip lựa chọn A][chip B][chip C]` → dấu ngăn cách dọc `border-right: 1px solid var(--paper-line)` (padding-right 10px) → nhóm biến kế tiếp → cuối hàng là nút hành động chính + nút Reset (dashed border)
•	Label mỗi nhóm biến rút gọn tối đa (ưu tiên icon Tabler thay chữ nếu đủ rõ nghĩa, hoặc chữ ≤ 2 từ) để tiết kiệm bề ngang
•	Button active: nền `var(--jade)`, chữ `var(--cream)`
•	Button disabled: opacity: 0.55, cursor: not-allowed

### Kiểu B — Stepper tuần tự
Dùng cho thí nghiệm nhiều bước phải thực hiện theo thứ tự.
•	KHÔNG cần header bar "Quy Trình Thí Nghiệm" hay step card mô tả riêng — tên bước và mô tả đã do THANH HƯỚNG DẪN AI phía trên đảm nhiệm
•	1 hàng duy nhất, chỉ giữ nút "Làm lại" và nút hành động chính của bước hiện tại nằm ngoài cùng bên phải. Không dùng nút "Quay lại" vì làm tăng tải nhận thức nhưng không hoàn tác trạng thái thí nghiệm. Label nút chính đổi theo ngữ cảnh bước. KHÔNG xuống hàng thứ 2.
•	Nút hành động chính PHẢI disabled khi animation đang chạy
•	Bỏ dòng "Status" riêng (đã tiết kiệm để giữ 1 dòng) — trạng thái ("Đang thực hiện...", "Hoàn tất"...) gộp vào text của THANH HƯỚNG DẪN AI phía trên thay vì hiện riêng ở đây

### Kiểu C — Timeline slider
Dùng cho thí nghiệm quan sát biến đổi theo thời gian (ăn mòn, oxi hóa...).
•	1 hàng duy nhất: badge phase hiện tại (trái, chip `var(--jade-pale)` chữ `var(--jade-text)`) → `input[type="range"]` chiếm phần còn lại (`flex: 1`, styled theo design system: track `var(--paper-line)`, thumb `var(--jade)`) → nút "Tự động chạy" + nút Reset (phải)
•	Label mốc thời gian 2 đầu (nếu cần) đặt ngay trên/dưới thanh trượt bằng `font-size: 0.75rem`, không chiếm thêm hàng riêng — hoặc bỏ nếu badge phase đã đủ diễn đạt
•	Nút "Tự động chạy": animate slider từ min→max trong N giây (mặc định 7s)
•	Trên mobile (≤640px): tăng kích thước núm kéo để dễ chạm bằng ngón tay:
```css
input[type="range"]::-webkit-slider-thumb { width: 26px; height: 26px; }
input[type="range"]::-moz-range-thumb { width: 26px; height: 26px; }
```

### Style chung tất cả kiểu
•	Card controls: background: `var(--cream-2)`; border: 1px solid `var(--paper-line)`; border-radius: 12px; padding: 8px 14px — TỐI GIẢN, đây là hàng thấp nhất có thể, không phải 1 "card nội dung" như các card khác
•	Nút xếp ngang bằng flex, gap 6–8px, `flex-shrink: 0`; chiều cao nút **tối thiểu 44px** (chuẩn vùng chạm mobile — ưu tiên đúng 44px hơn là giữ hàng thấp; nếu cần bù chiều cao, giảm padding dọc của card controls thay vì hạ chiều cao nút)
•	Nút: border-radius: **8–10px** (không pill), font-weight: 600, transition: 150–200ms, không shadow hoặc shadow cực nhẹ
•	Tất cả nút dùng icon Tabler, không emoji; ưu tiên hiện icon + ẩn bớt label chữ nếu cần thu gọn thêm bề ngang (giữ `aria-label` cho a11y)
•	Nút reset: border: 1.5px dashed `var(--jade)`, chữ `var(--jade-text)`, nền trong suốt; hover nền `var(--jade-pale)`
•	Trong suốt animation đang chạy (isAnimating = true, hoặc bất kỳ subStep = 1 hay 3): TẤT CẢ button phải disabled — áp dụng cho cả 3 kiểu A, B, C, không chỉ nút "Tiếp theo"
•	Nút vừa trigger animation: thay icon thường bằng ti-loader (CSS `animation: spin 0.9s linear infinite`) cho đến khi subStep = 4 hoặc animation hoàn tất

________________________________________
VỊ TRÍ TƯƠNG TÁC — NGOÀI CANVAS HAY TRÊN CANVAS (tùy kịch bản, không chốt 1 kiểu)

Tương tác chính của thí nghiệm có thể đặt ở **1 trong 2 vị trí**, kịch bản sẽ chỉ định rõ dùng kiểu nào cho từng bước — cả hai đều hợp lệ trong design system này:

•	**Ngoài canvas (mặc định)** — học sinh bấm nút/chip trong hàng CONTROLS (Kiểu A/B/C ở trên), canvas chỉ hiển thị kết quả/animation. Dùng khi kịch bản không yêu cầu học sinh chỉ trực tiếp vào vật thể.
•	**Trực tiếp trên canvas** — học sinh click/chạm vào 1 điểm/vùng trong chính hình vẽ thí nghiệm (VD: click đúng ống nghiệm cần thao tác, chạm vào vùng kết tủa để xem chú thích, chọn dụng cụ đang vẽ trên canvas). Dùng khi kịch bản mô tả rõ "học sinh click vào X trên hình" thay vì "bấm nút X".

**Nếu dùng tương tác trực tiếp trên canvas — BẮT BUỘC theo quy tắc sau (áp dụng cho MỌI tooltip/hitbox trên canvas):**

```
Vấn đề: mouseenter/mouseleave một mình KHÔNG chạy trên thiết bị cảm ứng (không có trạng thái
hover) — mọi tooltip/thông tin gắn theo hover sẽ hoàn toàn không hiện được trên mobile/tablet dù
giao diện đã responsive đúng.

Quy tắc bắt buộc: mọi điểm click/chạm trên canvas dùng 'click' (chạy đúng cho cả chuột lẫn tay
chạm), theo mô hình tap-to-toggle — chạm lần 1 để hiện, chạm lại (hoặc chạm ra ngoài) để ẩn.
```

```javascript
// Quy đổi tọa độ click về hệ tọa độ NGUỒN 760×600 của cảnh — dùng cùng fitScale,
// offsetX/offsetY và sceneScale responsive như trong loop() (mục B2)
function getLogicPos(e) {
  const rect = canvas.getBoundingClientRect();
  const scaleX = canvasW / W, scaleY = canvasH / H;
  const fitScale = Math.min(scaleX, scaleY);
  const offsetX = (canvasW - W * fitScale) / 2;
  const offsetY = (canvasH - H * fitScale) / 2;
  const px = e.clientX - rect.left, py = e.clientY - rect.top;
  const logicX = (px - offsetX) / fitScale;
  const logicY = (py - offsetY) / fitScale;
  const sceneScale = getSceneScale();
  const sceneOffsetX = (W - W * sceneScale) / 2;
  return {
    x: (logicX - sceneOffsetX) / sceneScale,
    y: logicY / sceneScale
  };
}

let activeHitId = null;
canvas.addEventListener('click', (e) => {
  const { x, y } = getLogicPos(e);
  const hit = hitTargets.find(t => Math.hypot(x - t.x, y - t.y) < t.radius);
  if (hit && activeHitId === hit.id) { hideCanvasTooltip(); activeHitId = null; }
  else if (hit) { showCanvasTooltip(hit); activeHitId = hit.id; }
  else { hideCanvasTooltip(); activeHitId = null; }
});
```

Checklist bổ sung khi kịch bản dùng tương tác trên canvas:
- [ ] Không có `mouseenter`/`mouseleave` đứng một mình cho tooltip/hitbox — luôn có `click` song song
- [ ] Vùng chạm hitbox đủ lớn cho ngón tay (bán kính tối thiểu tương đương ~40px sau khi quy đổi ra pixel thật, không chỉ đủ cho con trỏ chuột)
- [ ] Tooltip tự kẹp trong khung nhìn của canvas/trang, không tràn mép màn hình ở 375px

________________________________________
KHỐI KẾT QUẢ (dưới canvas — 2 card cuối cùng trong `.lab-wrapper`, xếp dọc, không còn ở sidebar)

2 card này giờ là 2 card thường, đứng ngay dưới `canvas-card` trong cùng cột đơn 100%, chiều cao tự nhiên theo nội dung, cuộn theo TRANG như mọi card khác — không còn `flex:1 1 auto`, không còn `min-height:0`, không còn `overflow-y:auto` nội bộ hay đồng bộ chiều cao với canvas (khái niệm đó đã bỏ hoàn toàn cùng với việc bỏ 3 cột).

### Khối 1 — "Bảng quan sát"
•	Card: nền `var(--cream-2)`, border 1px `var(--paper-line)`, radius 12px, padding 14px 16px
•	Tiêu đề: icon ti-table + "Bảng quan sát", weight 700
•	Áp dụng progressive disclosure: trước khi xong bước 3 chỉ hiện một dòng nhắc “Bảng quan sát sẽ mở sau bước 3”; sau đó mới hiện bảng. Các hàng chưa đến mốc mở phải `display:none`, không chiếm chiều cao.
•	Bảng `<table>` nhỏ gọn: hàng header nền `var(--jade-pale)`, chữ `var(--jade-text)` uppercase 12px weight 600; ô có border-bottom 1px `var(--paper-line)`; chữ nội dung 0.9rem màu `var(--ink)`
•	Cột/hàng của bảng do KỊCH BẢN quy định (điều kiện thí nghiệm, hiện tượng quan sát...)
•	Giá trị CHƯA quan sát được: hiển thị "?" màu `var(--ink-2)` — khi bước tương ứng hoàn thành, JS điền giá trị thật kèm fade + nền ô nháy `var(--jade-pale)` khoảng 1s để hút mắt học sinh
•	Không đặt custom dropdown bên trong ancestor có `overflow:auto/hidden` vì menu mở lên hoặc xuống sẽ bị cắt. Với bảng có custom select, `.table-wrap` dùng `overflow:visible`; tại breakpoint mobile bảng đã chuyển thành card nên không cần cuộn ngang. Field đang mở phải có `z-index` cao hơn các hàng xung quanh.
•	Trên mobile ≤767px, không ép người dùng cuộn ngang bảng: ẩn `thead`, chuyển từng hàng đã mở thành card dọc và dùng `td::before { content: attr(data-label) }` làm nhãn. Mỗi `<td>` bắt buộc có `data-label` tương ứng.
•	Chiều cao tự nhiên theo nội dung

### Khối 2 — "Kết luận & trắc nghiệm"
Đây là explanationBox — chứa kết luận và câu hỏi trắc nghiệm THEO TỪNG BƯỚC thí nghiệm:
•	Toàn bộ card này ẩn cho tới khi học sinh hoàn thành 8 bước và ghi đúng đủ các dòng quan sát; không hiển thị một card khóa dài từ đầu trang.
•	Card ngoài: nền `var(--cream-2)`, border 1px `var(--paper-line)`, radius 12px, padding 14px 16px; tiêu đề icon ti-checklist + "Kết luận & Câu hỏi", weight 700
•	Chiều cao tự nhiên theo nội dung, KHÔNG giới hạn `max-height`/`overflow-y:auto` nội bộ — nội dung dài bao nhiêu thì trang dài bấy nhiêu, học sinh cuộn trang bình thường như đọc 1 bài viết.
•	Nội dung tổ chức theo bước: mỗi bước hoàn thành sẽ THÊM 1 khối kết luận nhỏ + câu trắc nghiệm tương ứng (nếu kịch bản có) vào cuối danh sách — khối mới nhất tự cuộn vào tầm nhìn (`scrollIntoView({behavior:'smooth', block:'nearest'})`)
•	Mỗi khối con — trạng thái ban đầu (đang quan sát/chờ trả lời): background: `var(--sage-pale)`; border: 1px solid `var(--sage)`; border-left: 4px solid var(--jade)
` (điểm nhấn accent chính trong sim — giữ đúng tinh thần dùng rất tiết chế)
•	Mỗi khối con — sau khi có kết quả/trả lời đúng: background: `var(--correct-bg)`; border: 1px solid `var(--sage)`; border-left: 4px solid `var(--correct)`
•	Tiêu đề khối con dùng icon ti-info-circle (quan sát) / ti-circle-check (kết luận)
•	Nếu có quiz/câu hỏi: đáp án là các nút/card outline; chọn đúng → viền + nền `var(--correct)`/`var(--correct-bg)` + icon check; chọn sai → `var(--wrong)`/`var(--wrong-bg)` nhẹ nhàng, kèm chỉ dẫn tới đáp án đúng — không "phạt", không đỏ gắt. Luôn có feedback tức thì kèm transition.
•	**Căn hàng quiz options (BẮT BUỘC — chống lệch hàng):** mọi đáp án phải thẳng hàng tuyệt đối với nhau, không đáp án nào bị thụt vào:
    o	Mỗi option là `<button>` full-width: `display: flex; align-items: flex-start; text-align: left; width: 100%; gap: 10px;` — cùng một padding cho TẤT CẢ option (ví dụ `padding: 10px 14px`), không dùng text-indent, không thêm khoảng trắng/`&nbsp;` đầu chuỗi
    o	Ký hiệu đáp án (A/B/C/D) đặt trong `<span>` riêng có bề rộng cố định: `flex: 0 0 24px` (hoặc min-width: 24px) — KHÔNG gõ "A." dính liền vào chuỗi nội dung
    o	Phần nội dung đáp án trong `<span>` riêng: `flex: 1` — chữ xuống dòng vẫn thẳng mép trái với dòng trên
    o	Icon feedback (check/x) khi trả lời KHÔNG được chèn vào đầu text làm xô lệch — đặt ở cuối option hoặc reserve sẵn chỗ cố định
    o	Container danh sách đáp án: nếu dùng `<ul>`/`<ol>` phải reset `list-style: none; margin: 0; padding: 0;` để tránh browser tự thụt lề mặc định
•	Fade transition khi đổi nội dung: thêm class fade → opacity: 0; transform: translateY(6px), sau 200ms xóa class

________________________________________
YÊU CẦU CANVAS

Lưu ý chung: gradient/glow/shadow BỊ CẤM trong UI (CSS) nhưng ĐƯỢC PHÉP bên trong canvas khi dùng để mô phỏng vật thể thật (ánh kim loại, chất lỏng, LED phát sáng) — dùng tiết chế, phục vụ tính chân thực, không trang trí thừa.

**Chiều cao hiển thị canvas dùng `clamp()` trên `.canvas-glow-wrap`**: desktop `clamp(320px, 42vw, 540px)`; mobile ≤767px dùng `clamp(300px, 48vh, 420px)` để vùng mô phỏng lớn nhưng không lấn hết màn hình. Hệ logic JavaScript vẫn là `760×380`; `loop()` dùng `fitScale` và offset để tự căn giữa cảnh mà không kéo méo X/Y. Cảnh nguồn dùng scale responsive: `0.67` trên desktop và `0.88` trên mobile. `resizeCanvas()` chỉ đồng bộ buffer pixel theo kích thước CSS thực tế.

### A. Kích thước & Utility bắt buộc
```js
const canvas = document.getElementById('labCanvas');
const ctx = canvas.getContext('2d');
const W = 760, H = 380; // hệ tọa độ logic; chiều cao CSS được điều khiển độc lập bằng clamp()
const mobileCanvasQuery = window.matchMedia('(max-width: 767px)');
function getSceneScale() { return mobileCanvasQuery.matches ? 0.88 : 0.67; }
let canvasW = W, canvasH = H; // kích thước PIXEL THẬT của canvas trên màn hình — resizeCanvas() cập nhật liên tục

// Kích thước CSS của canvas do width/height responsive trên .canvas-glow-wrap quyết định —
// hàm này chỉ đo kích thước hiển thị THẬT rồi đồng bộ vào canvas.width/height (buffer pixel), gọi ở
// đầu mỗi frame trong loop(). Nhờ vậy nền lưới ô vuông luôn phủ kín 100% khung, ảnh không bị mờ/vỡ nét
// do lệch tỉ lệ buffer/CSS, dù màn hình có DPR khác nhau.
function resizeCanvas() {
    const rect = canvas.getBoundingClientRect();
    if (canvas.width !== Math.round(rect.width) || canvas.height !== Math.round(rect.height)) {
        canvas.width = Math.round(rect.width);
        canvas.height = Math.round(rect.height);
        canvasW = canvas.width;
        canvasH = canvas.height;
    }
}

function lerp(a, b, t) { return a + (b - a) * t; }
function clamp(v, lo, hi) { return Math.max(lo, Math.min(hi, v)); }
function easeOut(t) { return 1 - (1 - t) * (1 - t); }
function easeInOut(t) { return t < .5 ? 2 * t * t : 1 - Math.pow(-2 * t + 2, 2) / 2; }

function lerpColor(c1, c2, t) {
    t = clamp(t, 0, 1);
    return `rgb(${Math.round(c1.r+(c2.r-c1.r)*t)},${Math.round(c1.g+(c2.g-c1.g)*t)},${Math.round(c1.b+(c2.b-c1.b)*t)})`;
}
```

### B. Lưới nền (bắt buộc vẽ đầu tiên mỗi frame — dùng kích thước THẬT, không dùng W/H logic)
```js
function drawGrid() {
    ctx.save();
    ctx.strokeStyle = 'rgba(26,26,26,0.05)'; // tông ink theo design system
    ctx.lineWidth = 1;
    for (let x = 0; x <= canvasW; x += 30) { ctx.beginPath(); ctx.moveTo(x,0); ctx.lineTo(x,canvasH); ctx.stroke(); }
    for (let y = 0; y <= canvasH; y += 30) { ctx.beginPath(); ctx.moveTo(0,y); ctx.lineTo(canvasW,y); ctx.stroke(); }
    ctx.restore();
}
```
Nền canvas: `var(--cream)` hoặc trắng ngà ấm — đồng bộ với nền trang.

### B2. `loop()` — bắt buộc theo đúng khung này để canvas lấp đầy mà không méo nội dung
Canvas hiển thị theo khung logic 760×380. Các hàm dựng cảnh có thể tiếp tục dùng hệ tọa độ nguồn 760×600 để dễ bố trí dụng cụ; trong `loop()` phải thu đều cảnh bằng `SCENE_SCALE = 0.67` và căn giữa bằng `SCENE_OFFSET_X`. Tuyệt đối không kéo giãn X/Y bằng hai tỉ lệ khác nhau.
```js
function loop() {
    resizeCanvas();
    time += 0.045;
    updateTweens(16.7);
    ctx.clearRect(0, 0, canvasW, canvasH);
    drawGrid(); // vẽ NGOÀI khối scale — luôn phủ kín canvas thật, không méo vì chỉ là lưới kẻ đơn giản

    // Tỉ lệ + độ lệch để nội dung W×H (logic) nằm vừa khít, canh giữa, giữ nguyên tỉ lệ trong canvas thật
    const scaleX = canvasW / W;
    const scaleY = canvasH / H;
    const fitScale = Math.min(scaleX, scaleY);
    const offsetX = (canvasW - W * fitScale) / 2;
    const offsetY = (canvasH - H * fitScale) / 2;

    const sceneScale = getSceneScale();
    const sceneOffsetX = (W - W * sceneScale) / 2;
    ctx.save();
    ctx.translate(offsetX, offsetY);
    ctx.scale(fitScale, fitScale);
    ctx.translate(sceneOffsetX, 0);
    ctx.scale(sceneScale, sceneScale); // 0.67 desktop; 0.88 mobile

    switch (state.stage) {
        case 1: drawStage1(); break;
        case 2: drawStage2(); break;
        // ... các stage khác
    }

    // particles/rings PHẢI vẽ TRONG khối save/restore này (dùng chung tọa độ logic với nội dung thí nghiệm)
    updateRings();  rings.forEach(r => { /* vẽ ring như cũ */ });
    updateParticles(); particles.forEach(p => { /* vẽ particle như cũ */ });

    ctx.restore(); // đóng khối translate/scale — BẮT BUỘC, thiếu dòng này mọi thứ vẽ sau sẽ bị lệch

    requestAnimationFrame(loop);
}
```
Vì sao làm vậy: `drawGrid()` vẽ THEO KÍCH THƯỚC THẬT nên luôn phủ kín 100% khung viền. Nội dung mô phỏng được nhân lần lượt với `fitScale` và `sceneScale`, đều là scale đồng nhất, nên dụng cụ không méo. Desktop giữ canvas thấp; mobile vừa tăng tỷ lệ khung vừa phóng cảnh lên 0.88 để dụng cụ chiếm phần lớn canvas, không tạo khoảng trắng vô ích.

### C. Hệ thống particle (bắt buộc)
Mọi file đều cần mảng particles[] để tạo hiệu ứng (bọt khí, kết tủa, bột rơi, hơi bay...). Cấu trúc mỗi particle:
```js
let particles = [];
// Mỗi particle là object: { x, y, vx, vy, life, size, color, ... }
// Có thể thêm: targetY (kết tủa lắng), wobble (dao động ngang), gravity, alpha, targetAlpha

// Trong loop(): gọi updateParticles() rồi drawParticles()
// updateParticles: cập nhật position, giảm life, xóa particle hết life
// drawParticles: vẽ từng particle với globalAlpha = life hoặc alpha
```
Tùy thí nghiệm mà particle hoạt động khác nhau:
•	**Kết tủa / bột chìm**: spawn trên mặt nước, vy dương (rơi xuống), dừng tại targetY, wobble ngang nhẹ
•	**Bọt khí**: spawn đáy bình, vy âm (bay lên), pop khi chạm mặt nước, size dao động nhẹ
•	**Hơi / khói**: spawn trên mặt nước, vy âm (bay lên), fade dần, spread ngang

### D. Code vẽ ống nghiệm (dùng cho thí nghiệm kiểu ống nghiệm)
Khai báo tọa độ cố định ở đầu script:
```js
const TX = W / 2;   // tọa độ X ống nghiệm
const TY = 150;     // tọa độ Y đỉnh ống nghiệm
let dropY = TY - 80; // giọt bắt đầu rơi từ trên miệng ống
```

```js
function drawTestTube() {
    const tx = TX, ty = TY;
    ctx.save();

    // Thân ống nghiệm
    ctx.beginPath();
    ctx.moveTo(tx - 18, ty);
    ctx.lineTo(tx - 18, ty + 110);
    ctx.quadraticCurveTo(tx - 18, ty + 135, tx, ty + 135);
    ctx.quadraticCurveTo(tx + 18, ty + 135, tx + 18, ty + 110);
    ctx.lineTo(tx + 18, ty);
    ctx.fillStyle = 'rgba(220, 235, 255, 0.45)';
    ctx.fill();
    ctx.strokeStyle = '#94a3b8';
    ctx.lineWidth = 2;
    ctx.stroke();

    // Chất lỏng bên trong (nội suy màu theo reactProgress)
    const liquidColor = hasReacted
        ? lerpColor(initialLiquidColor, targetLiquidColor, reactProgress)
        : 'rgba(220, 235, 255, 0.6)';
    ctx.beginPath();
    ctx.moveTo(tx - 15, ty + 60);
    ctx.lineTo(tx - 15, ty + 110);
    ctx.quadraticCurveTo(tx - 15, ty + 130, tx, ty + 130);
    ctx.quadraticCurveTo(tx + 15, ty + 130, tx + 15, ty + 110);
    ctx.lineTo(tx + 15, ty + 60);
    ctx.fillStyle = liquidColor;
    ctx.fill();

    // Vệt sáng (highlight bên trái)
    ctx.beginPath();
    ctx.moveTo(tx - 12, ty + 10);
    ctx.lineTo(tx - 12, ty + 115);
    ctx.strokeStyle = 'rgba(255, 255, 255, 0.7)';
    ctx.lineWidth = 3;
    ctx.stroke();

    // Miệng ống
    ctx.beginPath();
    ctx.moveTo(tx - 18, ty);
    ctx.lineTo(tx + 18, ty);
    ctx.strokeStyle = '#94a3b8';
    ctx.lineWidth = 2.5;
    ctx.stroke();

    // Giá đỡ
    ctx.beginPath();
    ctx.moveTo(tx - 28, ty - 10);
    ctx.lineTo(tx + 28, ty - 10);
    ctx.strokeStyle = '#cbd5e1';
    ctx.lineWidth = 4;
    ctx.lineCap = 'round';
    ctx.stroke();

    // Nhãn hóa chất
    ctx.fillStyle = '#1A1A1A';
    ctx.font = "bold 12px 'JetBrains Mono', monospace";
    ctx.textAlign = 'center';
    ctx.fillText(currentChemLabel, tx, ty + 155);
    ctx.restore();
}
```

### E. Code vẽ bút nhỏ giọt (pipette/dropper)
```js
function drawDropper(x, y, liquidColor, label) {
    ctx.save();

    // Bóng đèn cao su (rubber bulb) — phần bóp
    ctx.fillStyle = '#e53e3e';
    ctx.beginPath();
    ctx.moveTo(x - 9, y - 10);
    ctx.quadraticCurveTo(x - 14, y - 35, x, y - 40);
    ctx.quadraticCurveTo(x + 14, y - 35, x + 9, y - 10);
    ctx.closePath();
    ctx.fill();
    // Vệt sáng bulb
    ctx.fillStyle = 'rgba(255,255,255,0.25)';
    ctx.beginPath();
    ctx.ellipse(x - 4, y - 28, 3, 8, -0.3, 0, Math.PI * 2);
    ctx.fill();

    // Thân ống thủy tinh (thon dần xuống đầu nhỏ giọt)
    ctx.fillStyle = 'rgba(220, 235, 255, 0.45)';
    ctx.strokeStyle = 'rgba(148, 163, 184, 0.7)';
    ctx.lineWidth = 1.5;
    ctx.beginPath();
    ctx.moveTo(x - 7, y - 10);
    ctx.lineTo(x - 7, y + 22);
    ctx.lineTo(x - 2, y + 50);  // thu hẹp đầu tip
    ctx.lineTo(x + 2, y + 50);
    ctx.lineTo(x + 7, y + 22);
    ctx.lineTo(x + 7, y - 10);
    ctx.closePath();
    ctx.fill();
    ctx.stroke();

    // Chất lỏng bên trong ống
    if (liquidColor) {
        ctx.fillStyle = liquidColor;
        ctx.beginPath();
        ctx.moveTo(x - 5, y);
        ctx.lineTo(x - 2, y + 45);
        ctx.lineTo(x + 2, y + 45);
        ctx.lineTo(x + 5, y);
        ctx.closePath();
        ctx.fill();
    }

    // Vệt sáng ống
    ctx.fillStyle = 'rgba(255,255,255,0.15)';
    ctx.fillRect(x - 6, y - 8, 3, 55);

    // Nhãn tên hóa chất
    if (label) {
        ctx.fillStyle = '#1A1A1A';
        ctx.font = "bold 12px 'JetBrains Mono', monospace";
        ctx.textAlign = 'center';
        ctx.fillText(label, x, y - 50);
    }

    ctx.restore();
}
```

Vẽ giọt đang rơi:
```js
function drawDrop(x, y, color) {
    ctx.save();
    ctx.fillStyle = color || 'rgba(200, 225, 245, 0.8)';
    ctx.beginPath();
    ctx.moveTo(x, y - 9);
    ctx.quadraticCurveTo(x - 5, y - 2, x - 4, y + 1);
    ctx.quadraticCurveTo(x, y + 5, x + 4, y + 1);
    ctx.quadraticCurveTo(x + 5, y - 2, x, y - 9);
    ctx.fill();
    // Điểm sáng
    ctx.fillStyle = 'rgba(255,255,255,0.4)';
    ctx.beginPath();
    ctx.arc(x - 1, y - 4, 1.5, 0, Math.PI * 2);
    ctx.fill();
    ctx.restore();
}
```

Điều kiện giọt chạm trong loop():
```js
if (isDropping) {
    dropY += 4;
    if (dropY >= TY + 55) {
        isDropping = false;
        hasReacted = true;
        updateExplanation();   // cập nhật khối Kết luận & trắc nghiệm (sidebar phải)
        updateAIGuide();       // cập nhật text thanh hướng dẫn AI
        updateObserveTable();  // điền giá trị vào Bảng quan sát nếu bước này có dữ liệu quan sát
    }
}
```

### F. Vẽ dụng cụ KHÁC (mô tả — không cần code cứng)
Khi thí nghiệm không dùng ống nghiệm + bút nhỏ giọt, hãy tự vẽ dụng cụ theo các nguyên tắc sau:

**Nguyên tắc vẽ thủy tinh (cốc, bình, ống...):**
•	Body: fillStyle rgba nhạt (opacity 0.3–0.5), strokeStyle '#94a3b8', lineWidth 2–2.5
•	Luôn có vệt sáng (highlight): đường trắng rgba(255,255,255,0.5–0.7) dọc bên trái, lineWidth 3–4
•	Đáy bo tròn bằng quadraticCurveTo
•	Miệng cốc/bình có gờ (lip): strokeStyle đậm hơn hoặc lineWidth lớn hơn

**Nguyên tắc vẽ chất lỏng bên trong:**
•	Dùng clip path theo hình dạng bình để chất lỏng không tràn ra ngoài
•	Mặt nước có sóng nhẹ: dùng Math.sin() với time variable để tạo wave
•	Gradient dọc: nhạt ở mặt trên, đậm hơn ở đáy (chỉ trong canvas — mô phỏng độ sâu thực)
•	Shimmer trên mặt: 1 lớp trắng mỏng rgba(255,255,255,0.3) ngay tại surface

**Nguyên tắc vẽ kim loại (điện cực, giá đỡ, kẹp...):**
•	Dùng createLinearGradient: sáng→tối→sáng để tạo cảm giác 3D trụ tròn
•	Màu: #4a5568 → #2d3748 → #1a202c (xám thép)
•	Viền highlight mỏng rgba(255,255,255,0.1) bên trái

**Nguyên tắc vẽ thiết bị điện (nguồn DC, LED...):**
•	Vỏ máy: gradient #2d3748 → #1a202c, bo góc 10px
•	Màn hình: nền tối #0d1117, chữ xanh lá #34d399 (font monospace)
•	LED: hình tròn nhỏ, có glow effect (shadowBlur 6–8) khi bật
•	Dây dẫn: xanh dương (#3b82f6) cho cực âm, đỏ (#ef4444) cho cực dương (quy ước vật lý — miễn trừ khỏi bảng token)

### G. Hiệu ứng đồ họa nâng cao (Visual Polish — bắt buộc áp dụng)

**Mặt chất lỏng (meniscus):**
•	Không vẽ đường thẳng ngang — dùng `quadraticCurveTo` với control point thấp hơn 2–4px ở tâm để tạo đường cong lõm tự nhiên
•	Shimmer overlay: lớp trắng mỏng `rgba(255,255,255,0.2)` ngay tại surface, y dao động ±1px theo `Math.sin(time * 2)`

**Bọt khí (bubbles):**
•	Kích thước tăng dần theo chiều cao: sinh ra ở đáy r=1–2px, lerp lên r=3–5px khi gần mặt nước theo tỉ lệ `(1 - distanceFromSurface / liquidHeight)`
•	Dao động ngang nhẹ: `x += Math.sin(time * 3 + index) * 0.3` mỗi frame

**Particle kết tủa / hạt bột:**
•	Wobble ngang khi chìm: `x += Math.sin(y * 0.15 + time) * wobbleAmp` (wobbleAmp = 0.3–0.8)
•	targetY mỗi particle lệch nhau ±5–10px (không để bằng nhau) → lớp cặn trông tự nhiên, không phẳng đều

**Hiệu ứng phản ứng (reaction flash):**
•	Frame đầu tiên khi `hasReacted = true`: vẽ vòng glow trắng/vàng tại điểm phản ứng — r tăng từ 0→30, opacity từ 0.6→0 trong ~0.3s (khoảng 18 frame)
•	Chỉ sau khi flash xong mới bắt đầu `lerpColor` từ từ — tránh cảm giác đổi màu đột ngột

**Splash khi giọt / stream chạm mặt nước:**
•	Spawn 3–5 micro-particle bắn tỏa từ điểm va chạm: `vx = (Math.random()-0.5)*3`, `vy = -(Math.random()*2+1)`, life giảm nhanh
•	Ripple ring: `arc()` với r tăng từ 2→12, `lineWidth` từ 2→0.5, opacity từ 0.6→0 trong ~0.4s (24 frame)

________________________________________
QUY TẮC ANIMATION (CỰC KỲ QUAN TRỌNG)

### Nguyên tắc #1: KHÔNG BAO GIỜ thay đổi trạng thái tức thì
Khi người dùng bấm nút, TUYỆT ĐỐI KHÔNG được nhảy thẳng đến kết quả.
SAI: `onClick → hasReacted = true; liquidColor = finalColor;`
ĐÚNG: `onClick → bắt đầu chuỗi animation → mỗi frame cập nhật progress → khi xong mới đổi state`

### Nguyên tắc #2: Mỗi thao tác = chuỗi animation nhiều pha
Mỗi thao tác thí nghiệm phải được chia thành các pha nhỏ, mỗi pha có progress 0→1:

| Thao tác | Pha animation bắt buộc | Thời gian tối thiểu |
|----------|----------------------|-------------------|
| Nhỏ giọt | ① Bút lerp đến vị trí → ② Giọt hình thành ở tip (r tăng 0→4) → ③ Giọt rơi với hình giọt nước thực (đầu tròn, đuôi nhọn lên, dùng quadraticCurveTo) → ④ Chạm mặt nước: spawn ripple ring + 3–5 micro-particle bắn tỏa | 1.5s |
| Rót dung dịch | ① Bình/ống được nâng và di chuyển đến bình nhận → ② nghiêng chậm → ③ stream bézier liên tục PHẢI xuất phát đúng từ miệng bình sau khi đã biến đổi theo góc quay; rộng 4–5px tại nguồn, có vệt phản sáng mảnh → ④ endpoint clamp trong miệng bình nhận → ⑤ ripple ring + micro-particles tại điểm va chạm → ⑥ mực nước dâng dần, màu khuếch tán dần → ⑦ chất rắn hình thành và lắng → ⑧ dòng rót dừng, bình nghiêng lại rồi trở về vị trí | 5–7s |
| Cho bột/chất rắn vào | ① Thìa/spatula xuất hiện với đống hạt nhìn rõ trên lòng thìa → ② di chuyển đến miệng bình → ③ thìa nghiêng và lượng chất rắn trên thìa giảm dần → ④ spawn khoảng 20–30 hạt riêng biệt có gravity, lệch X và wobble nhẹ → ⑤ hạt tích tụ/lắng tự nhiên ở đáy → ⑥ thìa rút ra | 2–3s |
| Khuấy/lắc | ① Đũa thủy tinh xuất hiện → ② Quay tròn (rotate animation) hoặc ống nghiệm lắc (oscillate góc ±5°) → ③ Particles di chuyển xoáy → ④ Màu dung dịch thay đổi dần (lerpColor) | 2s |
| Đun nóng | ① Vẽ đầy đủ đèn cồn gồm bình thủy tinh, cồn, nắp kim loại và bấc → ② ngọn lửa ba lớp xanh–vàng–cam xuất hiện, dao động bằng nhiều hàm `Math.sin()` và có quầng nhiệt nhẹ → ③ bọt khí nhỏ xuất hiện ở đáy → ④ bọt lớn dần, bay lên nhiều hơn → ⑤ hơi nước bay lên và fade | 3s |
| Làm lạnh | ① Chậu nước đá xuất hiện và ống nghiệm được hạ vào chậu → ② nhiệt kế giảm từ từ → ③ spawn các bông tuyết nhiều kích thước quanh chậu/ống nghiệm; mỗi bông chuyển động tỏa ra, xoay và fade dần → ④ giữ ống nghiệm ổn định trước thao tác kế tiếp | 3s |
| Lắp dụng cụ | ① Dụng cụ xuất hiện ở ngoài canvas (hoặc trên cao) → ② Di chuyển mượt đến vị trí đích (lerp + easeOut) → ③ Đến nơi, có hiệu ứng nhẹ (nhún, flash) | 1.5s |
| Nối dây điện | ① Đường dây vẽ dần từ cực nguồn đến điện cực (animated path, progress 0→1 theo tổng chiều dài) → ② Khi nối xong, glow nhẹ | 1.5s |
| Bật nguồn điện | ① LED sáng (glow) → ② Chấm sáng (electron dots) chạy dọc dây dẫn → ③ Bọt khí bắt đầu ở điện cực | 1s |
| Lọc / tách | ① Đổ hỗn hợp vào phễu (rót animation) → ② Giọt nhỏ rơi qua giấy lọc (drip particles) → ③ Bình nhận dung dịch dâng dần → ④ Kết tủa đọng lại trên phễu | 3s |

### Nguyên tắc #3: State machine cho mỗi bước
Mỗi bước thí nghiệm phải dùng subStep để quản lý trạng thái:
```
subStep = 0  →  Idle: chờ bấm nút, nút enabled
subStep = 1  →  Animating: animation đang chạy, TẤT CẢ nút disabled, animTimer tăng mỗi frame
subStep = 2  →  (Tùy chọn) Yêu cầu tương tác thêm: ví dụ "Nhấn Khuấy đều"
subStep = 3  →  Animating tương tác: animation khuấy/lắc đang chạy
subStep = 4  →  Done: animation xong, unlock bước tiếp hoặc hiện kết quả
```
Nếu bước chỉ có 1 animation đơn giản (không cần tương tác thêm), bỏ qua subStep 2-3, nhảy thẳng từ 1→4.

### Nguyên tắc #4: Timing & Easing
•	Di chuyển vật thể: dùng `easeOut` — nhanh lúc đầu, chậm dần khi đến đích
•	Đổi màu dung dịch: dùng `lerp` tuyến tính — đều đặn, tự nhiên
•	Lắc/quay: dùng `Math.sin(timer * speed) * amplitude` — dao động qua lại
•	Progress tăng mỗi frame: thường 0.015–0.04 (tức ~25–67 frame = 0.4–1.1 giây cho 1 pha)
•	Tổng thời gian 1 thao tác: 1.5–4 giây (90–240 frame @ 60fps)
•	Transition UI (nút, box, fade): 150–250ms, easing mượt (`ease` / `cubic-bezier(.4,0,.2,1)`)

### Nguyên tắc #5: Thứ tự vẽ mỗi frame (drawing order)
```
ctx.clearRect(0, 0, W, H);
drawGrid();                    // 1. Lưới nền mờ
drawStaticEquipment();         // 2. Dụng cụ cố định (giá đỡ, bàn, bình...)
drawLiquid();                  // 3. Chất lỏng bên trong bình (clip path)
drawParticlesInside();         // 4. Particles bên trong bình (kết tủa, bọt)
drawAnimatedObjects();         // 5. Vật đang di chuyển (bút nhỏ giọt, thìa, đũa khuấy, giọt rơi)
drawLabels();                  // 6. Nhãn, chỉ số
```
Vật di chuyển (dropper, spoon...) luôn vẽ TRÊN CÙNG để không bị che.

### Nguyên tắc #6: Visual polish — checklist bắt buộc
Mọi file PHẢI implement đủ các hiệu ứng sau, không bỏ qua:
•	**Reaction flash**: glow pulse tại điểm phản ứng khi `hasReacted` bật, TRƯỚC khi lerpColor
•	**Impact splash**: ripple ring + micro-particles mỗi khi giọt/stream chạm mặt nước
•	**Meniscus**: mặt chất lỏng luôn là đường cong lõm, không bao giờ phẳng ngang
•	**Bubble size gradient**: bubble nhỏ ở đáy, lớn dần khi gần surface
•	**Particle wobble**: hạt kết tủa chìm có dao động ngang `Math.sin()` tạo cảm giác lơ lửng
•	**Stream alignment**: endpoint X của dòng rót PHẢI clamp vào trong miệng bình nhận

### Nguyên tắc #7: Tôn trọng prefers-reduced-motion
```css
@media (prefers-reduced-motion: reduce) { ... }
```
•	UI: tắt mọi transition/animation trang trí (spin loader giữ lại vì mang thông tin trạng thái)
•	Canvas: tắt các hiệu ứng lặp vô hạn thuần trang trí (shimmer dao động, wave mặt nước) — GIỮ animation thao tác thí nghiệm (nhỏ giọt, rót, đổi màu) vì đó là nội dung sư phạm, nhưng có thể rút ngắn thời gian

________________________________________
CHECKLIST KỸ THUẬT (bắt buộc)

•	File HTML self-contained: toàn bộ CSS/JS inline trong 1 file, chỉ import font qua CDN (Google Fonts + Tabler Icons webfont). Ngoại lệ duy nhất: ảnh Athena tải trực tiếp từ `https://www.aiducation.edu.vn/athena/athena_idle.webp` — ngoài ra không phụ thuộc file ngoài nào khác.
•	`<meta charset="UTF-8">` và `<meta name="viewport" content="width=device-width, initial-scale=1">` — bắt buộc để tiếng Việt và mobile chuẩn.
•	Toàn bộ UI text bằng tiếng Việt, hiển thị đúng dấu, không lỗi font, không tràn chữ.
•	Responsive: chạy tốt từ 360px (mobile) tới desktop — test thực tế ở 3 mốc 375 / 768 / 1280px, không chỉ xem ở desktop rồi suy đoán.
•	Nút hành động chính (`.controls-card`) đã ghim đáy màn hình ở ≤767px (xem mục LAYOUT TỔNG THỂ — "Tối ưu mobile" mục 1).
•	Nếu kịch bản dùng tương tác trực tiếp trên canvas (xem mục VỊ TRÍ TƯƠNG TÁC): đã dùng `click` tap-to-toggle, không có `mouseenter`/`mouseleave` đứng một mình.
•	Accessibility cơ bản: contrast đủ, có `:focus` state rõ, dùng thẻ ngữ nghĩa (`<button>`, `<h1>`...).
•	KHÔNG dùng localStorage/sessionStorage — giữ state bằng biến JS trong session.
•	Chemical labels trong button text dùng Unicode subscripts (ví dụ `CH₃NH₂`); bên trong canvas `ctx.fillText` có thể dùng plain ASCII (`CH3NH2`) nếu font render hạn chế.

________________________________________
TÍCH HỢP LMS & ATHENA MANIFEST (bắt buộc — mọi file đều upload lên Aiducation)

Mọi file build từ prompt này đều được nhúng vào Aiducation LMS bằng iframe, nơi gia sư AI "Athena" hỗ trợ học sinh nhưng KHÔNG đọc được DOM/canvas bên trong file — mọi thứ Athena biết về bài đến từ đúng 1 nguồn: manifest + state phát ra dưới đây. Viết instrumentation này CÙNG LÚC với logic thật (gọi từ trong chính handler chuyển bước/chọn đáp án...), không phải lớp quan sát tách rời gắn thêm sau.

**Ràng buộc sandbox:** không gọi mạng ngoài `cdn.jsdelivr.net`, `fonts.googleapis.com`, `fonts.gstatic.com`, `aiducation.edu.vn`; không dùng `localStorage`/cookie (đã nêu ở CHECKLIST KỸ THUẬT) — nếu cần lưu tiến trình, lưu qua `LMS().state()`.

**1. Safe accessor** — dán đầu `<script>` đầu tiên, để file vẫn chạy được khi mở độc lập (Live Server) không lỗi `window.AiducationLMS is undefined`:
```js
function LMS(){return window.AiducationLMS||{ready:function(){},progress:function(){},event:function(){},state:function(){},complete:function(){},resize:function(){}};}
```

**2. Athena Manifest** — dán trong `<head>`:
```html
<script type="application/json" id="athena-context">
{
  "title": "...", "subject": "Hóa học", "grade": "...",
  "objectives": ["...", "..."],
  "structure": [ { "id": "exp1", "title": "..." } ],
  "athenaGuidance": "..."
}
</script>
```
`athenaGuidance` bắt buộc đủ 3 phần: (a) 1-2 câu thí nghiệm này làm gì, học sinh thao tác gì; (b) đánh số TỪNG câu hỏi trắc nghiệm trong "Kết luận & trắc nghiệm" kèm NGUYÊN VĂN lựa chọn đáp án, không tóm tắt, không lộ đáp án đúng; (c) quy tắc đứng — Athena chỉ gợi ý, không bao giờ nói thẳng đáp án đúng.

**3. Progress + Events** — gọi từ trong handler thật (vd khi `subStep` chuyển sang 4, hoặc khi học sinh chọn đáp án quiz):
```js
LMS().progress({ done, total }); // done = số bước/câu đã xong, total = tổng số bắt buộc
LMS().event('answered', { id:'q1', chosen:'B', correct:false });
```

**4. Completion** — bắn ĐÚNG 1 LẦN khi thí nghiệm + toàn bộ quiz đã hoàn thành (guard bằng biến boolean, vd `let completed = false`):
```js
LMS().complete({
  summary: "...", score: 0, max: 0,
  items: [ { id:"q1", prompt:"...", options:["A) …","B) …"], chosen:"...", correct:"...", isCorrect:true } ]
});
// Không lấy được giá trị nào từ kịch bản → dùng null, KHÔNG bịa câu hỏi/đáp án.
```

**5. Live State + resume** — gọi mỗi khi `subStep`/bước thí nghiệm hoặc đáp án đổi:
```js
LMS().state({ currentStep: subStep, totalSteps: 4, answeredSoFar: {q1:"B"}, lastAction:"..." });
if (window.AiducationLMS) window.AiducationLMS.onResume = function(state){ /* áp lại state */ };
```

**6. Resize** — báo chiều cao thật, tránh khoảng trắng thừa trước file/video kế tiếp trên LMS:
```js
function reportHeight() {
  const h = document.documentElement.scrollHeight;
  LMS().resize({ height: h });
}
window.addEventListener('load', reportHeight);
const ro = new ResizeObserver(() => { clearTimeout(window._rz); window._rz = setTimeout(reportHeight, 100); });
ro.observe(document.body);
```
Đồng thời KHÔNG để `body`/`html` có `min-height: 100vh` hay `height: 100vh` — chiều cao trang phải co theo đúng nội dung thật.

**Checklist LMS trước khi giao file:**
- [ ] Có `function LMS(){...}` safe accessor ở đầu script đầu tiên
- [ ] `#athena-context` hợp lệ, `structure[].id` khớp id dùng ở progress/complete/state
- [ ] `athenaGuidance` liệt kê ĐỦ mọi câu hỏi trắc nghiệm + lựa chọn, không lộ đáp án đúng
- [ ] `LMS().complete()` bắn đúng 1 lần, `results.items[]` khớp 1-1 câu hỏi thật trong bài
- [ ] `LMS().state()` gọi ở mọi thay đổi bước/đáp án có ý nghĩa + có `onResume`
- [ ] Đã gọi `LMS().resize()` lúc load + mỗi khi chiều cao nội dung đổi
- [ ] Không còn `<header>` banner trang trí trong file mới — chỉ có `.briefing` gộp mục tiêu và mô tả. Nếu là file cũ đang patch: `<header>` cũ đã ẩn bằng CSS, không xóa khỏi DOM.

________________________________________
KỊCH BẢN VÀ KIẾN THỨC CHO GAME:

Sự điện li — Phân loại chất điện li

STAGE 1  —  Mục tiêu & Giả thuyết
🎯  Mục tiêu Stage
Nêu được khái niệm sự điện li, chất điện li, chất không điện li.
🖥️  Canvas
Màn hình chia 2 phần:
— Phần trên: tiêu đề bài + đoạn mục tiêu 2–3 câu.
— Phần dưới: khung nền xanh nhạt hiển thị giả thuyết in đậm.
"Giả thuyết: Tất cả các chất có thể tan trong nước đều có khả năng dẫn điện khi hòa tan; mức độ dẫn điện phụ thuộc vào lượng chất tan."
Nút "Bắt đầu thí nghiệm" ở cuối màn hình.
🤖  Robot
"Trước khi bắt đầu, đây là giả thuyết chúng ta sẽ kiểm tra hôm nay."
"Đọc kỹ và ghi nhớ — ở cuối bài bạn sẽ cần đánh giá lại xem giả thuyết này đúng không!"
👆  Thao tác học sinh
Đọc mục tiêu và giả thuyết → nhấn nút "Bắt đầu thí nghiệm".
💡  Aha moment
Giả thuyết khoa học cần được kiểm chứng bằng thực nghiệm — không phải sự thật hiển nhiên.
▶  Điều kiện chuyển Stage
Nhấn nút "Bắt đầu thí nghiệm".
STAGE 2  —  Giới thiệu bộ thí nghiệm
🎯  Mục tiêu Stage
Học sinh làm quen với giao diện: 7 cốc, điện cực, bóng đèn.
🖥️  Canvas
7 cốc thủy tinh đặt hàng ngang, có nhãn tên chất bên dưới mỗi cốc:
(1) NaCl rắn  (2) dd NaCl  (3) dd HCl  (4) dd NaOH  (5) dd CH₃COOH  (6) dd đường  (7) dd ethanol
Mỗi cốc có 1 cặp điện cực treo phía trên, nối với bóng đèn riêng.
Tất cả đèn đang tắt. Mũi tên nhấp nháy chỉ vào điện cực cốc 1.
Bảng báo cáo (7 dòng trống) hiển thị bên phải màn hình.
🤖  Robot
"Đây là 7 mẫu chất bạn sẽ thử hôm nay."
"Mỗi cốc có một cặp điện cực — kéo điện cực xuống cắm vào cốc để kiểm tra tính dẫn điện."
"Quan sát bóng đèn và điền kết quả vào bảng. Bắt đầu thôi!"
👆  Thao tác học sinh
Quan sát giao diện → nhấn "Tiếp tục".
💡  Aha moment
7 mẫu chất khác nhau — kết quả có thể khác nhau bất ngờ.
▶  Điều kiện chuyển Stage
Nhấn "Tiếp tục" hoặc tự động sau 3 giây.
STAGE 3  —  Thử tính dẫn điện — Pha 1
🎯  Mục tiêu Stage
Học sinh tự thử tính dẫn điện 7 mẫu và ghi kết quả vào bảng báo cáo.
🖥️  Canvas
Giao diện giống Stage 2. Caption duy nhất ở đầu màn hình:
"Cắm điện cực vào từng cốc để thử tính dẫn điện. Quan sát bóng đèn và điền kết quả vào bảng."
Kết quả từng cốc khi cắm điện cực:
— Cốc 1 (NaCl rắn): đèn KHÔNG sáng.
— Cốc 2 (dd NaCl): đèn SÁNG MẠNH.
— Cốc 3 (dd HCl): đèn SÁNG MẠNH.
— Cốc 4 (dd NaOH): đèn SÁNG MẠNH.
— Cốc 5 (dd CH₃COOH): đèn SÁNG YẾU (~20% độ sáng so với cốc 2–4).
— Cốc 6 (dd đường): đèn KHÔNG sáng. Chú thích nhỏ bên cốc: "Đường tan hoàn toàn trong nước".
— Cốc 7 (dd ethanol): đèn KHÔNG sáng.
Sau khi cắm: điện cực giữ nguyên trong cốc, đèn hiển thị kết quả.
🤖  Robot
Không nói gì trong quá trình thử — chỉ nhắc khi HS bỏ qua mẫu (xem Phản hồi).
👆  Thao tác học sinh
Kéo/nhấn điện cực xuống cắm vào từng cốc (theo thứ tự hoặc tự chọn).
Quan sát đèn sau mỗi lần cắm.
Điền vào bảng báo cáo: cột "Hiện tượng đèn" (gợi ý 3 lựa chọn: Sáng mạnh / Sáng yếu / Không sáng) và cột "Dẫn điện?" (Có / Không).
⚠️  Phản hồi khi sai
HS nhấn "Tiếp tục" khi còn dòng trống trong bảng → thông báo: "Bảng báo cáo còn [X] cốc chưa thử."
HS điền khác hiện tượng đã thấy → gợi ý nhẹ: "Bạn có muốn xem lại cốc số [X] không?"
💡  Aha moment
7 mẫu cho ra 3 nhóm kết quả khác nhau — dữ liệu sẵn sàng để phân tích.
▶  Điều kiện chuyển Stage
Điền đủ 7 dòng bảng báo cáo (cả 2 cột).
STAGE 4  —  Câu hỏi nhóm 1 — NaCl rắn vs dd NaCl
🎯  Mục tiêu Stage
Học sinh tự rút ra: sự điện li cần môi trường nước → ion mới chuyển động tự do được.
🖥️  Canvas
Hệ thống highlight 2 dòng đầu bảng báo cáo (NaCl rắn và dd NaCl).
Hiển thị câu hỏi + 3 phương án bên dưới bảng.
Sau khi HS chọn: hiện giải thích đầy đủ (dù đúng hay sai).
Sau giải thích: xuất hiện nút "Xem vai trò của nước trong sự điện li NaCl".
Khi HS nhấn nút → Animation:
  · Mạng tinh thể NaCl với Na⁺ (tím) và Cl⁻ (xanh lá) xếp xen kẽ
  · Các phân tử H₂O (có ký hiệu δ⁺ ở đầu H, δ⁻ ở đầu O) tiến lại gần bề mặt tinh thể
  · Đầu δ⁻ của H₂O hướng vào Na⁺; đầu δ⁺ của H₂O hướng vào Cl⁻
  · Từng ion bị kéo ra khỏi mạng tinh thể, được bao quanh bởi các phân tử H₂O
  · Ion khuếch tán tự do vào dung dịch
  · Phương trình hiển thị: NaCl(aq) → Na⁺(aq) + Cl⁻(aq)
❓  Câu hỏi
"Cùng là NaCl nhưng NaCl rắn không dẫn điện còn dung dịch NaCl dẫn điện tốt. Điều này chứng tỏ điều gì?"
A. NaCl rắn không có ion; khi tan vào nước mới tạo thành ion Na⁺ và Cl⁻
B. Nước là chất dẫn điện tốt, làm cho dung dịch NaCl dẫn điện được
C. NaCl chỉ dẫn điện khi hòa tan vào nước vì lúc đó các ion mới có thể chuyển động tự do
→ Đáp án đúng: C
✅  Giải thích (hiện với cả đúng lẫn sai)
"Trong tinh thể NaCl, ion Na⁺ và Cl⁻ đã tồn tại nhưng bị giữ cố định trong mạng tinh thể — không thể di chuyển nên không dẫn điện."
"Khi hòa tan vào nước, các phân tử nước phân cực kéo ion ra khỏi mạng tinh thể và giải phóng chúng vào dung dịch → ion chuyển động tự do → dẫn điện."
"Lưu ý: phương án A sai vì ion đã có sẵn trong tinh thể NaCl rồi, không phải đến khi tan mới tạo ra."
🤖  Robot
"Bạn vừa quan sát thấy điều gì khác nhau giữa NaCl rắn và dung dịch NaCl? Hãy chọn câu trả lời đúng nhất."
💡  Aha moment
Ion đã có trong tinh thể NaCl — nước không tạo ra ion mà giải phóng ion để chúng chuyển động tự do.
▶  Điều kiện chuyển Stage
Trả lời câu hỏi + xem animation (hoặc bỏ qua animation).
STAGE 5  —  Câu hỏi nhóm 2 — dd NaCl, HCl, NaOH
🎯  Mục tiêu Stage
Học sinh tự rút ra đặc trưng của chất điện li mạnh: phân li gần như hoàn toàn.
🖥️  Canvas
Hệ thống highlight 3 dòng tương ứng trong bảng báo cáo.
Hiển thị câu hỏi + 3 phương án.
Sau khi HS chọn: hiện giải thích đầy đủ.
❓  Câu hỏi
"Ba dung dịch NaCl, HCl, NaOH đều làm đèn sáng mạnh. Điểm chung nào giải thích điều này?"
A. Cả ba đều là hợp chất vô cơ nên dẫn điện tốt
B. Cả ba đều phân li gần như hoàn toàn thành ion khi tan vào nước
C. Cả ba đều có khối lượng phân tử nhỏ nên dễ phân li
→ Đáp án đúng: B
✅  Giải thích (hiện với cả đúng lẫn sai)
"NaCl là muối, HCl là acid mạnh, NaOH là base mạnh — tuy thuộc loại hợp chất khác nhau nhưng điểm chung là đều phân li gần như hoàn toàn thành ion khi tan vào nước."
"Càng nhiều ion tự do trong dung dịch, dòng điện càng mạnh, đèn càng sáng. Đây là đặc trưng của chất điện li mạnh."
"Phương án A sai vì tính dẫn điện không phụ thuộc vào loại hợp chất (vô cơ hay hữu cơ) mà phụ thuộc vào khả năng phân li ra ion."
🤖  Robot
"Ba dung dịch này đều cho kết quả giống nhau. Điều gì ở cấp độ phân tử giải thích điểm chung đó?"
💡  Aha moment
Acid mạnh, base mạnh, muối → đều là chất điện li mạnh — phân li gần như hoàn toàn (→).
▶  Điều kiện chuyển Stage
Trả lời câu hỏi.
STAGE 6  —  Câu hỏi nhóm 3 — CH₃COOH vs HCl
🎯  Mục tiêu Stage
Học sinh tự rút ra: CH₃COOH phân li một phần → chất điện li yếu.
🖥️  Canvas
Hệ thống highlight dòng CH₃COOH, đặt cạnh dòng HCl để so sánh trực tiếp.
Hiển thị câu hỏi + 3 phương án.
Sau khi HS chọn: hiện giải thích đầy đủ.
❓  Câu hỏi
"CH₃COOH cũng là acid nhưng đèn sáng yếu hơn HCl rõ rệt. Nguyên nhân đúng nhất là gì?"
A. CH₃COOH có nồng độ thấp hơn HCl nên ít ion hơn
B. CH₃COOH chỉ phân li một phần — phần lớn vẫn tồn tại dưới dạng phân tử trong dung dịch
C. CH₃COOH là acid hữu cơ nên bản chất không dẫn điện tốt bằng acid vô cơ
→ Đáp án đúng: B
✅  Giải thích (hiện với cả đúng lẫn sai)
"HCl phân li gần như hoàn toàn (→): gần như không còn phân tử HCl nguyên vẹn trong dung dịch."
"CH₃COOH là acid yếu, phân li thuận nghịch (⇌): phần lớn tồn tại dưới dạng phân tử CH₃COOH, chỉ một phần nhỏ phân li ra ion H⁺ và CH₃COO⁻."
"Ít ion hơn → dòng điện yếu hơn → đèn sáng yếu hơn. Đây là chất điện li yếu."
"Phương án A sai vì hai dung dịch có cùng nồng độ trong thí nghiệm này."
🤖  Robot
"Cùng là acid nhưng kết quả lại khác nhau. Bạn nghĩ nguyên nhân là gì? Pha 2 sẽ cho bạn thấy rõ hơn ở cấp độ phân tử!"
💡  Aha moment
Không phải acid nào cũng điện li như nhau — mức độ phân li tạo ra sự khác biệt điện li mạnh/yếu.
▶  Điều kiện chuyển Stage
Trả lời câu hỏi.
STAGE 7  —  Câu hỏi nhóm 4 — Đường và ethanol
🎯  Mục tiêu Stage
Học sinh tự rút ra: tan được ≠ điện li → khái niệm chất không điện li.
🖥️  Canvas
Hệ thống highlight 2 dòng cuối bảng báo cáo, đặt cạnh dòng dd NaCl để so sánh.
Hiển thị câu hỏi + 3 phương án.
Sau khi HS chọn: hiện giải thích đầy đủ.
❓  Câu hỏi
"Đường tan hoàn toàn trong nước nhưng đèn không sáng. Kết luận nào đúng?"
A. Đường không tan trong nước nên không có hạt mang điện
B. Đường tan trong nước nhưng không phân li ra ion — phân tử đường vẫn nguyên vẹn trong dung dịch
C. Đường dẫn điện kém vì phân tử quá lớn, ion không thể di chuyển tự do
→ Đáp án đúng: B
✅  Giải thích (hiện với cả đúng lẫn sai)
"Đường saccharose tan rất tốt trong nước nhưng không phân li thành ion — phân tử C₁₂H₂₂O₁₁ tồn tại nguyên vẹn trong dung dịch."
"Không có ion → không có hạt mang điện tự do → không dẫn điện. Tương tự với ethanol."
"Đây là chất không điện li. Điểm mấu chốt: tan được KHÔNG có nghĩa là điện li."
"Phương án A sai vì thực tế đường tan rất tốt trong nước."
🤖  Robot
"Đường đã tan hoàn toàn — vậy tại sao đèn vẫn không sáng? Hãy suy nghĩ về điều gì thực sự cần thiết để dung dịch dẫn điện."
💡  Aha moment
Tan được ≠ điện li. Điều kiện để dẫn điện là phải có ion tự do — không phải chỉ cần tan.
▶  Điều kiện chuyển Stage
Trả lời câu hỏi.
STAGE 8  —  Kết luận
🎯  Mục tiêu Stage
Học sinh tự điền kết luận từ dữ liệu đã thu thập → hệ thống chốt kiến thức bản chất.
🖥️  Canvas
Phần 5A — Hiển thị câu kết luận với 5 ô trống:
"Quá trình ___(1)___ các chất trong nước tạo thành ___(2)___ được gọi là sự điện li. Những chất khi tan trong nước phân li ra ion được gọi là ___(3)___. Chất điện li mạnh phân li ___(4)___ hoàn toàn; chất điện li yếu chỉ phân li ___(5)___ một phần."
Đáp án: (1) phân li  (2) ion  (3) chất điện li  (4) gần như  (5) ra ion

Phần 5B — Sau khi HS điền xong, hiện đoạn chốt kiến thức:
"Sự điện li là quá trình phân li các chất trong nước tạo thành ion. Chỉ các chất phân li được ra ion mới làm dung dịch dẫn điện — đó là lý do NaCl rắn không dẫn điện nhưng dung dịch NaCl dẫn điện tốt: ion chỉ xuất hiện khi chất tan vào nước."
"Chất điện li mạnh (HCl, NaOH, NaCl...) phân li gần như hoàn toàn — phương trình dùng mũi tên một chiều (→). Chất điện li yếu (CH₃COOH...) chỉ phân li một phần — phương trình dùng mũi tên thuận nghịch (⇌)."
"Đường và ethanol tan trong nước nhưng không phân li ra ion — đây là chất không điện li. Điểm mấu chốt: tan được ≠ điện li."
🤖  Robot
"Dựa vào những gì vừa quan sát và phân tích, hãy điền vào chỗ trống để hoàn thành kết luận nhé!"
👆  Thao tác học sinh
Điền 5 từ khóa vào chỗ trống. Đọc đoạn chốt bài.
💡  Aha moment
Tan ≠ điện li. Điện li = phân li thành ion. Mức độ phân li → mạnh / yếu / không điện li.
▶  Điều kiện chuyển Stage
Điền đủ 5 ô trống.
STAGE 9  —  Pha 2 — Mô hình phân tử/ion HCl vs CH₃COOH
🎯  Mục tiêu Stage
Học sinh quan sát trực quan ở cấp độ vi mô sự khác biệt giữa điện li mạnh và điện li yếu.
🖥️  Canvas
Màn hình hiển thị 2 cốc song song:
— Trái: cốc HCl 0,1M, đèn sáng mạnh (từ Pha 1)
— Phải: cốc CH₃COOH 0,1M, đèn sáng yếu (từ Pha 1)
Mỗi cốc có điện cực và bóng đèn nối sẵn.
Caption: "Tại sao hai dung dịch acid này lại cho kết quả khác nhau? Nhấn Zoom để quan sát bên trong."

Khi HS nhấn nút ZOOM duy nhất → cả 2 cốc zoom đồng thời:
— Cốc HCl: gần như toàn bộ phân tử HCl tách thành H⁺ và Cl⁻ chuyển động tự do.
   Bộ đếm: "Ion: ~1994 / Phân tử còn lại: ~3"
   Phương trình: HCl → H⁺ + Cl⁻
— Cốc CH₃COOH: phần lớn vẫn là phân tử CH₃COOH nguyên vẹn, chỉ một phần nhỏ tách thành H⁺ và CH₃COO⁻.
   Bộ đếm: "Ion: ~6 / Phân tử còn lại: ~997"
   Phương trình: CH₃COOH ⇌ H⁺ + CH₃COO⁻
Độ sáng đèn tương ứng hiện bên dưới mỗi cốc để đối chiếu.
🤖  Robot
"Nhấn Zoom để thấy bên trong hai dung dịch acid này ở cấp độ phân tử. Chú ý đến số ion so với số phân tử còn lại ở mỗi cốc."
👆  Thao tác học sinh
Nhấn nút Zoom duy nhất → quan sát 2 animation chạy song song → đọc bộ đếm ion.
💡  Aha moment
HCl: phân li gần hoàn toàn → mũi tên một chiều (→).
CH₃COOH: phân li một phần, thuận nghịch → mũi tên hai chiều (⇌).
▶  Điều kiện chuyển Stage
Quan sát xong animation (tự động sau 6 giây) hoặc nhấn "Tiếp tục".
STAGE 10  —  Đánh giá giả thuyết
🎯  Mục tiêu Stage
Học sinh đối chiếu kết quả thực nghiệm với giả thuyết ban đầu — luyện tư duy khoa học.
🖥️  Canvas
Hiển thị lại giả thuyết ban đầu (nền xanh nhạt) đặt cạnh bảng kết quả thực nghiệm đã điền.
Ba nút lựa chọn: [Đúng] [Sai] [Đúng một phần]
Ô điền giải thích ngắn bên dưới.
Sau khi HS trả lời: hệ thống hiện phân tích (không phán xét đúng/sai câu trả lời của HS):
"Giả thuyết đúng một phần: Vế thứ nhất sai — đường và ethanol tan trong nước nhưng không dẫn điện vì không phân li ra ion. Vế thứ hai cũng chưa chính xác — mức độ dẫn điện phụ thuộc vào số lượng ion trong dung dịch, không đơn giản là lượng chất tan."
🤖  Robot
"Đây là giả thuyết bạn đã đọc trước khi bắt đầu. Dựa vào tất cả những gì vừa quan sát và phân tích, bạn đánh giá giả thuyết này như thế nào?"
👆  Thao tác học sinh
Chọn [Đúng / Sai / Đúng một phần] + điền giải thích ngắn → nhấn Xác nhận.
💡  Aha moment
Giả thuyết khoa học có thể đúng một phần — điều đó không phải thất bại mà là cơ sở để hoàn thiện hiểu biết.
▶  Điều kiện chuyển Stage
Nhấn Xác nhận sau khi chọn và điền giải thích.
STAGE 11  —  Câu hỏi MCQ — NL1
🎯  Mục tiêu Stage
Học sinh vận dụng kiến thức vừa xây dựng để trả lời 3 câu hỏi NL1.
🖥️  Canvas
3 câu MCQ lần lượt. Mỗi câu: hiện câu hỏi + 4 phương án → HS chọn → hiện giải thích → chuyển câu tiếp.
❓  Câu 1
"Dung dịch HCl và dung dịch CH₃COOH có cùng nồng độ, nhưng dung dịch HCl làm đèn sáng mạnh hơn. Nguyên nhân đúng nhất là:"
A. HCl có khối lượng phân tử nhỏ hơn CH₃COOH
B. HCl phân li hoàn toàn tạo nhiều ion hơn CH₃COOH trong cùng thể tích dung dịch  ← ĐÚNG
C. HCl là acid vô cơ còn CH₃COOH là acid hữu cơ nên tính dẫn điện khác nhau
D. CH₃COOH không phải chất điện li nên không dẫn điện
Đúng: "HCl phân li gần như hoàn toàn (→), ~997/1000 phân tử tạo ion; CH₃COOH phân li thuận nghịch (⇌), chỉ ~3/1000 phân tử tạo ion. Nhiều ion hơn → dòng điện mạnh hơn → đèn sáng hơn."
Gợi ý khi sai: "Nhìn lại animation Pha 2 — bộ đếm ion ở hai cốc chênh lệch bao nhiêu?"
❓  Câu 2
"Tinh thể NaCl không dẫn điện, nhưng dung dịch NaCl dẫn điện tốt. Điều này chứng tỏ:"
A. NaCl không phải chất điện li
B. Sự điện li của NaCl chỉ xảy ra khi NaCl được hòa tan vào nước  ← ĐÚNG
C. Nước là chất dẫn điện tốt nên làm cho dung dịch NaCl dẫn điện
D. NaCl rắn không có ion, chỉ khi đun nóng mới tạo ra ion
Đúng: "Trong tinh thể NaCl, ion Na⁺ và Cl⁻ đã tồn tại nhưng bị giữ cố định trong mạng tinh thể → không dẫn điện. Khi tan vào nước, phân tử nước phân cực kéo ion ra → ion chuyển động tự do → dẫn điện."
Gợi ý khi sai: "NaCl rắn có ion không? Điều gì thay đổi khi bạn thêm nước vào?"
❓  Câu 3
"Một học sinh cho rằng: 'Dung dịch đường không dẫn điện vì đường không tan trong nước.' Nhận định này:"
A. Đúng, đường khó tan nên không có ion
B. Sai về cả hai vế — đường tan tốt và có ion trong dung dịch
C. Đúng về kết quả nhưng sai về nguyên nhân — đường tan được nhưng không điện li nên không có ion  ← ĐÚNG
D. Đúng hoàn toàn
Đúng: "Đường saccharose tan rất tốt trong nước nhưng không phân li thành ion — phân tử đường tồn tại nguyên vẹn. Không có ion → không dẫn điện. Đây là chất không điện li: không phải không tan mà là tan nhưng không phân li."
Gợi ý khi sai: "Khi cho đường vào nước, đường có tan không? Nếu tan hoàn toàn mà đèn vẫn không sáng, lý do thực sự là gì?"
✅  Phản hồi MCQ
Đúng: hiện giải thích bản chất → chuyển câu tiếp.
Sai: hiện gợi ý hướng suy nghĩ (không lộ đáp án) → cho thử lại tối đa 2 lần → sau 2 lần mới hiện đáp án đúng + giải thích đầy đủ.
Vị trí đáp án đúng: Câu 1 = B, Câu 2 = B, Câu 3 = C — không trùng vị trí liên tiếp.
💡  Aha moment
Bản chất của sự điện li và phân loại chất điện li nằm ở mức độ phân li ra ion — không phải loại chất hay khả năng tan.
▶  Điều kiện chuyển Stage
Hoàn thành cả 3 câu MCQ.

CHECKLIST BÀN GIAO — ĐỘI BUILD
#
Hạng mục kiểm tra
Ghi chú kỹ thuật
1
Đủ 6 nội dung bắt buộc (Mục tiêu / Giả thuyết / Tiến trình / Bảng báo cáo / Kết luận / Câu hỏi)
✅
2
Giả thuyết do hệ thống đưa ra, học sinh chỉ đọc (Stage 1)
✅
3
Stage 3 (tiến trình Pha 1): KHÔNG có câu hỏi xen vào
✅
4
Phản hồi khi sai Stage 3: chỉ thông báo nhẹ, không phán xét
✅
5
Bảng báo cáo: hiện tượng → gợi ý 3 lựa chọn; dẫn điện → Có/Không. Không điền hộ.
✅
6
Stage 10 (đánh giá giả thuyết) đứng trước Stage 11 (MCQ)
✅
7
Câu hỏi nhóm 6A (Stage 4–7): 3 phương án, dù đúng hay sai đều hiện giải thích ngay, không cho chọn lại
✅
8
Animation Stage 4: mạng tinh thể NaCl + H₂O phân cực (δ⁺/δ⁻) + ion bị kéo ra + khuếch tán vào dung dịch
Tham khảo hình SGK đã cung cấp
9
Animation Stage 9: 2 cốc zoom đồng thời, bộ đếm ion HCl (~1994) vs CH₃COOH (~6), mũi tên → vs ⇌
Số liệu bám theo SGK: 0,1M, ~3/1000
10
MCQ Stage 11: sai → gợi ý → thử lại tối đa 2 lần → mới hiện đáp án. Vị trí đúng: B, B, C
✅
11
Robot: không giải thích thay simulation, tối đa 3 câu mỗi Stage
✅
12
Canvas Stage 9: màn hình chia đôi đủ rộng, bộ đếm ion hiển thị rõ ràng
Ưu tiên desktop layout



________________________________________
Hãy sinh ra toàn bộ code HTML/CSS/JS hoàn chỉnh, không cắt xén. Ưu tiên giao diện đẹp, tinh tế, nhất quán theo đúng design system "Haugomat editorial flat" trước khi nghĩ đến logic phức tạp.
