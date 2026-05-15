# Tra cứu Dược điển

Website tra cứu và so sánh tiêu chuẩn QC giữa các Dược điển (DĐVN, USP, BP). Kết nối với workflow n8n để scrape, trích xuất chuyên luận và phân tích bằng AI Agent.

## Truy cập

🌐 **Live demo:** https://YOUR_USERNAME.github.io/tra-cuu-duoc-dien/

## Tính năng

- Form điền tên hoạt chất + dạng bào chế (datalist gợi ý 36 dạng phổ biến)
- Gọi webhook n8n để trigger workflow xử lý
- Hiển thị kết quả markdown từ AI Agent (bảng so sánh, đánh giá khắt khe, lưu ý QC)
- In / Lưu PDF với print stylesheet tối ưu
- Lịch sử tra cứu (sidebar)
- Cấu hình webhook URL trực tiếp trên giao diện

## Kết nối n8n

Workflow n8n cần có:

1. **Webhook node** (đầu workflow)
   - HTTP Method: `POST`
   - Path: UUID của webhook
   - Respond: `Using 'Respond to Webhook' Node`

2. **Respond to Webhook node** (cuối workflow)
   - Respond With: `First Incoming Item` (trả về output của AI Agent)
   - Response Headers (Options → Add Header):
     - `Access-Control-Allow-Origin: *`
     - `Access-Control-Allow-Methods: POST, OPTIONS`
     - `Access-Control-Allow-Headers: Content-Type`

## Cấu trúc request

Website gửi `POST` đến webhook với body:

```json
{
  "Tên hoạt chất": "Paracetamol",
  "Dạng bào chế": "Viên nén",
  "active_ingredient": "Paracetamol",
  "dosage_form": "Viên nén"
}
```

Workflow trả về JSON có field `output` chứa markdown từ AI Agent:

```json
[
  {
    "output": "📋 **Đang phân tích đối tượng:** Paracetamol Tablets\n\n## 1. Bảng so sánh tiêu chuẩn\n..."
  }
]
```

## Local development

Mở trực tiếp `index.html` trong trình duyệt, hoặc chạy local server:

```bash
python -m http.server 8080
```

Sau đó truy cập http://localhost:8080
