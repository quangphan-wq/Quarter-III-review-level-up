# Hướng dẫn dựng deck HTML: Level-up Defense (Q3 Review · Action Plan · Forecast Q4)

> **Gửi Claude Code.** File này là brief đầy đủ để dựng một bài trình bày **HTML một file, có animation, phong cách consulting chuyên nghiệp**. Mọi số liệu đã được kiểm tra với dữ liệu CRM. **Không tự thay đổi, làm tròn khác đi, bịa thêm số liệu hoặc thêm số liệu không có trong file này.**

---

## 0. Bối cảnh

| Mục | Nội dung |
|---|---|
| Người trình bày | **Phan Mạnh Quang (Aiden)**, Business Consultant Level 4, PKD 1.1 – Dreamers, RC01, Base.vn (Base Enterprise) |
| Mục đích | Review Q3/2026 → Action plan → Forecast Q4/2026 (commit **1 tỷ**), đồng thời **bảo vệ đề xuất lên Level 5, nhánh BC** |
| Người nghe | **DCEO**, Manager trực tiếp, đội **L&D** |
| Thời lượng | 45 phút: ~25 phút trình bày + ~20 phút Q&A |
| Người nghe quan tâm nhất | **DCEO: Action plan và Forecast Q4.** L&D: sự trưởng thành. Manager: kết quả và tính bền vững |
| Ngôn ngữ | Nội dung **tiếng Việt**. Nhãn nhỏ phía trên tiêu đề (eyebrow) bằng tiếng Anh in hoa, ví dụ `01 · REVIEW Q3`. Giữ nguyên thuật ngữ sales: Pipeline, Upsell, Showcase, SAL, MQL, ARPD, Commit, Upside |

### Câu chuyện xuyên suốt

> **"Q3 em đã làm được những điều này → nhờ đó em đã trưởng thành lên mức mới → đây là cách em sẽ làm trong Q4 → và đây là con số em cam kết: 1 tỷ."**

Mỗi tiêu đề slide là **một câu kết luận**, không phải nhãn chủ đề. Đọc liền các tiêu đề với nhau phải thành câu chuyện.

### Những điều KHÔNG được đưa lên slide

- **Không** hiển thị bất kỳ con số hay tỷ lệ % nào về lead bị thu hồi. Chỉ được nói định tính: *"số lượng lead bị thu hồi chưa được kiểm soát tốt"*.
- **Không** đưa phương án "tái ký thu hẹp" của DMS.
- **Không** có card "Em cần hỗ trợ".
- **Không** có chức năng in/xuất PDF.

---

## 1. Yêu cầu kỹ thuật

1. **Một file `index.html` tự chứa** (CSS + JS inline). Chỉ được tải font từ Google Fonts. Không dùng framework, không build step. Nếu thật sự cần thư viện animation, chỉ dùng GSAP từ `cdnjs.cloudflare.com`. Ưu tiên CSS/Web Animations API thuần.
2. **Canvas 16:9 cố định 1920×1080**, scale vừa khung cửa sổ (letterbox), không vỡ layout ở mọi kích thước màn hình.
3. **Điều hướng:**
   - `→` `Space` `PageDown` `click`: bước tiếp (từng build trong slide, hết build thì sang slide mới)
   - `←` `PageUp`: lùi
   - `Home` / `End`: slide đầu / cuối
   - `F`: fullscreen
   - `O`: overview, lưới thumbnail để nhảy nhanh tới slide (quan trọng khi Q&A)
   - `N`: mở cửa sổ **speaker notes** kèm đồng hồ đếm giờ và slide kế tiếp
   - `A`: nhảy tới Phụ lục
   - Hash URL `#/5` để mở thẳng một slide
4. **Thanh tiến độ** mảnh ở đáy màn hình và số trang ở footer.
5. Tôn trọng `prefers-reduced-motion`: tắt chuyển động, chỉ giữ fade nhanh.
6. **Mọi biểu đồ vẽ bằng SVG inline**, tự viết, có animation. Không dùng ảnh chụp biểu đồ.
7. **Quy tắc trình bày số liệu:** mọi tỷ lệ chuyển đổi và mọi tỷ trọng theo nguồn **phải thể hiện bằng biểu đồ** (funnel, donut, bar), không chỉ ghi số dạng chữ.
8. Số liệu đặt trong **một object JS `DATA` ở đầu file** để dễ sửa, không rải rác trong HTML.
9. Ký tự tiếng Việt phải hiển thị đúng dấu ở mọi cỡ chữ.
10. Thẻ `<title>`: `Level-up Defense · Phan Mạnh Quang`

---

## 2. Hệ thống thiết kế (lấy từ deck H1 2026 của Aiden, nâng cấp)

### Màu

```css
:root{
  --navy-900:#0F1F3D;  /* nền slide mở/chương/kết */
  --navy-800:#16294D;
  --navy-700:#1A325F;
  --blue-600:#2F5BA8;  /* accent chính: Base blue */
  --blue-500:#3D6FD1;  /* accent sáng hơn cho số liệu nổi bật, chart */
  --blue-300:#6F86AD;  /* chữ phụ trên nền tối */
  --blue-100:#DCE6F7;
  --card:#F3F6FC;      /* nền card trên slide trắng */
  --line:#D3DAE8;      /* viền card, kẻ bảng */
  --ink:#16223F;       /* chữ chính */
  --muted:#5B6B8C;     /* chữ phụ */
  --good:#1F9D6B;      /* đạt / tăng */
  --warn:#E0A100;      /* cần chú ý */
  --bad:#D2483F;       /* hụt / rủi ro */
}
```

- Slide nội dung: nền trắng, card nền `--card` bo góc 16px, viền 1px `--line`, shadow rất nhẹ.
- Slide cover, chương, kết: nền `--navy-900`, có 1–2 vòng tròn lớn mờ (gradient navy) ở góc, giống deck H1.
- Màu xanh/vàng/đỏ **chỉ dùng cho trạng thái**, không dùng để trang trí.
- Biểu đồ nhiều nhóm (donut theo nguồn): dùng các sắc độ trong dải navy → blue (`--navy-800`, `--blue-600`, `--blue-500`, `--blue-300`), không dùng cầu vồng.

### Chữ

- **Poppins** (400/500/600/700) cho toàn bộ deck.
- Eyebrow: 14px, 600, chữ in hoa, `letter-spacing: .18em`, màu `--blue-600`.
- Tiêu đề slide: 44–52px, 700, `--ink`, tối đa 2 dòng.
- Subtitle: 20px, 400, `--muted`.
- Số KPI lớn: 56–72px, 700, `tabular-nums`.
- Nội dung: tối thiểu 18px. **Không chữ nào dưới 14px** (phòng họp chiếu xa).

### Bố cục

- Lề 96px trái/phải, 72px trên. Lưới 12 cột.
- Footer: trái `BASE.VN · LEVEL-UP DEFENSE`, phải là số trang, 12px, `--muted`.
- Icon: SVG line icon tự vẽ inline (phong cách Lucide), nằm trong vòng tròn `--navy-800`, icon màu trắng, giống deck H1.
- Mỗi slide **một ý chính**, tối đa 3–4 khối.

---

## 3. Animation (chuyên nghiệp, tiết chế)

**Nguyên tắc:** animation để dẫn mắt người xem theo đúng thứ tự câu chuyện, không để trang trí. Easing `cubic-bezier(.2,.7,.2,1)`, thời lượng 400–700ms, không bounce, không xoay.

| Thành phần | Hiệu ứng |
|---|---|
| Chuyển slide | Slide cũ fade + dịch trái 40px, slide mới fade + dịch từ phải 40px (500ms). Slide chương (nền navy): wipe/mask từ trái sang |
| Eyebrow → tiêu đề → subtitle | Lần lượt fade-up 16px, cách nhau 80ms |
| Card / khối | Stagger fade-up, cách nhau 90ms |
| Số KPI | **Count-up** từ 0 tới giá trị trong 900ms, định dạng kiểu Việt (`701M`, `1.003M`, `132%`) |
| Cột biểu đồ | Mọc từ trục 0 lên, stagger theo tháng. Nhãn giá trị fade-in sau khi cột mọc xong |
| Đường biểu đồ | Vẽ dần bằng `stroke-dashoffset`. Các điểm pop-in theo sau |
| Donut | Các cung vẽ dần theo chiều kim đồng hồ, nhãn % fade-in sau |
| Thanh tiến độ / % | Width tăng từ 0, con số chạy song song |
| Funnel | Từng tầng trượt vào từ trên xuống, nhãn % chuyển đổi giữa các tầng hiện sau |
| Bảng | Từng hàng fade-in. Hàng cần nhấn mạnh có nền `--blue-100` quét từ trái sang |
| Badge trạng thái ✓ | Scale 0.8 → 1 kèm fade |
| Quote kết | Từng chữ (word-by-word) fade-up chậm, cách nhau 120ms |
| Build theo click | Các slide có ghi **[BUILD]** thì từng phần chỉ xuất hiện khi bấm, để người trình bày kể từng ý |

---

## 4. Số liệu gốc (đặt vào `DATA`)

```js
const DATA = {
  q3: {
    revenue: 701.3,            // triệu, Total Revenue Q3 (con số chính dùng toàn deck)
    perfRevenue: 690.9,        // triệu, Revenue tính performance (chỉ dùng ở slide L5)
    commit: 800,
    achievePct: 87.7,          // 701.3/800
    dealsWon: 15,
    arpd: 46.8,                // 701.3/15
    months: [                  // theo tháng ghi nhận performance
      { m: 'T7', actual: 241.9, commit: 300 },
      { m: 'T8', actual: 149.4, commit: 200 },
      { m: 'T9', actual: 306.8, commit: 300 }
    ],
    mixRevenuePct: { 'Up/cross': 36.9, 'CSM': 23.5, 'Marketing': 22.9, 'Partner': 16.7 },
    mixDealsPct:   { 'Up/cross': 40.0, 'CSM': 40.0, 'Marketing': 13.3, 'Partner': 6.7 },
    existingShare: { revenuePct: 60.4, deals: 12, of: 15 },
    newMeetings: 33, newMeetingTarget: 45,
    rank: { juniorBC: '8/54', rchn: 9, company: 22 }
  },
  quarters: [                  // xu hướng doanh thu theo quý (triệu)
    { q: 'Q4/25', v: 442 }, { q: 'Q1/26', v: 341 },
    { q: 'Q2/26', v: 542 }, { q: 'Q3/26', v: 701 }
  ],
  months2026: [193.2, 0, 148.0, 9.8, 364.7, 167.3, 241.9, 149.4, 306.8], // T1–T9
  avg9m: 176,
  perfRatingL4: { T4:'Dưới tối thiểu', T5:'Outperformance', T6:'Goal', T7:'Outperformance', T8:'Goal', T9:'Outperformance' },
  year: { goal: 2300, ytd: 1584, ytdPct: 68.9, neededQ4: 716, withQ4: 2584, withQ4Pct: 112 },
  l4: { backup: 70, kpi: 100, goal: 145, outperf: 190 },
  l5: { backup: 90, kpi: 125, goal: 175, outperf: 250 },
  funnelNew: [                 // phễu new sales Q3 (không tính khách hiện hữu)
    { s: 'Lead', n: 107 },     // tổng lead Q3 theo báo cáo nguồn
    { s: 'New meeting', n: 33 },
    { s: 'Deal new won', n: 3 } // Biogreen, Ba Vì (Marketing) + Máy tính Trần Hạnh (Partner)
  ],
  conversion: {                // %
    leadToMeeting: 30.8,       // 33/107
    meetingToWon: { H1: 8.8, Q3: 9.1 }, // 3/33
    leadToWon: 2.8             // 3/107
  },
  q4: {
    commit: 1000,
    months: [
      { m: 'T10', target: 250, items: [['LPH', 66], ['Deal nhỏ', 70], ['New sales', 114]] },
      { m: 'T11', target: 300, items: [['DMS tái ký', 250], ['Upsell/New', 50]] },
      { m: 'T12', target: 453, items: [['Diamond Plaza', 143], ['Pearl River', 310]] }
    ],
    floor: 529,                // nền + DMS, ≥ Goal L5 cả quý (525)
    upside: ['Draho tái ký sớm 500 (thay DMS nếu DMS trượt)', 'LiteOn ~1.000', 'Truyền dẫn Long Biên (chưa xác định dealsize)']
  }
};
```

---

## 5. Nội dung từng slide (21 slide chính + phụ lục)

Ký hiệu: **Eyebrow** · **Tiêu đề** · *Subtitle* · Nội dung · Animation · 🎤 Speaker notes (hiển thị ở cửa sổ `N`, không hiện trên slide).

### S1 · Cover (nền navy)
- Eyebrow: `BASE.VN · BUSINESS CONSULTANT`
- Tiêu đề: **Level-up Defense**
- Subtitle: *Q3 Review · Action Plan · Forecast Q4 — Đề xuất lên Level 5, Middle Business Consultant*
- Góc dưới trái: Phan Mạnh Quang · Business Consultant · PKD 1.1 Dreamers · 10/2026
- Phải: đồ họa trừu tượng gồm 4 cột tăng dần (tỷ lệ theo 442 → 341 → 542 → 701, không ghi số), mọc lên khi mở slide.
- 🎤 "Đầu năm em đặt mục tiêu lên L5 vào tháng 10. Hôm nay em xin trình bày kết quả của hành trình đó."

### S2 · Câu chuyện hôm nay
- Eyebrow: `AGENDA` · Tiêu đề: **4 câu hỏi em sẽ trả lời hôm nay**
- 4 card đánh số, stagger:
  1. **Review Q3**: Quý vừa rồi em làm được gì?
  2. **Trưởng thành**: Em đã sẵn sàng cho L5 chưa?
  3. **Action plan**: Q4 em sẽ làm gì khác?
  4. **Forecast Q4**: 1 tỷ đến từ đâu?
- Dưới cùng, chữ nhỏ: *25 phút trình bày · 20 phút trao đổi*

---

### CHƯƠNG 01 · REVIEW Q3

### S3 · Slide chương (nền navy)
`01 · REVIEW Q3` · **Em đã làm được gì**

### S4 · Kết quả Q3 so với cam kết
- Eyebrow: `01 · REVIEW Q3`
- Tiêu đề: **Q3 đạt 701M, 88% cam kết, tháng 9 về đích vượt kế hoạch**
- Trái: 4 KPI card count-up: **701M** doanh thu · **87,7%** cam kết 800M · **15** deal won · **46,8M** ARPD
- Phải: biểu đồ cột đôi theo tháng, Commit (viền xanh nhạt, rỗng) và Actual (xanh đặc): T7 242/300 · T8 149/200 · **T9 307/300 ✓**. Nhãn % đạt trên mỗi cặp: 81% · 75% · **102%**
- Dải nhỏ dưới cùng: Xếp hạng **#8/54 Junior BC · #9 RCHN · #22 toàn công ty**
- [BUILD] KPI → biểu đồ → dải xếp hạng
- 🎤 Nói thẳng phần hụt 12% trước, rồi nhấn mạnh tháng 9 đã bắt kịp và vượt kế hoạch.

### S5 · Bốn quý và nhịp ổn định
- Tiêu đề: **Doanh thu tăng 2 quý liên tiếp, 5 tháng liền đạt Goal trở lên**
- Trái: line chart theo quý 442 → 341 → 542 → **701**, vẽ dần. Thêm đường ngang nét đứt "Goal L5/quý = 525M"; Q3 nằm trên đường này.
- Phải: dải 6 ô "Xếp hạng performance hằng tháng (L4)" T4–T9: Dưới tối thiểu (đỏ nhạt) · Outperformance · Goal · Outperformance · Goal · Outperformance (xanh). Các ô lật lần lượt.
- Chú thích: *Trung bình 9 tháng: **176M/tháng**, vượt Goal L5 (175M)*
- 🎤 Đây là câu trả lời cho câu hỏi "có duy trì được phong độ không".

### S6 · Doanh thu đến từ đâu
- Tiêu đề: **Khách hiện hữu đóng góp 60% doanh thu, nền ổn định cho năng lực new sales**
- Hai **donut chart** cạnh nhau:
  - Trái: **Tỷ trọng doanh thu theo nguồn**: Up/cross 36,9% · CSM 23,5% · Marketing 22,9% · Partner 16,7%. Giữa donut: **701M**
  - Phải: **Tỷ trọng số deal won theo nguồn**: Up/cross 40% · CSM 40% · Marketing 13,3% · Partner 6,7%. Giữa donut: **15 deal**
- Dưới hai donut, một thanh ngang 100% tách **Khách hiện hữu 60%** / **Khách mới 40%**
- Chú thích nhỏ: *Tỷ trọng khách hiện hữu tăng do nhận bàn giao account AM/CSM. H1/2026 new sales chiếm ~74% doanh thu.*
- [BUILD] donut trái → donut phải → thanh tổng hợp
- 🎤 Chủ động giải thích trước để không bị hiểu là new sales yếu đi.

### S7 · Làm tốt và cần cải thiện
- Tiêu đề: **Thế mạnh đã rõ, điểm nghẽn cũng đã rõ**
- Hai cột:
  - **Làm tốt (xanh)**
    - Đạt ~90% mục tiêu, tháng 9 vượt cam kết
    - Doanh thu trung bình ổn định: 176M/tháng trong 9 tháng
    - 15 deal won, nhịp chốt đều
    - Khai thác hiệu quả nguồn up/cross và CSM
  - **Cần cải thiện (vàng)**
    - New meeting còn thấp, nên new sales còn thấp. Biểu đồ dạng bullet/progress bar: **33 / 45 new meeting** (73%)
    - Số lượng lead bị thu hồi chưa được kiểm soát tốt, **chưa giữ được chuẩn cơ bản** *(chỉ ghi chữ, không có số)*
- [BUILD] cột trái → cột phải

### S8 · Phễu new sales
- Tiêu đề: **107 lead ra 3 deal new: phễu nghẽn ở đầu vào, chất lượng sau meeting giữ vững**
- **Funnel chart** 3 tầng từ `DATA.funnelNew`: **107 lead → 33 new meeting → 3 deal new won**. Giữa các tầng hiển thị % chuyển đổi: Lead → Meeting **30,8%** · Meeting → Won **9,1%**. Dưới đáy phễu: Lead → Won **2,8%**.
- Chú thích 3 deal new: Biogreen, CP TM Ba Vì (Marketing) · Máy tính Trần Hạnh (Partner)
- Hai chỗ nghẽn khoanh viền vàng, có chú thích:
  - ① **Đầu phễu (Lead → Meeting)**: chất lượng lead đầu vào thấp hơn do qualify chặt hơn (vấn đề chung của RC), cộng thêm lead chưa được chăm sóc đủ nhanh và đủ đều → *Action 1: Back to basic · Action 3: 15 new meeting/tháng*
  - ② **Sau meeting (Meeting → Won)**: cần tăng tỷ lệ chuyển meeting thành deal → *Action 4: deck & demo riêng cho từng khách*
  - **Không** ghi chữ "thu hồi" hay bất kỳ số nào về lead thu hồi trên slide này.
- Bên phải, **bar chart so sánh** Meeting → Won: H1 **8,8%** vs Q3 **9,1%** (mũi tên tăng nhẹ). Chú thích: *Chất lượng sau meeting giữ vững dù lead đầu vào khó hơn.*
- [BUILD] funnel → nghẽn ① → nghẽn ② → bar chart so sánh
- 🎤 Đầu phễu rớt nhiều chủ yếu do lead được qualify chặt hơn, đây là xu hướng chung của RC. Phần em kiểm soát được là tốc độ chăm sóc lead và số meeting. Mỗi chỗ nghẽn có một action tương ứng ở chương 3.

### S9 · Deal tiêu biểu Q3
- Tiêu đề: **Ba deal, ba cách thắng khác nhau**
- 3 card lớn (icon · tên khách · giá trị count-up · nguồn · "vì sao thắng"):
  1. **Nebecom**: **179M** (3 đợt upsell, lớn nhất 156,8M) · Up/cross
     *Mở rộng từ nhu cầu hiện hữu: sau triển khai, khách dùng rất active nên nhu cầu mới tự xuất hiện.*
  2. **Máy tính Trần Hạnh**: **117,1M** · Partner
     *Chốt nhanh gọn: 6 ngày từ lúc tạo deal đến khi chốt. Always keep it hot.*
  3. **Biogreen (Hóa dược & CNSH)**: **108,8M** · Marketing
     *Tối đa hóa giá trị deal: phần mềm ~54M và triển khai ~54M, bài toán quản lý quy trình sản xuất theo đơn.* Có thể thêm một mini bar chia đôi 50/50 phần mềm/triển khai trong card.
- 🎤 Cầu nối sang chương 2: những deal này cho thấy em đã thay đổi cách làm việc.

---

### CHƯƠNG 02 · TRƯỞNG THÀNH

### S10 · Slide chương (nền navy)
`02 · TRƯỞNG THÀNH` · **Em đã sẵn sàng cho Level 5**

### S11 · Em đã thay đổi thế nào
- Tiêu đề: **Từ người bán hàng thành người tư vấn chốt được deal ở mọi nguồn**
- Hai card lớn "Trước → Nay":
  1. **Chốt deal AM & CSM**: Trước chủ yếu chốt new sales → Nay chốt được cả upsell và tái ký: **425M từ khách hiện hữu trong Q3**
  2. **Làm việc khoa học hơn**: recap mọi meeting, review deal định kỳ, ra quyết định dựa trên số liệu CRM, ứng dụng AI vào quy trình
- Card case phía dưới (giữ card này): **Lotte (OOM) → Diamond Plaza**: *Tư vấn hệ thống KPI cho OOM, từ đó OOM chỉ định Diamond Plaza làm việc với Base (chỉ định thầu, 143M).* Đây là ví dụ tư vấn tốt tạo ra deal tiếp theo. Vẽ thành sơ đồ nhỏ 2 node có mũi tên OOM → Diamond Plaza.
- 🎤 Slide này dành cho L&D.

### S12 · Đối chiếu tiêu chí Level 5 (chính sách v5.1)
- Tiêu đề: **Đáp ứng đầy đủ điều kiện thăng cấp chuẩn lên Level 5**
- Bảng 3 cột: Tiêu chí · Yêu cầu · Thực tế · (badge ✓ pop-in)
  1. Tổng doanh thu 3 tháng gần nhất ≥ Goal L5 · **≥ 525M** (3 × 175M) · **690,9M performance (132%)** ✓
  2. Tối đa 1 tháng dưới KPI L4 · **< 100M tối đa 1 tháng** · **0 tháng** (thấp nhất T8: 149M) ✓
- Dưới bảng: **bar chart ngang** 690,9M so với vạch mốc 525M (Goal L5 cả quý), thanh tràn vượt vạch
- 3 chip bằng chứng bổ sung: *TB 9 tháng 176M ≥ Goal L5* · *7/9 tháng ≥ KPI L5* · *Nếu Q3 đã ở L5: Goal / KPI / Outperformance, không tháng nào dưới KPI*
- 🎤 Đây là slide quan trọng nhất với hội đồng. Nói chậm.

### S13 · Lộ trình & hướng 2027
- Tiêu đề: **Lên L5 đúng kế hoạch đặt ra từ đầu năm và bước tiếp theo**
- Timeline ngang, đường kẻ vẽ dần, node pop-in:
  1. **T1/2026**: Đặt mục tiêu lên L5 vào T10 (kế hoạch năm)
  2. **T10/2026: Level 5 · Middle BC** (node nổi bật)
  3. **Incubator Program**
  4. **Build team riêng** (sau Incubator)
- Card phụ **"Thị trường mới (giai đoạn khám phá)"**: *Tiên phong mở thị trường ngoài Việt Nam cho Base (Malaysia, Philippines, Nhật Bản, Jordan…) qua LinkedIn. Rework đã làm, Base chưa có người.* Ghi rõ **không phải mảng chính, không ảnh hưởng mục tiêu Q4**.

---

### CHƯƠNG 03 · ACTION PLAN

### S14 · Slide chương (nền navy)
`03 · ACTION PLAN` · **Q4 em sẽ làm gì khác**

### S15 · Bốn hành động trọng tâm
- Tiêu đề: **4 hành động, mỗi hành động gắn một chỉ số đo được**
- 4 card dọc. Mỗi card gồm: số thứ tự · tên · việc cụ thể · **chỉ số** (chip màu) · nhãn nhỏ nối với chỗ nghẽn ở S8
  1. **Back to basic: không để lead nguội**
     - Tuân thủ tuyệt đối cơ chế: gọi trong **4h** · **≥5 call trong 6 ngày** · **book meeting trong 15 ngày**
     - Chỉ số: **giảm tối đa số lead bị thu hồi** *(không ghi con số)* · nối với nghẽn ①
  2. **Tìm lại điểm chạm với khách hàng**
     - Email sequence 3 email về **hệ thống 2.0 tích hợp AI**, chia theo 4 nhóm bài toán: quản lý công việc · nhân sự/chấm công/lương · bán hàng/CRM · văn bản/E-Office
     - **Chương trình ưu đãi cho trường học**: đào lại lead trường học, gọi lại toàn bộ
  3. **Tăng tỷ lệ new sales (quan trọng nhất)**
     - **15 new meeting/tháng**. Mini bar chart: Q3 ~11/tháng → mục tiêu 15/tháng
     - Đảm bảo số lượng deal mới trong quý
  4. **Ứng dụng AI vào công việc** (chi tiết ở S16) · nối với nghẽn ②
- Dải dưới cùng: **Nhịp theo dõi**: tự review deal hằng tuần
- [BUILD] lần lượt từng card

### S16 · AI trong quy trình bán hàng
- Tiêu đề: **AI có mặt ở mọi bước của một deal, giúp em có thêm ~22 giờ mỗi tháng để gặp khách**
- Trên cùng bên trái, số lớn count-up: **2 giờ → 30 phút** cho mỗi meeting recap (**−75%**)
  - Chú thích: *~15 meeting/tháng ≈ 22 giờ tiết kiệm, gần 3 ngày làm việc*
- Phần chính: **sơ đồ dòng chảy ngang theo hành trình deal**, 6 chặng, mỗi chặng là một cột có các use case AI bên dưới. Node sáng lần lượt từ trái sang phải.
  - Node **đặc** (đậm) = đang dùng. Node **viền** = mở rộng trong Q4. Có chú giải ở góc.

  | Chặng | Use case AI | Trạng thái |
  |---|---|---|
  | **1. Trước meeting** | Brief nghiên cứu khách hàng 1 trang (ngành, quy mô, tin tức, phần mềm đang dùng) + bộ câu hỏi discovery | Mở rộng Q4 |
  | **2. Meeting** | **Recap call & meeting** | Đang dùng |
  | **3. Sau meeting** | **Hệ thống demo riêng** cho từng khách · **Slide pitching riêng** | Đang dùng |
  | **4. Đề xuất & đàm phán** | **Proposal & báo giá** · Phân tích yêu cầu/hồ sơ thầu → bảng đáp ứng tính năng (ví dụ Diamond Plaza, NĐ 355 cho DMS) · Tài liệu đa ngôn ngữ EN/KR cho khách FDI | Proposal & báo giá: đang dùng · Còn lại: mở rộng Q4 |
  | **5. Chốt & bàn giao** | Tài liệu hand-off cho đội triển khai (bài toán, phạm vi, kỳ vọng của khách) | Mở rộng Q4 |
  | **6. Khách hiện hữu** | Báo cáo "nhìn lại 1 năm sử dụng" làm cơ sở cho tái ký và upsell (DMS, Draho) | Mở rộng Q4 |

- Một dải mảnh chạy ngang dưới cả 6 chặng: **Review pipeline hằng tuần**: AI đọc dữ liệu export từ CRM, đánh dấu deal đứng yên lâu và lead sắp quá hạn chăm sóc · Mở rộng Q4
- **Không** đưa use case email cá nhân hóa (email đang chạy bằng automation của công ty).
- [BUILD] số tiết kiệm → lần lượt từng chặng → dải review pipeline

---

### CHƯƠNG 04 · FORECAST Q4 (để cuối cùng)

### S17 · Slide chương (nền navy)
`04 · FORECAST Q4` · **1 tỷ đến từ đâu**

### S18 · Mục tiêu Q4: 1 tỷ
- Tiêu đề: **Q4 commit 1 tỷ: vượt goal năm và giữ nhịp Outperformance L5**
- Trái, 3 KPI: **1.000M** commit Q4 · **2,58B** cả năm (**112%** goal 2,3B) · **333M/tháng** (> Outperformance L5 250M)
- Phải: **stacked bar chart** theo tháng **T10 250 · T11 300 · T12 453**, mỗi cột chia lớp theo nguồn trong `DATA.q4.months[].items` (có nhãn tên từng lớp). Đường ngang nét đứt KPI L5 (125) và Goal L5 (175)
- Dưới: **thanh tiến độ năm**: YTD 1.584M (68,9%) + Q4 1.000M → 2.584M, vạch mốc goal 2.300M. Chú thích: *Chỉ cần 716M trong Q4 để đạt goal năm.*
- [BUILD] KPI → cột T10 → T11 → T12 → thanh tiến độ năm
- 🎤 T10 thấp nhất vì chưa có deal lớn về. New sales ~114M là quay lại nhịp new sales của H1 (~109M/tháng).

### S19 · Danh sách deal Q4 (slide DCEO sẽ dừng lâu nhất)
- Tiêu đề: **1 tỷ = nền + DMS + Pearl River, có Draho dự phòng và LiteOn là upside**
- Bảng nhóm theo tầng, mỗi tầng có thanh màu bên trái:
  - **NỀN (xanh đậm)**
    - LPH (Lotte Properties Hanoi) · 66M (30% của 221M, phần còn lại thu sau triển khai) · T10
    - Deal nhỏ (Viện A, XNNL, M7) · ~70M · T10, ghi gộp, không liệt kê từng deal
    - **Diamond Plaza** · 143,4M · chốt T11, tiền về T12 · *OOM chỉ định thầu, đang thống nhất scope*
    - New sales/upsell · ~164M · T10–T11
  - **TÁI KÝ (xanh)**
    - **DMS** · 250M (+ PS 60M: chấm KPI theo NĐ 355 & phê duyệt kế hoạch công việc năm) · hoàn tất thủ tục T11 · HĐ hết hạn T12
  - **CƠ HỘI LỚN (xanh nhạt)**
    - **Pearl River Hotel** · 310M · chốt T12 · *Chủ đầu tư đã có chủ trương, PIC (anh Tuyển) tích cực*
  - **DỰ PHÒNG / UPSIDE (viền nét đứt, xám)**
    - Draho tái ký sớm · 500M · HĐ hiện tại hết hạn T5/2027, thay DMS nếu DMS trượt
    - LiteOn · ~1.000M · đang dựng showcase, đã nhận dữ liệu, khách cần giải ngân trong năm
    - Truyền dẫn Long Biên · chưa xác định dealsize · quan tâm bảo mật, đề xuất on-premise
- Cột phải: **thanh tích lũy dọc** (waterfall) tới vạch 1.000M, có mốc **"Sàn: nền + DMS ≈ 529M ≥ Goal L5 cả quý (525M)"**
- Chip nhỏ dưới bảng: *Manager và DCEO đã cùng tham gia 2 deal lớn trong danh sách*
- [BUILD] từng tầng

### S20 · Rủi ro & phương án
- Tiêu đề: **Em đã tính trước các rủi ro lớn nhất**
- 4 card rủi ro (mức độ: chấm đỏ/vàng) → phương án:
  1. 🔴 **DMS: tỷ lệ active chỉ 30–40%.** Luồng văn bản đến thuộc hệ thống Bộ Công Thương, không đấu nối API được.
     → Chuyển trọng tâm giá trị sang **giao việc, kế hoạch công việc năm, chấm KPI theo NĐ 355** (gói PS 60M) · Review sử dụng theo phòng ban trong T10, tìm phòng ban ủng hộ · Nếu trượt: **Draho thay thế**
  2. 🟡 **Draho: hợp đồng còn hạn tới T5/2027.**
     → Lý do ký sớm: chốt ngân sách 2027 ngay trong Q4 · lên hệ thống 2.0 sớm, cấn trừ thời gian còn lại của hợp đồng cũ
  3. 🟡 **Tháng 10 mỏng (chưa có deal về).**
     → Đẩy LPH và deal nhỏ về trong T10 · new sales ~114M · chương trình ưu đãi trường học
  4. 🟡 **Pearl River phụ thuộc thời điểm chốt cuối năm.**
     → Bám sát PIC, chuẩn bị sẵn hợp đồng · LiteOn là upside bù
- 🎤 Nếu bị hỏi "cả DMS và Draho đều trượt thì sao": trả lời bằng nền + Pearl River + upsell/new sales, kèm LiteOn.

### S21 · Kết (nền navy)
- Giữa màn hình, **quote lớn** (64–80px, 700, trắng), animation word-by-word:
  > **"Raise the bar. Then clear it."**
- Dưới quote, một dòng mảnh `--blue-300`: *Q4: 1 tỷ · Cả năm: 112% goal · Level 5*
- Góc dưới: Cảm ơn anh chị · Phan Mạnh Quang · Business Consultant · Base.vn
- Đặt quote trong một hằng số `CLOSING_QUOTE` ở đầu file để người trình bày dễ đổi. Các phương án thay thế (để trong comment, không hiện trên slide):
  - `"Same hunger. Higher bar."`
  - `"Level up is not the finish line. It's the new baseline."`
  - `"Earn it. Then own it."`
  - `"Pressure is a privilege."`

### Phụ lục (sau slide kết, chỉ mở khi được hỏi, phím `A`)
- **A1** · Doanh thu theo tháng T1–T9/2026 (bar chart từ `months2026`, đường KPI L5 125 và Goal L5 175). T2 và T4 là 2 tháng trắng deal.
- **A2** · 15 deal won Q3 (bảng: tên · giá trị · nguồn · ngày chốt)
  - LynkID gia hạn 72,0 · CSM · 31/07
  - Biogreen 108,84 · Marketing · 18/08
  - Máy tính Trần Hạnh 117,12 · Partner · 28/07
  - Nebecom upsell 15,0 · Up/cross · 31/08
  - Nebecom upsell 5 TK 7,16 · Up/cross · 20/07
  - MC-Bifi upsell 10 TK 5,6 · CSM · 24/07
  - PS Xí nghiệp Năng lượng TN 43,2 · Up/cross · 02/08
  - CP TM Ba Vì 51,41 · Marketing · 18/09
  - Nebecom upsells 156,84 · Up/cross · 30/09
  - Dược Nam Hà 34,72 · CSM · 30/09
  - Bifi upsells 22,4 · CSM · 28/09
  - LynkID upsell 27,06 · CSM · 30/09
  - M7 upsell 20 TK 14,4 · Up/cross · 10/09
  - Draho upsell 10 TK HRM 3,15 · CSM · 27/08
  - Rino edu upsell 22,37 · Up/cross · 19/08
  - **Tổng 701,28M**
- **A3** · Bảng mốc doanh thu L4/L5 theo chính sách v5.1 (Back-up/KPI/Goal/Outperformance) và điều kiện hạ cấp: *cảnh báo khi 2 tháng dưới KPI; hạ cấp khi 3 tháng dưới KPI trong 6 tháng gần nhất*
- **A4** · Pipeline dài hạn: GAET (Showcase, ~1,5B, chưa có next step cụ thể) · Lotte Jakarta

---

## 6. Checklist trước khi bàn giao

- [ ] Mọi số khớp với `DATA` và mục 5. Không có số nào tự suy ra thêm.
- [ ] **Không có bất kỳ con số hay % nào về lead bị thu hồi** ở bất cứ đâu (slide, notes, phụ lục).
- [ ] Mọi tỷ lệ chuyển đổi và tỷ trọng theo nguồn đều có biểu đồ đi kèm.
- [ ] Phễu new sales dùng đúng gốc 107 lead → 33 new meeting → 3 deal new won. Không dùng số 51, 22, 17 của bản cũ.
- [ ] Thứ tự chương đúng: Review Q3 → Trưởng thành → Action plan → Forecast Q4 → Kết.
- [ ] Chạy thử ở 1920×1080, 1366×768 và cửa sổ hẹp: không tràn chữ, không vỡ layout.
- [ ] Không chữ nào nhỏ hơn 14px. Tiêu đề không quá 2 dòng.
- [ ] Phím `N` (notes + timer), `O` (overview), `A` (phụ lục), `F` (fullscreen) hoạt động.
- [ ] Dấu tiếng Việt hiển thị đúng ở mọi cỡ chữ.
- [ ] Mở file trực tiếp bằng trình duyệt (`file://`) vẫn chạy, không cần server.
