# Sản khoa Y4

Website ôn tập bằng tiếng Việt cho sinh viên Y khoa năm 4. Ngân hàng khởi đầu được biên soạn từ hai thư mục tài liệu học, đối chiếu nguồn chính thống tại những điểm cần cập nhật. Xem số lượng thực tế và độ phủ tài liệu ở `reports/bank-validation.json`, cùng ghi chú chuyên môn ở `reports/source-review.md`.

## Sử dụng website

1. Nhấn **Tạo đề kiểm tra** ở trang chủ.
2. Mặc định có **90 câu / 90 phút**, với nhận biết 10%, thông hiểu 15%, vận dụng 40%, vận dụng cao 35%. Đề 90 câu lấy tương ứng **9 / 13 / 36 / 32** câu. Có thể thay số câu, thời gian, tỉ lệ, chủ đề và bài học.
3. Nhấn **Bắt đầu làm bài**, chọn một đáp án đúng nhất. Dùng **Xem lại sau**, **Câu tiếp**, **Câu trước** hoặc danh sách số câu để di chuyển.
4. Nhấn **Nộp bài** rồi **Xác nhận**, hoặc để bài tự hoàn tất khi hết thời hạn. Kết quả hiển thị số đúng/tổng, sai và bỏ trống; không quy đổi thang điểm.
5. Mở từng câu trong **Đáp án & lời giải** để xem căn cứ tài liệu và cập nhật guideline. **Ôn những câu đã sai** tạo bài luyện không giới hạn thời gian từ các câu trả lời sai.
6. Trong **Cài đặt & sao lưu**, xuất lịch sử và ngân hàng JSON/CSV để chuyển thủ công giữa các thiết bị. Bản dự phòng bài đang làm có thể được khôi phục mà giữ nguyên thời hạn.

Khi nhập lịch sử, bài có cùng ID đã tồn tại trên thiết bị được giữ nguyên; số bài được nhập và bỏ qua được thông báo. Bản dự phòng không mở lại bài đã nộp có cùng ID.

Website lưu bài trong IndexedDB của trình duyệt. Lịch sử không tự đồng bộ giữa điện thoại, máy tính hoặc trình duyệt. Khi đóng trang, thời hạn vẫn tiếp tục: nếu mở lại sau hạn, bài được hoàn tất với các đáp án đã lưu. Đồng hồ trên thiết bị và đáp án phát hành cùng website phù hợp với tự ôn tập, không tạo hệ thống chống gian lận cho thi chính thức.

Tính năng đọc PDF/DOCX/TXT giúp tham khảo văn bản trên thiết bị; không tự sinh câu hỏi mới từ tài liệu bất kỳ. PDF scan không có lớp chữ cần đọc ở bản gốc. Tệp nhập tối đa 25 MB; trình đọc PDF tối đa 500 trang.

## Nguồn câu hỏi và ảnh

Mỗi câu có ID tài liệu và số trang, slide hoặc đoạn. `src/data/sources.json` chỉ chứa danh mục tham chiếu. Văn bản trích xuất và các tài liệu gốc nằm ngoài phần triển khai công khai.

Câu yêu cầu đọc ảnh có bệnh cảnh luyện tập cụ thể, ảnh thật và câu hỏi rõ ràng. Ảnh hiện tại gồm hai siêu âm từ JETem, dùng trong sáu mục tiêu học tập; ảnh được giữ nguyên và có ghi nguồn, tác giả, giấy phép trong lời giải. Các câu xử trí có thể dùng kết quả báo cáo xét nghiệm hoặc siêu âm bằng văn bản như dữ kiện của bệnh cảnh.

Ảnh theo [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/):

- Farhat, Hoffmann, Alvarado (2017), [Point-of-care Ultrasound for the Diagnosis of Ectopic Pregnancy](https://jetem.org/ectopic_pregnancy/).
- Wietecha, Williams, Slane (2022), [Point-of-Care Ultrasound to Diagnose Molar Pregnancy: A Case Report](https://jetem.org/molar/).

Mức độ là phân loại theo yêu cầu tư duy khi biên soạn, chưa được hiệu chỉnh bằng dữ liệu kết quả học tập của một nhóm sinh viên. Các ghi chú nguồn chỉ rõ điểm mâu thuẫn, lỗi tài liệu và nội dung chưa kiểm chứng đủ.

## Chạy và kiểm tra mã nguồn

Cần Node.js 24 và pnpm 11.25.0 trên máy phát triển. Khi website đã triển khai, người học chỉ cần trình duyệt.

```sh
pnpm install --frozen-lockfile
pnpm dev
```

Kiểm tra trước khi cập nhật:

```sh
pnpm validate:bank
pnpm test
pnpm build
```

`pnpm build` tạo thư mục `dist` và service worker chứa danh sách tệp ngoại tuyến. Không chạy tiến trình Node.js ở nền tảng hosting. Giao diện dùng đường dẫn hash, ví dụ `/#create`, nên tải lại trang con không cần cấu hình API hoặc định tuyến máy chủ.

Các kiểm tra giao diện dùng Playwright và Google Chrome. Mở `pnpm preview` ở một cửa sổ terminal, rồi chạy `pnpm test:e2e` ở cửa sổ khác. Trên máy khác, đặt biến `CHROME_BIN` thành đường dẫn Chrome; có thể dùng `QUIZ_TEST_URL` để đổi địa chỉ bản preview. Kết quả kiểm thử được tóm tắt ở `reports/software-checks.json`. Báo cáo Playwright thô chứa đường dẫn máy được giữ cục bộ và loại khỏi gói phát hành. Không suy ra rằng các trình duyệt khác đã được kiểm tra từ kết quả Chrome này.

## Xuất bản qua GitHub Pages

Đã chuẩn bị `.github/workflows/deploy.yml`: tự kiểm tra ngân hàng, chạy Vitest, build và triển khai sau khi cập nhật nhánh `main`.

1. Dùng tài khoản GitHub, tạo repository công khai tên **san-khoa-y4**. Repository công khai dùng được GitHub Pages với GitHub Free. Không tải các thư mục tài liệu học gốc lên repository.
2. Đưa mã nguồn website vào repository, gồm `src`, `public`, `scripts`, `.github`, cấu hình dự án và lockfile. Không tải `node_modules`, `.local-sources`, `.local-qa` hoặc các bản sao lưu cá nhân.
3. Trong repository, nhấn **Settings** → **Pages**.
4. Ở mục **Build and deployment**, chọn **Source: GitHub Actions**.
5. Mở thẻ **Actions**, chọn **Deploy static website to GitHub Pages**. Nếu cần, nhấn **Run workflow** và chọn nhánh `main`.
6. Khi tác vụ hoàn tất thành công, quay lại **Settings → Pages**, nhấn **Visit site** để lấy địa chỉ thực tế. Mẫu địa chỉ là `https://TEN-TAI-KHOAN.github.io/san-khoa-y4/`; đây là mẫu, không phải địa chỉ đã triển khai của dự án.

Workflow tự đặt `VITE_BASE_PATH` theo tên repository. Nếu dùng repository cấp tài khoản `TEN-TAI-KHOAN.github.io`, hãy đổi biến đó trong workflow thành `/`. Nếu build thủ công cho một repository có tên khác, đặt `VITE_BASE_PATH=/TEN-REPOSITORY/` khi chạy build.

Khi tải qua giao diện web, có thể đưa `san-khoa-y4-source.zip`, `README.md` và `.github/workflows/deploy.yml` vào repository. Workflow nhận diện gói ZIP, giải nén mã nguồn rồi chạy cùng các bước kiểm tra/build như khi upload toàn bộ cây thư mục. Để cập nhật theo cách này, thay gói ZIP bằng phiên bản mới; để chỉnh sửa bằng Git, giải nén gói rồi dùng cây mã nguồn đầy đủ.

Tham khảo [hướng dẫn GitHub Pages chính thức](https://docs.github.com/en/pages/getting-started-with-github-pages/creating-a-github-pages-site). Gói miễn phí có giới hạn và có thể thay đổi theo nhà cung cấp.

## Phương án Cloudflare Pages

Bản `san-khoa-y4-static.zip` chứa đúng nội dung `dist`, có `index.html` ở gốc, dùng đường dẫn tương đối. Có thể tải trực tiếp theo [hướng dẫn Direct Upload chính thức](https://developers.cloudflare.com/pages/get-started/direct-upload/): trong Cloudflare, mở **Workers & Pages** → tạo ứng dụng Pages → tải tệp ZIP → **Deploy site**. Chỉ dùng Pages để phục vụ tệp tĩnh; dự án không chứa Pages Functions hay Workers.

Nếu dùng tích hợp repository để tự build, lệnh build là `pnpm build`, thư mục output là `dist`, phiên bản Node là 24. Không đặt `VITE_BASE_PATH` theo repository khi triển khai ở gốc tên miền `pages.dev`.

## Cập nhật và bổ sung câu hỏi

Các câu nằm trong `src/data/bank-a.json`, `bank-b.json`, `bank-c.json`, `imaging.json`, được ghép ở `src/data/index.ts`. Có thể thêm một tệp JSON rồi nhập vào danh sách này. Chạy kiểm tra và triển khai lại để cập nhật ngân hàng dùng chung.

`correctAnswer` là vị trí phương án trong dữ liệu gốc, tính từ 0 đến 3. Thuật toán giữ ánh xạ này khi xáo phương án. Không chỉnh đáp án theo chữ A/B/C/D của một đề đã tạo.

```json
{
  "id": "LOCAL-001",
  "subject": "Sản khoa",
  "grade": 4,
  "chapter": "Ba tháng đầu thai kỳ",
  "lesson": "Tên bài học",
  "topic": "thai-som",
  "difficulty": 3,
  "type": "multiple_choice",
  "clinicalCase": "Tình huống luyện tập: bệnh cảnh đủ dữ kiện...",
  "question": "Câu hỏi cụ thể?",
  "options": ["Phương án 1", "Phương án 2", "Phương án 3", "Phương án 4"],
  "correctAnswer": 0,
  "explanation": "Giải thích và phân biệt những lựa chọn dễ nhầm...",
  "source": { "documentId": "ID-từ-danh-mục-nguồn", "locator": "trang 1" }
}
```

Câu ảnh bổ sung trường `image` với `src`, `alt`, `caption`, `sourceUrl`, `credit`, `license`. Đặt ảnh có giấy phép phù hợp trong `public/images`, cung cấp bệnh cảnh và kiểm tra ảnh thực sự hỗ trợ câu hỏi. Ghi nguồn cập nhật ở `updates` để phân biệt với căn cứ tài liệu học. Không tự sửa ảnh chẩn đoán hoặc dùng ảnh AI làm chứng cứ bệnh lý.

Để thêm môn hoặc năm học, dùng các giá trị `subject` và `grade` mới trong ngân hàng. Bộ lọc tạo đề đọc các giá trị này từ dữ liệu và tách riêng môn/năm học; chủ đề mới được nhận diện từ `topic` và `chapter`. Có thể thêm mô tả, màu, biểu tượng cho chủ đề ở `TOPICS` trong `src/types.ts`. Nhãn thương hiệu hiện mang tên Sản khoa Y4; có thể đổi riêng phần nhãn nếu biến website thành cổng ôn nhiều môn.

Ngân hàng chỉnh sửa qua giao diện chỉ có hiệu lực trên thiết bị đó. Xuất ngân hàng rồi cập nhật repository nếu muốn mọi người nhận phiên bản mới. Liên kết chia sẻ chỉ tái tạo đề khi ngân hàng và thuật toán khớp phiên bản; đề ôn câu sai hoặc đề thuộc ngân hàng cũ có thể giữ nguyên bằng xuất lịch sử.

## Cấu trúc dự án

- `src/core/engine.ts`: lọc, chia mức độ, chọn câu theo seed, xáo đáp án, chấm số câu và tái tạo đề.
- `src/core/storage.ts`: lưu bài và ngân hàng cục bộ.
- `src/core/validation.ts`, `files.ts`: kiểm tra và nhập/xuất dữ liệu.
- `src/data`: ngân hàng và danh mục nguồn.
- `src/App.tsx`, `src/components`, các tệp CSS: giao diện.
- `scripts/prepare-offline.mjs`: đóng gói ngoại tuyến.
- `scripts/validate-bank.mjs`, `src/core/engine.test.ts`, `tests`: kiểm tra dữ liệu, thuật toán và giao diện.

Các công cụ trích xuất và biên soạn `author-*.py` được giữ cục bộ cùng dữ liệu nguồn riêng, loại khỏi Git và gói phát hành. Gói mã nguồn chứa toàn bộ ngân hàng JSON có thể chỉnh sửa cùng các công cụ chạy, kiểm tra và build website.
