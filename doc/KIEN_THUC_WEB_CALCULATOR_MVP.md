# Kiến thức HTML, CSS và JavaScript qua Calculator MVP

## 1. Tài liệu này dành cho ai?

Tài liệu này dành cho người chưa từng làm web. Sau khi đọc xong, bạn có thể:

- giải thích một trang web được tạo bởi HTML, CSS và JavaScript như thế nào;
- hiểu cấu trúc source của Hello Calculator MVP;
- mô tả quá trình từ lúc người dùng bấm nút đến lúc kết quả xuất hiện;
- chạy ứng dụng trên máy, kiểm thử và triển khai lên GitHub Pages;
- tự giải thích lại dự án bằng ngôn ngữ đơn giản.

Không cần biết lập trình trước. Những thuật ngữ mới đều được giải thích khi xuất hiện.

---

## 2. Bức tranh tổng thể: một trang web hoạt động thế nào?

Một ứng dụng web chạy trong **trình duyệt** như Chrome, Edge hoặc Firefox. Trong Calculator MVP, ba công nghệ chính có nhiệm vụ khác nhau:

- **HTML** tạo cấu trúc và nội dung: tiêu đề, màn hình, nút số, nút phép tính.
- **CSS** quyết định cách trình bày: màu sắc, kích thước, khoảng cách, bố cục và khả năng thích ứng với màn hình nhỏ.
- **JavaScript** tạo hành vi: nhận thao tác, tính toán, lưu trạng thái và cập nhật giao diện.

Có thể hình dung bằng ví dụ một ngôi nhà:

- HTML là khung nhà và các phòng;
- CSS là màu sơn, cách bố trí và trang trí;
- JavaScript là điện, công tắc và thiết bị làm cho ngôi nhà hoạt động.

Luồng chính của Calculator MVP là:

```text
Người dùng bấm nút
        ↓
Trình duyệt tạo Event
        ↓
JavaScript xử lý Event
        ↓
State của calculator thay đổi
        ↓
Hàm render cập nhật DOM
        ↓
Màn hình hiển thị kết quả mới
```

Các từ **Event**, **State**, **Render** và **DOM** sẽ được giải thích chi tiết ở phần sau.

---

## 3. Cấu trúc dự án

Source chính nằm trong thư mục `hello-calculator-mvp-v1.0`:

```text
hello-calculator-mvp-v1.0/
├── index.html
├── css/
│   └── style.css
├── js/
│   ├── app.js
│   └── calculator.js
├── docs/
│   ├── DEPLOY_GITHUB_PAGES.md
│   └── TEST_CASES.md
├── tests/
│   └── model_test.mjs
├── .nojekyll
└── README.md
```

Ý nghĩa của từng phần:

- `index.html`: trang được trình duyệt mở đầu tiên.
- `css/style.css`: toàn bộ phần trình bày của giao diện.
- `js/calculator.js`: mô hình và quy tắc tính toán.
- `js/app.js`: kết nối mô hình với giao diện và sự kiện người dùng.
- `tests/model_test.mjs`: kiểm thử tự động cho logic tính toán.
- `docs/TEST_CASES.md`: các tình huống kiểm thử thủ công.
- `docs/DEPLOY_GITHUB_PAGES.md`: hướng dẫn đưa ứng dụng lên Internet.
- `.nojekyll`: yêu cầu GitHub Pages phục vụ các file tĩnh trực tiếp, không xử lý bằng Jekyll.
- `README.md`: giới thiệu và hướng dẫn nhanh của dự án.

Việc tách file theo nhiệm vụ giúp mã dễ đọc, dễ sửa và dễ kiểm thử.

---

## 4. HTML: tạo cấu trúc cho calculator

### 4.1. HTML là gì?

HTML là viết tắt của **HyperText Markup Language**. HTML không thực hiện phép cộng hay phép chia. Nó mô tả nội dung nào có trên trang và quan hệ giữa các nội dung đó.

Một phần tử HTML thường có thẻ mở, nội dung và thẻ đóng:

```html
<h1>Hello Calculator</h1>
```

Trong ví dụ này:

- `<h1>` là thẻ mở;
- `Hello Calculator` là nội dung;
- `</h1>` là thẻ đóng;
- toàn bộ dòng là một phần tử HTML.

### 4.2. Bộ khung tài liệu

File `index.html` bắt đầu bằng:

```html
<!doctype html>
<html lang="vi">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Hello Calculator MVP v1.0</title>
</head>
<body>
  <!-- Nội dung nhìn thấy nằm ở đây -->
</body>
</html>
```

Ý nghĩa:

- `<!doctype html>` báo cho trình duyệt dùng chuẩn HTML hiện đại.
- `<html lang="vi">` cho biết nội dung chính dùng tiếng Việt.
- `<head>` chứa thông tin mô tả trang, không phải nội dung chính trên màn hình.
- `charset="utf-8"` giúp hiển thị đúng tiếng Việt và ký hiệu `÷`, `×`, `⌫`.
- thẻ `viewport` giúp giao diện hiển thị đúng trên điện thoại.
- `<title>` là tên xuất hiện trên tab trình duyệt.
- `<body>` chứa nội dung mà người dùng tương tác.

### 4.3. Nối HTML với CSS và JavaScript

Hai dòng quan trọng trong `<head>` là:

```html
<link rel="stylesheet" href="css/style.css" />
<script type="module" src="js/app.js"></script>
```

Dòng đầu yêu cầu trình duyệt tải CSS. Dòng sau yêu cầu tải file JavaScript dưới dạng **module**.

Đường dẫn `css/style.css` và `js/app.js` là đường dẫn tương đối. Trình duyệt tìm chúng dựa trên vị trí của `index.html`. Vì vậy cần giữ đúng cấu trúc thư mục khi triển khai.

`type="module"` cho phép JavaScript dùng `import` và `export`. Module cũng được thực thi sau khi HTML đã được phân tích, nên các phần tử giao diện đã sẵn sàng để JavaScript tìm kiếm.

### 4.4. HTML ngữ nghĩa

Dự án dùng các thẻ có ý nghĩa rõ ràng:

- `<main>` chứa nội dung chính;
- `<header>` chứa phần đầu trang;
- `<section>` nhóm một nội dung có chủ đề;
- `<footer>` chứa phần cuối trang;
- `<button>` biểu diễn một nút có thể thao tác;
- `<output>` biểu diễn dữ liệu đầu ra;
- `<details>` và `<summary>` tạo vùng có thể mở hoặc thu gọn.

Đây gọi là **HTML ngữ nghĩa**. Nó giúp người đọc mã, trình duyệt, công cụ tìm kiếm và công nghệ hỗ trợ hiểu trang tốt hơn.

### 4.5. `id`, `class` và `data-*`

Một nút số trong dự án có dạng:

```html
<button
  class="key key-number"
  type="button"
  data-action="number"
  data-value="7"
>7</button>
```

Các thuộc tính có nhiệm vụ khác nhau:

- `class` phân loại phần tử để CSS tạo kiểu. Một phần tử có thể có nhiều class.
- `id` định danh duy nhất một phần tử, ví dụ `id="display"`.
- `data-action` và `data-value` lưu dữ liệu riêng để JavaScript đọc.
- `type="button"` xác định đây là nút thông thường.

Với nút trên, JavaScript hiểu hành động là `number` và giá trị là `7`. Nhờ đó không cần viết một hàm riêng cho từng nút số.

### 4.6. Khả năng tiếp cận

Khả năng tiếp cận, hay **accessibility**, giúp nhiều nhóm người dùng sử dụng trang web, bao gồm người dùng bàn phím và trình đọc màn hình.

Dự án có các ví dụ:

```html
<button aria-label="Chia">÷</button>
<div id="display" role="status" aria-live="polite">0</div>
```

- `aria-label="Chia"` cung cấp tên dễ hiểu cho ký hiệu `÷`.
- `role="status"` cho biết vùng này chứa trạng thái cần thông báo.
- `aria-live="polite"` cho phép trình đọc màn hình thông báo kết quả thay đổi mà không ngắt nội dung đang đọc.

HTML đúng ngữ nghĩa là nền tảng của accessibility. ARIA chỉ bổ sung khi HTML thông thường chưa diễn đạt đủ.

---

## 5. CSS: trình bày và responsive

### 5.1. CSS là gì?

CSS là viết tắt của **Cascading Style Sheets**. CSS chọn phần tử HTML rồi gán các quy tắc trình bày cho nó.

Ví dụ:

```css
.key {
  min-height: 58px;
  border-radius: 10px;
  cursor: pointer;
}
```

- `.key` là bộ chọn, nghĩa là chọn mọi phần tử có class `key`.
- các cặp `thuộc-tính: giá-trị` nằm trong `{}` là khai báo CSS.

### 5.2. Cascade và độ ưu tiên

Từ “Cascading” diễn tả cách trình duyệt giải quyết khi nhiều quy tắc cùng áp dụng cho một phần tử. Quy tắc cụ thể hơn hoặc xuất hiện sau thường có thể ghi đè quy tắc trước.

Ví dụ mọi nút dùng `.key`, còn nút bằng dùng thêm `.key-equal`:

```css
.key { background: #f8fafc; }
.key-equal { background: #2558c7; color: #fff; }
```

Nút `=` nhận kiểu chung của `.key`, đồng thời nhận nền xanh và chữ trắng của `.key-equal`.

### 5.3. CSS variables

Dự án khai báo các giá trị dùng lại tại `:root`:

```css
:root {
  --accent: #2558c7;
  --danger: #a83535;
  --radius-md: 14px;
  --gap: 10px;
}
```

Sau đó dùng bằng `var(...)`:

```css
.keypad { gap: var(--gap); }
```

Lợi ích là màu sắc và kích thước nhất quán. Khi muốn đổi màu chủ đạo, chỉ cần sửa một biến thay vì tìm nhiều vị trí.

### 5.4. Box model

Mỗi phần tử HTML có thể xem như một chiếc hộp gồm:

```text
margin → border → padding → content
```

- `content`: nội dung thật;
- `padding`: khoảng trống bên trong viền;
- `border`: đường viền;
- `margin`: khoảng cách bên ngoài.

Dự án dùng:

```css
* { box-sizing: border-box; }
```

Quy tắc này làm cho kích thước khai báo bao gồm cả padding và border, nhờ đó việc tính kích thước dễ dự đoán hơn.

### 5.5. Bố cục bằng Flexbox và Grid

**Flexbox** phù hợp để sắp xếp phần tử theo một hàng hoặc một cột. Phần đầu calculator dùng Flexbox để đặt tiêu đề và trạng thái ở hai phía:

```css
.calculator-title-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
```

**CSS Grid** phù hợp với bố cục có hàng và cột. Bàn phím calculator có bốn cột:

```css
.keypad {
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
  gap: 10px;
}
```

`repeat(4, ...)` tạo bốn cột. `1fr` nghĩa là mỗi cột nhận một phần bằng nhau của không gian còn lại.

Nút `0` chiếm hai cột và nút `=` chiếm hai hàng:

```css
.key-zero { grid-column: span 2; }
.key-equal { grid-row: 4 / span 2; }
```

### 5.6. Đơn vị kích thước

Dự án sử dụng nhiều loại đơn vị:

- `px`: số pixel, phù hợp với viền hoặc kích thước nhỏ cần rõ ràng;
- `rem`: dựa trên cỡ chữ gốc, thuận tiện cho typography;
- `%`: tỷ lệ so với vùng chứa;
- `vw`: tỷ lệ theo chiều rộng màn hình;
- `vh`: tỷ lệ theo chiều cao màn hình.

Hàm `clamp()` giúp cỡ chữ linh hoạt nhưng không quá nhỏ hoặc quá lớn:

```css
font-size: clamp(2.25rem, 11vw, 3.4rem);
```

Nghĩa là cỡ chữ mong muốn là `11vw`, nhưng tối thiểu `2.25rem` và tối đa `3.4rem`.

### 5.7. Responsive là gì?

**Responsive design** là thiết kế tự thích ứng với nhiều kích thước màn hình. Dự án đặt chiều rộng tối thiểu hỗ trợ là khoảng 320 px và dùng media query:

```css
@media (max-width: 360px) {
  .calculator-card { padding: 10px; }
  .keypad { gap: 6px; }
  .state-grid { grid-template-columns: 1fr; }
}
```

Khi viewport rộng không quá 360 px, padding và khoảng cách nhỏ lại; bảng trạng thái đổi sang một cột. Nhờ đó trang tránh cuộn ngang.

Trong đó **viewport** là vùng trang web đang nhìn thấy trong cửa sổ trình duyệt.

### 5.8. Trạng thái tương tác

CSS có thể tạo kiểu theo trạng thái:

- `:hover`: con trỏ đang nằm trên nút;
- `:active`: nút đang được nhấn;
- `:focus-visible`: phần tử nhận focus bằng bàn phím;
- `.is-selected`: class do JavaScript thêm khi phép toán được chọn;
- `.is-error`: class do JavaScript thêm khi có lỗi.

Ví dụ:

```css
.key:focus-visible {
  outline: 3px solid rgba(37, 88, 199, .30);
  outline-offset: 2px;
}
```

Đường viền focus rõ ràng giúp người không dùng chuột biết mình đang chọn nút nào.

### 5.9. Tôn trọng thiết lập giảm chuyển động

Dự án có:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after { transition: none !important; }
}
```

Nếu người dùng yêu cầu hệ điều hành giảm hiệu ứng chuyển động, trang sẽ tắt transition. Đây là một thực hành accessibility tốt.

---

## 6. JavaScript: tạo hành vi

### 6.1. JavaScript làm gì trong dự án?

JavaScript thực hiện bốn công việc chính:

1. lưu trạng thái hiện tại của calculator;
2. nhận hành động từ chuột hoặc bàn phím;
3. áp dụng quy tắc tính toán;
4. cập nhật nội dung và class trên giao diện.

Dự án tách hai trách nhiệm:

- `calculator.js` chỉ xử lý logic;
- `app.js` làm việc với trình duyệt và DOM.

Đây là nguyên tắc **separation of concerns**, tức là tách các mối quan tâm. Logic độc lập với giao diện sẽ dễ kiểm thử hơn.

### 6.2. Module, `export` và `import`

Trong `calculator.js`:

```js
export class CalculatorModel {
  // ...
}
```

Trong `app.js`:

```js
import { CalculatorModel } from './calculator.js';
```

`export` công khai một thành phần để file khác sử dụng. `import` đưa thành phần đó vào file hiện tại.

Đường dẫn `./calculator.js` nghĩa là tìm file trong cùng thư mục với `app.js`.

### 6.3. Class và object

`CalculatorModel` là một **class**, có thể hiểu là bản thiết kế cho calculator. Dòng sau tạo một object cụ thể từ bản thiết kế đó:

```js
const model = new CalculatorModel();
```

Object `model` chứa dữ liệu trạng thái và các hàm xử lý.

### 6.4. State của calculator

**State** là tập hợp dữ liệu mô tả calculator tại một thời điểm. Các biến quan trọng là:

- `currentInput`: số hoặc kết quả đang hiển thị;
- `firstOperand`: toán hạng thứ nhất;
- `operator`: phép toán đang chọn;
- `waitingForOperand`: có đang chờ người dùng nhập số thứ hai hay không;
- `justCalculated`: phép tính vừa hoàn thành hay chưa;
- `error`: có đang ở trạng thái lỗi hay không;
- `completedExpression`: biểu thức vừa hoàn thành;
- `lastEvent` và `lastValue`: thông tin phục vụ Learning Panel.

Ví dụ khi bắt đầu, state gần tương đương:

```js
{
  currentInput: '0',
  firstOperand: null,
  operator: null,
  waitingForOperand: false,
  justCalculated: false,
  error: false
}
```

`null` nghĩa là hiện chưa có giá trị phù hợp. Nó khác chuỗi rỗng `''` và khác số `0`.

### 6.5. Vì sao số đang nhập được lưu dưới dạng chuỗi?

`currentInput` có giá trị như `'123'` hoặc `'1.5'`, tức là chuỗi ký tự. Khi người dùng bấm lần lượt `1`, `2`, `3`, chương trình dễ ghép chuỗi:

```js
this.currentInput = this.currentInput + digit;
```

Đến lúc tính toán, chương trình đổi chuỗi thành số:

```js
const inputValue = Number(this.currentInput);
```

Đây là sự khác nhau giữa **dữ liệu để nhập/hiển thị** và **dữ liệu để tính toán**.

### 6.6. Điều kiện và rẽ nhánh

JavaScript dùng `if` và `switch` để lựa chọn hành vi.

Ví dụ, không thêm dấu chấm nếu số đã có dấu chấm:

```js
if (!this.currentInput.includes('.')) {
  this.currentInput += '.';
}
```

Phép tính được chọn bằng `switch`:

```js
switch (operator) {
  case '+': return a + b;
  case '-': return a - b;
  case '*': return a * b;
  case '/': return b === 0 ? null : a / b;
  default: return b;
}
```

Khi chia cho `0`, hàm trả về `null`. Phần xử lý phía trên nhận biết giá trị này và chuyển calculator sang `Error`.

### 6.7. Phương thức xử lý hành động

Mọi thao tác đi qua một cửa chung:

```js
handleAction(action, value, eventName = 'click')
```

Ví dụ:

- bấm số 7 tạo action `number`, value `'7'`;
- bấm dấu cộng tạo action `operator`, value `'+'`;
- bấm bằng tạo action `equals`, value `'='`;
- bấm xóa tạo action `clear` hoặc `backspace`.

`handleAction` dùng `switch` để gọi đúng phương thức. Cách thiết kế này thống nhất xử lý chuột và bàn phím.

### 6.8. Xử lý số thực

Số thực trong máy tính có thể phát sinh sai số, ví dụ một số phép tính lý tưởng là `0.3` nhưng biểu diễn nội bộ có thể dài hơn. Dự án làm tròn kết quả đến 10 chữ số thập phân:

```js
const rounded = Math.round((value + Number.EPSILON) * 1e10) / 1e10;
```

Đây là cách đơn giản phù hợp với calculator phục vụ học tập. Nó không nên được dùng thay cho thư viện số thập phân chuyên dụng trong phần mềm tài chính.

---

## 7. DOM, Event và Render

### 7.1. DOM là gì?

Sau khi đọc HTML, trình duyệt tạo một mô hình trong bộ nhớ gọi là **Document Object Model**, viết tắt là DOM.

Mỗi thẻ trở thành một object mà JavaScript có thể tìm, đọc và thay đổi. Khi JavaScript đổi DOM, giao diện trên màn hình đổi theo mà không cần tải lại toàn bộ trang.

### 7.2. Tìm phần tử trong DOM

`app.js` lưu các phần tử cần dùng:

```js
const dom = {
  keypad: document.querySelector('#keypad'),
  display: document.querySelector('#display'),
  expression: document.querySelector('#expression')
};
```

`document.querySelector('#display')` tìm phần tử có `id="display"`.

Việc lưu kết quả vào object `dom` giúp mã phía sau ngắn và tránh phải tìm lại cùng phần tử nhiều lần.

### 7.3. Event là gì?

**Event** là thông báo rằng một việc vừa xảy ra, ví dụ:

- người dùng click;
- người dùng nhấn phím;
- trang tải xong;
- giá trị của ô nhập thay đổi.

JavaScript đăng ký một **event listener**, tức là một hàm chờ và xử lý sự kiện:

```js
dom.keypad.addEventListener('click', (event) => {
  // Xử lý click
});
```

### 7.4. Event delegation

Thay vì gắn listener vào từng nút, dự án gắn một listener vào cả keypad:

```js
const button = event.target.closest('button[data-action]');
if (!button) return;
handleAction(button.dataset.action, button.dataset.value, 'click');
```

Sự kiện click từ nút sẽ **nổi lên** phần tử cha. Kỹ thuật này gọi là **event delegation**.

Lợi ích:

- chỉ cần một listener;
- mã ngắn hơn;
- thêm nút mới có cùng cấu trúc thường không cần đăng ký listener mới.

`dataset.action` đọc `data-action`; `dataset.value` đọc `data-value` trong HTML.

### 7.5. Sự kiện bàn phím

Ứng dụng nghe `keydown` trên `window`. Sau đó nó ánh xạ phím thành action:

- `0–9` → `number`;
- `.` → `decimal`;
- `+`, `-`, `*`, `/` → `operator`;
- `Enter` hoặc `=` → `equals`;
- `Backspace` → `backspace`;
- `Escape` → `clear`.

Với một số phím, mã gọi `event.preventDefault()` để ngăn hành vi mặc định của trình duyệt gây ảnh hưởng đến trải nghiệm calculator.

### 7.6. Render là gì?

**Render** trong dự án là quá trình biến state hiện tại thành giao diện hiện tại.

Hàm `render()` thực hiện các việc như:

```js
dom.display.textContent = model.currentInput;
dom.expression.textContent = model.buildExpression();
dom.display.classList.toggle('is-error', model.error);
```

- `textContent` thay đổi văn bản trong phần tử;
- `classList.toggle(className, condition)` thêm class nếu điều kiện đúng và bỏ class nếu điều kiện sai.

Quy tắc quan trọng là:

```text
State là nguồn sự thật → render đọc state → DOM phản ánh state
```

Không nên để nhiều đoạn mã tự ý thay giao diện theo những cách không liên quan đến state, vì khi đó rất khó biết giao diện đang đại diện cho dữ liệu nào.

### 7.7. Chu trình đầy đủ với phép tính `2 + 3 =`

1. Người dùng bấm `2`.
2. Trình duyệt phát sinh event `click`.
3. Listener đọc `data-action="number"` và `data-value="2"`.
4. `handleAction` gọi `inputNumber('2')`.
5. `currentInput` đổi từ `'0'` thành `'2'`.
6. `render()` ghi `'2'` vào DOM của màn hình.
7. Người dùng bấm `+`; model lưu `firstOperand = 2`, `operator = '+'` và bắt đầu chờ số tiếp theo.
8. Người dùng bấm `3`; `currentInput` thành `'3'`.
9. Người dùng bấm `=`; model tính `2 + 3`.
10. `currentInput` thành `'5'`; `render()` hiển thị `5`.

Learning Panel cho phép quan sát Event và State thay đổi sau từng bước.

---

## 8. Các quy tắc nghiệp vụ của Calculator MVP

**Quy tắc nghiệp vụ** là cách ứng dụng phải phản ứng trong từng tình huống.

Calculator hiện hỗ trợ:

- nhập số từ `0` đến `9`;
- nhập một dấu thập phân cho mỗi số;
- cộng, trừ, nhân và chia;
- thay phép toán nếu bấm hai phép toán liên tiếp;
- xóa ký tự cuối bằng `⌫`;
- xóa toàn bộ bằng `C` hoặc `Escape`;
- hiện `Error` khi chia cho `0`;
- nhập số mới sau khi vừa tính xong;
- thao tác bằng chuột hoặc bàn phím.

Một vài trường hợp đáng chú ý:

- Nhập `1 . 2 . 3` cho kết quả nhập là `1.23`, vì dấu chấm thứ hai bị bỏ qua.
- Nhập `12 + − 5 =` cho kết quả `7`, vì dấu `−` thay thế dấu `+` trước đó.
- Sau `5 + 3 =`, bấm `2` thì bắt đầu phép tính mới và màn hình trở thành `2`.
- Bấm `⌫` khi chỉ còn một chữ số sẽ đưa màn hình về `0`.

MVP chưa có ngoặc, phần trăm, đổi dấu, bộ nhớ, lịch sử hoặc ưu tiên phép toán. Đây là giới hạn có chủ ý để sản phẩm đầu tiên nhỏ và dễ học.

---

## 9. Kiểm thử

### 9.1. Tại sao phải kiểm thử?

Kiểm thử xác nhận ứng dụng thực hiện đúng yêu cầu, đồng thời phát hiện lỗi khi mã được sửa sau này.

Dự án có hai lớp kiểm thử:

- **unit test** kiểm tra riêng logic `CalculatorModel`;
- **manual test** kiểm tra ứng dụng thật trong trình duyệt.

### 9.2. Unit test

Chạy tại thư mục dự án:

```bash
cd hello-calculator-mvp-v1.0
node tests/model_test.mjs
```

Kết quả thành công:

```text
PASS: 12 calculator model tests
```

Một unit test thường có ba ý:

1. chuẩn bị model;
2. thực hiện chuỗi hành động;
3. dùng assertion để so sánh kết quả thật với kết quả mong đợi.

Ví dụ ý tưởng của test cộng:

```js
assert.equal(m.currentInput, '5');
```

Nếu giá trị không phải `'5'`, Node.js báo lỗi và dừng test.

### 9.3. Kiểm thử thủ công

Mở `docs/TEST_CASES.md` và thực hiện T01 đến T15. Ngoài kết quả phép tính, cần kiểm tra:

- phím bàn phím hoạt động;
- viewport 320 px không có thanh cuộn ngang;
- focus bằng phím Tab nhìn thấy rõ;
- Learning Panel cập nhật đúng;
- DevTools Console không có lỗi;
- giao diện ổn định trên Chrome, Edge và Firefox hiện đại.

Unit test đạt không có nghĩa toàn bộ giao diện chắc chắn đúng. Nó không tự kiểm tra màu sắc, bố cục, focus hoặc cách trình duyệt hiển thị DOM.

---

## 10. Chạy ứng dụng trên máy

Calculator không cần cài package. Tuy nhiên, vì JavaScript dùng module, nên nên phục vụ trang qua HTTP thay vì mở file trực tiếp.

Chạy:

```bash
cd hello-calculator-mvp-v1.0
python3 -m http.server 8080
```

Mở trình duyệt tại:

```text
http://localhost:8080
```

Giải thích:

- `python3` chạy Python;
- `-m http.server` chạy module web server đơn giản có sẵn;
- `8080` là cổng mạng;
- `localhost` có nghĩa là chính máy đang sử dụng.

Để dừng server, quay lại Terminal và nhấn `Ctrl+C`.

Server này phù hợp cho học tập và phát triển cục bộ, không phải server production cho một hệ thống lớn.

---

## 11. Triển khai lên GitHub Pages

### 11.1. Triển khai là gì?

**Deploy** hoặc triển khai là đưa ứng dụng từ máy cá nhân lên một máy chủ để người khác truy cập qua Internet.

Calculator là **static web app** vì server chỉ cần gửi các file HTML, CSS và JavaScript. Phần tính toán chạy trong trình duyệt, không cần backend hay cơ sở dữ liệu.

### 11.2. Git và GitHub

- **Git** lưu lịch sử thay đổi mã nguồn.
- **GitHub** lưu repository Git trên Internet và cung cấp công cụ cộng tác.
- **GitHub Pages** phục vụ website tĩnh từ một repository.

Các lệnh cơ bản:

```bash
git init
git add .
git commit -m "Initial Hello Calculator MVP v1.0"
git branch -M main
git remote add origin https://github.com/USERNAME/hello-calculator-mvp-v1.git
git push -u origin main
```

Ý nghĩa:

- `git init`: tạo repository Git cục bộ;
- `git add .`: đưa thay đổi vào vùng chuẩn bị commit;
- `git commit`: lưu một mốc lịch sử;
- `git branch -M main`: đặt tên nhánh chính là `main`;
- `git remote add origin`: liên kết repository cục bộ với GitHub;
- `git push`: gửi commit lên GitHub.

Phải thay `USERNAME` bằng tên tài khoản GitHub thật.

### 11.3. Cấu hình GitHub Pages

Trong repository trên GitHub:

1. mở **Settings**;
2. chọn **Pages**;
3. tại **Build and deployment**, chọn **Deploy from a branch**;
4. chọn branch `main` và folder `/(root)`;
5. nhấn **Save**.

URL thường có dạng:

```text
https://USERNAME.github.io/hello-calculator-mvp-v1/
```

GitHub Pages tìm `index.html` làm trang đầu. Các đường dẫn tương đối trong dự án giúp CSS và JavaScript vẫn được tìm đúng khi website nằm dưới `/hello-calculator-mvp-v1/`.

### 11.4. Cập nhật website

Sau khi sửa source:

```bash
git add .
git commit -m "Mô tả thay đổi"
git push
```

GitHub Pages sẽ tự triển khai lại. Sau khi push, có thể cần chờ một khoảng ngắn rồi tải lại trang.

### 11.5. Kiểm tra sau triển khai

Sau khi có URL HTTPS:

- chạy lại toàn bộ test thủ công;
- thử trên máy tính và điện thoại;
- mở DevTools Console để tìm lỗi JavaScript;
- mở tab Network để kiểm tra HTML, CSS và JavaScript tải thành công;
- xác nhận URL dùng HTTPS;
- thử hard refresh nếu trình duyệt đang giữ bản cũ trong cache.

---

## 12. DevTools và cách tìm lỗi

**Developer Tools**, hay DevTools, là bộ công cụ phát triển tích hợp trong trình duyệt. Thường có thể mở bằng `F12` hoặc `Ctrl+Shift+I`.

Các khu vực hữu ích:

- **Elements**: xem HTML/DOM và CSS đang áp dụng;
- **Console**: xem lỗi JavaScript và thử lệnh;
- **Network**: xem các file có tải thành công hay không;
- **Sources**: xem và đặt breakpoint trong JavaScript;
- **Device toolbar**: mô phỏng kích thước điện thoại.

Quy trình tìm lỗi đơn giản:

1. mô tả chính xác thao tác tạo ra lỗi;
2. kiểm tra Console;
3. xem Event có được nhận trong Learning Panel không;
4. xem State có thay đổi đúng không;
5. nếu State đúng nhưng màn hình sai, kiểm tra `render()` và DOM;
6. nếu DOM đúng nhưng hiển thị sai, kiểm tra CSS;
7. sửa một vấn đề nhỏ rồi chạy lại test.

Ví dụ phân loại lỗi:

- bấm nút nhưng không có Event: kiểm tra listener hoặc `data-action`;
- Event đúng nhưng kết quả sai: kiểm tra `CalculatorModel`;
- kết quả đúng nhưng màu lỗi không đổi: kiểm tra class trong `render()` và selector CSS;
- giao diện vỡ ở 320 px: kiểm tra width, grid và media query.

---

## 13. Những nguyên tắc thiết kế học được từ dự án

### 13.1. Tách cấu trúc, trình bày và hành vi

- HTML trả lời: “Có những thành phần nào?”
- CSS trả lời: “Các thành phần trông như thế nào?”
- JavaScript trả lời: “Các thành phần phản ứng ra sao?”

### 13.2. State là nguồn sự thật

Kết quả hiển thị nên được tạo từ state. Khi người dùng hành động, hãy đổi state rồi render lại.

### 13.3. Tách logic khỏi DOM

`calculator.js` không cần biết một nút có màu gì hoặc nằm ở đâu. Vì vậy có thể unit test logic bằng Node.js mà không cần mở trình duyệt.

### 13.4. Thiết kế từ yêu cầu có thể kiểm tra

Yêu cầu như “hỗ trợ màn hình nhỏ” còn mơ hồ. Test “viewport 320 px không cuộn ngang” cụ thể hơn và có thể xác nhận.

### 13.5. Xử lý cả trường hợp bình thường và biên

Không chỉ thử `2 + 3`. Cần thử chia cho `0`, hai dấu chấm, hai toán tử liên tiếp, backspace và nhập sau khi vừa tính xong.

### 13.6. Accessibility không phải phần bổ sung cuối cùng

Dùng `<button>`, focus-visible, keyboard và ARIA ngay từ đầu tạo ra giao diện tốt hơn cho mọi người.

### 13.7. MVP phải nhỏ nhưng hoàn chỉnh

**Minimum Viable Product** là phiên bản nhỏ nhất vẫn giải quyết được mục tiêu chính. Calculator chưa có mọi tính năng, nhưng các tính năng đã công bố phải hoạt động và được kiểm thử.

---

## 14. Từ điển thuật ngữ ngắn

- **Accessibility**: khả năng để nhiều nhóm người dùng có thể sử dụng sản phẩm.
- **Attribute**: thuộc tính nằm trong thẻ HTML, ví dụ `id` hoặc `data-value`.
- **Browser**: trình duyệt chạy và hiển thị ứng dụng web.
- **CSS selector**: biểu thức chọn phần tử để áp dụng CSS.
- **Deploy**: đưa ứng dụng lên môi trường có thể truy cập.
- **DOM**: mô hình object mà trình duyệt tạo từ HTML.
- **Event**: thông báo về một việc vừa xảy ra.
- **Event listener**: hàm chờ và xử lý một loại event.
- **Git**: công cụ quản lý phiên bản.
- **GitHub Pages**: dịch vụ lưu trữ website tĩnh từ GitHub.
- **HTML element**: một thành phần trong tài liệu HTML.
- **JavaScript module**: file JavaScript có thể import hoặc export.
- **MVP**: phiên bản nhỏ nhất có đủ giá trị sử dụng chính.
- **Render**: biến dữ liệu/state thành giao diện.
- **Responsive**: khả năng giao diện thích ứng với kích thước màn hình.
- **State**: dữ liệu mô tả ứng dụng tại một thời điểm.
- **Static web app**: ứng dụng được phục vụ bằng các file tĩnh, không cần xử lý phía server cho chức năng chính.
- **Unit test**: kiểm thử một phần logic nhỏ và độc lập.
- **Viewport**: vùng trang web đang hiển thị trong cửa sổ trình duyệt.

---

## 15. Bài tự kiểm tra khả năng giải thích lại

Hãy thử trả lời bằng lời của mình, không nhìn lại tài liệu:

1. HTML, CSS và JavaScript có vai trò gì?
2. DOM khác file HTML ban đầu ở điểm nào?
3. Event là gì và calculator đang nghe những event nào?
4. State gồm những dữ liệu nào?
5. Vì sao cần gọi `render()` sau khi xử lý action?
6. Tại sao `calculator.js` được tách khỏi `app.js`?
7. Vì sao keypad dùng CSS Grid?
8. Media query giúp gì cho màn hình 320 px?
9. Vì sao unit test đạt vẫn cần kiểm thử trên trình duyệt?
10. GitHub Pages làm gì khi repository có `index.html`?

Nếu có thể trả lời mạch lạc mười câu này và mô tả lại luồng `Event → State → Render → DOM`, bạn đã nắm được phần cốt lõi của dự án.

---

## 16. Mẫu giải thích dự án trong hai phút

> Hello Calculator là một ứng dụng web tĩnh dùng HTML, CSS và JavaScript. HTML tạo cấu trúc gồm màn hình, keypad và bảng học tập. CSS dùng Flexbox, Grid, biến màu và media query để giao diện rõ ràng, responsive từ khoảng 320 px và hỗ trợ focus bàn phím. JavaScript được tách thành model tính toán và phần kết nối DOM. Khi người dùng click hoặc nhấn phím, event được chuyển thành một action. Model xử lý action và thay đổi state, sau đó hàm render đọc state để cập nhật DOM. Logic được unit test bằng Node.js, còn giao diện được kiểm thử thủ công trên trình duyệt. Vì dự án chỉ gồm file tĩnh nên có thể chạy bằng Python HTTP server và triển khai trực tiếp lên GitHub Pages.

Đây là câu trả lời ngắn gọn bao quát toàn bộ kiến trúc của MVP.

