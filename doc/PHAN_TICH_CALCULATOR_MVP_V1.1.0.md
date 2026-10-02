# Phân tích Hello Calculator MVP v1.1.0 và ánh xạ kiến thức Web

## 1. Mục tiêu của phiên bản

Hello Calculator MVP v1.1.0 là ứng dụng web tĩnh phục vụ hai mục tiêu song song:

- cung cấp một máy tính cơ bản có thể sử dụng bằng chuột và bàn phím;
- giúp người mới quan sát được cách HTML, CSS và JavaScript phối hợp trong một sản phẩm thật.

Phiên bản 1.1.0 mở rộng v1.0 bằng `±`, `%`, lịch sử 5 phép tính, xóa lịch sử, Learning Panel chi tiết hơn và nhiều test tự động hơn. Dự án vẫn không dùng framework hay package runtime, nên người học có thể nhìn thấy trực tiếp các khái niệm nền tảng.

---

## 2. Kiến trúc tổng thể

Ứng dụng được chia thành bốn lớp:

```text
index.html
   │ tạo cấu trúc và khai báo dữ liệu cho nút
   ↓
css/base.css + css/style.css
   │ trình bày cấu trúc và trạng thái
   ↓
js/app.js
   │ nhận Event, chuyển thành Action, gọi model và render DOM
   ↓
js/calculator.js
     lưu State và thực hiện quy tắc tính toán
```

Test logic nằm trong `tests/model_test.mjs`. Test thủ công nằm trong `docs/TEST_CASES.md`.

Điểm quan trọng nhất là `calculator.js` không phụ thuộc trình duyệt. Vì vậy logic có thể được kiểm thử bằng Node.js mà không cần DOM.

---

## 3. Ánh xạ yêu cầu tới mã nguồn

### Nhập số và dấu thập phân

- HTML: các button có `data-action="number"` hoặc `data-action="decimal"`.
- JavaScript model: `inputNumber()` và `inputDecimal()`.
- JavaScript giao diện: listener click và `keydown` trong `app.js`.
- CSS: `.key`, `.key-number` và các trạng thái focus/hover.
- Test: phép tính thập phân, hai dấu chấm và giới hạn 16 ký tự.

### Bốn phép toán

- HTML: button có `data-action="operator"` và `data-value` tương ứng.
- Model: `chooseOperator()` lưu toán hạng và `calculate()` thực hiện phép tính.
- Render: class `.is-selected` cho biết toán tử đang hoạt động.
- Test: cộng, trừ, nhân, chia và thay toán tử liên tiếp.

### Đổi dấu `±`

- HTML: button `data-action="toggle-sign"`.
- Model: `toggleSign()` thêm hoặc bỏ dấu `-` ở đầu chuỗi nhập.
- Bàn phím: `F9` được ánh xạ thành action `toggle-sign`.
- Test: đổi số dương thành âm và đổi hai lần trở về số dương.

### Phần trăm `%`

- HTML: button `data-action="percent"`.
- Model: `inputPercent()`.
- Bàn phím: ký tự `%` tạo cùng action với click.
- Test: phần trăm độc lập, phần trăm trong phép cộng và phép nhân.

Quy tắc được chọn:

- đứng độc lập hoặc đi với nhân/chia: chia số hiện tại cho 100;
- đi với cộng/trừ: tính phần trăm của toán hạng thứ nhất.

Ví dụ `200 + 10 % =` được biến thành `200 + 20 =`, cho kết quả `220`.

### Lịch sử

- HTML: `<section class="history">`, danh sách `<ol>` và nút xóa.
- Model: mảng `history`, `addHistory()` và `clearHistory()`.
- Render: `renderHistory()` tạo `<li>` bằng DOM API.
- CSS: `.history-*` trong stylesheet v1.1.0.
- Test: lưu phép tính, giới hạn 5 mục và xóa lịch sử.

Lịch sử chỉ tồn tại trong bộ nhớ. Tải lại trang sẽ xóa lịch sử. Đây là giới hạn phù hợp với MVP và tránh đưa thêm localStorage trước khi người học nắm chắc State.

### Chia cho 0

- Model: `calculate()` trả `null` khi mẫu số bằng 0.
- `setError()` chuyển model sang trạng thái lỗi.
- Render thêm `.is-error` cho display và `.error` cho status badge.
- CSS chuyển màu để lỗi dễ nhận biết.
- Test xác nhận cả `currentInput === 'Error'` và `error === true`.

---

## 4. Kiến thức HTML thể hiện trong dự án

### HTML tạo cấu trúc, không tính toán

`index.html` mô tả tiêu đề, màn hình, keypad, lịch sử và Learning Panel. Nó không chứa công thức cộng trừ. Việc này giúp phân biệt rõ nội dung với hành vi.

### HTML ngữ nghĩa

Dự án sử dụng:

- `<main>` cho nội dung chính;
- `<header>` và `<footer>` cho phần đầu và cuối;
- `<section>` cho calculator và lịch sử;
- `<button>` cho thao tác;
- `<output>` cho giá trị state;
- `<details>` và `<summary>` cho Learning Panel có thể thu gọn;
- `<ol>` cho lịch sử có thứ tự.

Thẻ đúng ngữ nghĩa hỗ trợ accessibility và làm mã dễ giải thích.

### `id`, `class` và `data-*`

- `id` định danh duy nhất để JavaScript tìm phần tử, ví dụ `#display`.
- `class` gom nhóm phần tử để CSS trình bày, ví dụ `.key-control`.
- `data-action` nói nút muốn làm gì.
- `data-value` cung cấp giá trị của hành động.

Một button vừa là giao diện HTML, vừa là nơi khai báo dữ liệu đầu vào cho JavaScript:

```html
<button data-action="operator" data-value="+">+</button>
```

### Accessibility

- Button thật có thể focus và kích hoạt bằng bàn phím.
- `aria-label` giải thích các ký hiệu như `÷` và `±`.
- Display dùng `role="status"` và `aria-live="polite"`.
- Lịch sử dùng `aria-live="polite"` để thông báo khi danh sách thay đổi.
- Thứ tự button trong HTML cũng là thứ tự Tab tự nhiên.

---

## 5. Kiến thức CSS thể hiện trong dự án

### CSS theo lớp

`base.css` là hệ thống giao diện gốc của v1.0. `style.css` dùng `@import` để nạp base rồi khai báo phần mở rộng cho v1.1.0.

```css
@import url('./base.css');
```

Do cascade, quy tắc tải sau có thể điều chỉnh quy tắc cũ. Ví dụ v1.0 làm nút `=` cao hai hàng, còn v1.1.0 đặt lại:

```css
.key-equal {
  grid-column: auto;
  grid-row: auto;
}
```

Đây là ví dụ trực tiếp về chữ “Cascading” trong CSS.

### Design tokens bằng custom properties

Các biến như `--accent`, `--line`, `--radius-md` giúp màu sắc, viền và bo góc nhất quán. Phần lịch sử tái sử dụng các biến này thay vì tạo một phong cách không liên quan.

### Grid và Flexbox

- `.keypad` dùng Grid vì các phím nằm theo hàng và cột.
- `.history-heading` dùng Flexbox vì tiêu đề và nút xóa nằm trên một hàng.
- mỗi dòng lịch sử dùng Flexbox để tách biểu thức và kết quả về hai phía.

### Responsive

Media query tại 560 px và 360 px làm giảm khoảng cách, thay bố cục bảng state và chuyển dòng lịch sử sang dạng dọc. Mục tiêu kiểm thử cụ thể là không có cuộn ngang ở 320 px.

### Trạng thái tương tác

- `:hover` phản hồi con trỏ;
- `:active` phản hồi lúc nhấn;
- `:focus-visible` cho người dùng bàn phím;
- `.is-selected` thể hiện toán tử đang chọn;
- `.is-error` và `.error` thể hiện lỗi từ State.

CSS không tự quyết định ứng dụng có lỗi. JavaScript đọc State rồi thêm class; CSS chỉ quyết định class đó trông như thế nào.

---

## 6. Kiến thức JavaScript thể hiện trong dự án

### Module

`calculator.js` export `CalculatorModel`. `app.js` import class đó để tạo model. Test cũng import cùng class. Một phần logic được dùng ở cả ứng dụng thật và test mà không cần sao chép.

### Class và object

Class là bản thiết kế. `new CalculatorModel()` tạo object có state riêng và các method xử lý.

### State

State của v1.1.0 gồm:

- `currentInput`;
- `firstOperand`;
- `operator`;
- `waitingForOperand`;
- `justCalculated`;
- `error`;
- `completedExpression`;
- `history`;
- `lastEvent`, `lastAction`, `lastValue`.

Learning Panel đưa các state quan trọng ra màn hình để người học quan sát.

### Action

Action là tên chuẩn hóa cho ý định người dùng. Click `%` và gõ `%` là hai Event khác nhau nhưng đều tạo action `percent`. Nhờ vậy logic không cần quan tâm action đến từ chuột hay bàn phím.

### Rẽ nhánh

`handleAction()` dùng `switch` để chuyển action tới method tương ứng. `calculate()` dùng `switch` để chọn phép toán. `inputPercent()` dùng điều kiện để chọn quy tắc phần trăm.

### Mảng và tính bất biến ở mức đơn giản

Lịch sử thêm mục mới ở đầu bằng `unshift()`, sau đó tạo mảng tối đa 5 phần tử bằng `slice()`.

### Giới hạn đầu vào

Model kiểm tra độ dài trước khi nối thêm chữ số. Đặt quy tắc trong model bảo đảm cả click lẫn bàn phím đều tuân theo cùng giới hạn.

### Không dùng `eval()`

Ứng dụng không biến chuỗi người dùng thành code JavaScript. Bốn phép toán được liệt kê rõ trong `switch`, giúp hành vi dễ đọc, dễ test và tránh rủi ro thực thi code ngoài ý muốn.

---

## 7. Event → Action → State → Render → DOM

Xét thao tác `200 + 10 % =`:

1. Người dùng click `2`.
2. Trình duyệt tạo Event `click`.
3. `app.js` đọc `data-action="number"`, tạo Action `number` với Value `2`.
4. `CalculatorModel.handleAction()` gọi `inputNumber()`.
5. State `currentInput` thay đổi.
6. `render()` đọc State và ghi vào DOM.
7. Các bước tương tự diễn ra với `0`, `0` và `+`.
8. Khi bấm `%`, `inputPercent()` thấy toán tử là `+`, nên tính 10% của 200 thành 20.
9. Khi bấm `=`, model tính `200 + 20`, tạo kết quả 220 và thêm lịch sử.
10. `render()` cập nhật display, biểu thức, lịch sử và Learning Panel.

Câu tóm tắt dễ nhớ:

> Event là điều vừa xảy ra; Action là ý định đã chuẩn hóa; State là dữ liệu hiện tại; Render biến State thành DOM; DOM là giao diện sống trong trình duyệt.

---

## 8. Tại sao `renderHistory()` dùng DOM API?

`renderHistory()` tạo `li`, `span` và `strong` bằng `document.createElement()`, rồi gán nội dung bằng `textContent`.

Ưu điểm:

- người học nhìn thấy cách tạo DOM động;
- `textContent` không diễn giải dữ liệu thành HTML;
- `replaceChildren()` làm DOM phản ánh chính xác mảng `history` hiện tại.

Đây tiếp tục là quy tắc “State là nguồn sự thật”. Danh sách không tự quản lý dữ liệu riêng; nó được dựng lại từ model.

---

## 9. Ánh xạ test với quy tắc nghiệp vụ

Nhóm test phép tính xác nhận bốn toán tử và số thập phân. Nhóm test trạng thái xác nhận reset, backspace, thay toán tử và nhập số sau kết quả. Nhóm test v1.1.0 xác nhận đổi dấu, phần trăm, lịch sử, xóa lịch sử và giới hạn đầu vào.

Việc test trực tiếp `CalculatorModel` chứng minh kiến trúc tách lớp có hiệu quả: không cần giả lập click để biết `200 + 10%` có ra `220` hay không.

Các test giao diện vẫn phải làm thủ công hoặc bằng browser automation vì unit test không nhìn thấy:

- bố cục ở 320 px;
- màu và focus ring;
- thứ tự Tab;
- khả năng trình đọc màn hình thông báo;
- lỗi tải file trong Network;
- lỗi JavaScript trong Console.

---

## 10. So sánh v1.0 và v1.1.0

v1.0 tập trung vào bốn phép toán, reset, backspace, lỗi chia cho 0, keyboard và Learning Panel cơ bản.

v1.1.0 giữ nguyên những khả năng đó và thêm:

- action `toggle-sign`;
- action `percent` với quy tắc rõ ràng;
- state `history` giới hạn 5 mục;
- action `clear-history`;
- quan sát `lastAction`, `justCalculated`, `error`, `history.length`;
- giới hạn phần nhập 16 ký tự;
- giao diện lịch sử responsive;
- 21 unit test và 39 test case thủ công.

Đây là nâng cấp theo kiểu tăng dần: không thay framework, không viết lại toàn bộ và không phá các chức năng cũ.

---

## 11. Giới hạn và hướng phát triển tiếp theo

Các giới hạn hiện tại:

- phép tính ghép chạy tuần tự, không áp dụng ưu tiên nhân chia;
- lịch sử mất khi tải lại trang;
- chưa có ngoặc, căn bậc hai hoặc bộ nhớ;
- làm tròn số thực phù hợp học tập, không phù hợp phần mềm tài chính;
- chưa có test trình duyệt tự động.

Một phiên bản sau có thể chọn một trong các hướng nhỏ:

- dùng `localStorage` để giữ lịch sử;
- thêm test browser bằng Playwright;
- thêm chế độ biểu thức có ưu tiên toán tử;
- thêm theme tối với preference được lưu cục bộ.

Không nên thêm tất cả cùng lúc. Mỗi phiên bản nên có phạm vi nhỏ, tiêu chí test rõ ràng và tài liệu cập nhật đồng bộ.

---

## 12. Mẫu trình bày dự án

> Hello Calculator MVP v1.1.0 là ứng dụng web tĩnh dùng HTML, CSS và JavaScript thuần. HTML tạo cấu trúc có ngữ nghĩa cho display, keypad, lịch sử và Learning Panel. CSS dùng biến, Grid, Flexbox, pseudo-class và media query để tạo giao diện responsive, hỗ trợ chuột và bàn phím. JavaScript tách thành CalculatorModel chứa State và quy tắc nghiệp vụ, cùng app.js nhận Event, tạo Action và render DOM. Một thao tác luôn đi theo luồng Event → Action → State → Render → DOM. Logic được kiểm thử độc lập bằng Node.js, còn giao diện được kiểm thử trong trình duyệt. Phiên bản 1.1.0 thêm đổi dấu, phần trăm, lịch sử 5 mục và Learning Panel chi tiết nhưng vẫn giữ kiến trúc nhỏ, dễ đọc và không cần framework.

Nếu có thể giải thích đoạn trên bằng lời của mình và minh họa bằng phép tính `200 + 10 % =`, người học đã hiểu phần cốt lõi của project.
