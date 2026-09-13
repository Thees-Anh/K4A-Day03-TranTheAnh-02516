# 📊 BÁO CÁO THU HOẠCH NGHIỆM THU BÀI LAB 3 (BƯỚC 3 — SUBMISSION ARTIFACT)

> **Họ và Tên Học viên:** Trần Thế Anh
> **Mã Sinh Viên / Mã Học viên:** 2A202602516
> **Chủ đề Lựa chọn:** Trợ lý Học vụ và Tra cứu Lịch tư vấn VinUni

---

## 1. BẢNG CHẤM ĐIỂM AGENTIC FIT SCORING MATRIX (ĐÁNH GIÁ CHỦ ĐỀ)

| Tiêu chí Đánh giá | Mức độ (1 - 5) | Giải trình chi tiết lý do chọn điểm |
| :--- | :---: | :--- |
| **1. Multi-step Reasoning** | 4 / 5 | Agent có thể phải tra cứu sinh viên, lấy tên cố vấn rồi đặt lịch. |
| **2. Tool Interaction** | 5 / 5 | Agent bắt buộc gọi công cụ tra cứu và đặt lịch qua MCP Server. |
| **3. Dynamic Decision** | 4 / 5 | Có đặt lịch tiếp hay không phụ thuộc kết quả tra cứu trước đó. |
| **4. Long Horizon Goal** | 3 / 5 | Agent giữ mục tiêu qua vài bước, nhưng quy trình chưa quá dài. |
| **TỔNG ĐIỂM AGENTIC FIT** | 16 / 20 | Trên 12, phù hợp triển khai Agent. |

---

## 2. TRÍCH XUẤT KẾT QUẢ WATERFALL TRACE LOG (SAU KHI CHẠY TEST SUITE TRÊN API THẬT)

> ⚠️ **YÊU CẦU NGHIỆM THU:** Mở tệp `.env` điền `GEMINI_API_KEY` (hoặc `OPENAI_API_KEY`) để kết nối LLM thật trước khi thực thi `python src/app.py --all`. Bài nộp chỉ dùng Mock Offline Provider sẽ không đạt điểm nghiệm thực tế.

Đoạn trace tiêu biểu của TC04 cho thấy Gemini API thật thực hiện chuỗi ReAct đa bước: tra cứu cố vấn rồi đặt lịch:

```json
[
  {
    "step": 1,
    "query": "Hãy tra cứu thông tin và cố vấn của sinh viên SV2026002, sau đó đặt lịch tư vấn với cố vấn đó vào 09:00 ngày 16/09/2026.",
    "action_type": "TOOL_EXECUTION",
    "thought": "Gemini (gemini-flash-lite-latest) quyết định gọi công cụ 'academic_query' với tham số: {\"student_id\": \"SV2026002\"}",
    "provider": "gemini",
    "model": "gemini-flash-lite-latest",
    "tool_name": "academic_query",
    "arguments": {
      "student_id": "SV2026002"
    },
    "observation": {
      "status": "SUCCESS",
      "student_id": "SV2026002",
      "data": {
        "full_name": "Trần Thị Bình",
        "class": "AI-K4",
        "gpa": 3.6,
        "advisor": "TS. Lê Thị B"
      }
    },
    "latency_ms": 34793.0
  },
  {
    "step": 2,
    "query": "Hãy tra cứu thông tin và cố vấn của sinh viên SV2026002, sau đó đặt lịch tư vấn với cố vấn đó vào 09:00 ngày 16/09/2026.",
    "action_type": "TOOL_EXECUTION",
    "thought": "Gemini (gemini-flash-lite-latest) quyết định gọi công cụ 'schedule_appointment' với tham số: {\"datetime_str\": \"09:00 ngày 16/09/2026\", \"advisor_name\": \"TS. Lê Thị B\", \"student_id\": \"SV2026002\"}",
    "provider": "gemini",
    "model": "gemini-flash-lite-latest",
    "tool_name": "schedule_appointment",
    "arguments": {
      "student_id": "SV2026002",
      "datetime_str": "09:00 ngày 16/09/2026",
      "advisor_name": "TS. Lê Thị B"
    },
    "observation": {
      "status": "SUCCESS",
      "booking_id": "BK-SV2026002-99",
      "message": "Đặt lịch thành công cho sinh viên SV2026002 với TS. Lê Thị B vào lúc 09:00 ngày 16/09/2026."
    },
    "latency_ms": 35175.91
  },
  {
    "step": 3,
    "query": "Hãy tra cứu thông tin và cố vấn của sinh viên SV2026002, sau đó đặt lịch tư vấn với cố vấn đó vào 09:00 ngày 16/09/2026.",
    "action_type": "FINAL_ANSWER",
    "thought": "Tổng hợp kết quả từ MCP Server thành công.",
    "provider": "gemini",
    "model": "gemini-flash-lite-latest",
    "output": "Đặt lịch thành công cho sinh viên SV2026002 với TS. Lê Thị B vào lúc 09:00 ngày 16/09/2026.",
    "latency_ms": 10.0
  }
]
```

> **Trạng thái lượt nghiệm thu:** Cả 5 test đã được xử lý bằng Gemini API thật; Waterfall Trace có 10 sự kiện, gồm 5 lần gọi Tool chính xác, không có `API_ERROR` và không fallback sang Mock.

---

## 3. TỔNG KẾT KẾT QUẢ NGHIỆM THU & NỘP BÀI

- [x] Đã điền API Key thật trong `.env` và kết nối thành công với `GeminiProvider`.
- [x] Đã xác nhận toàn bộ 5 test chạy trên LLM API thật mà không fallback sang Mock.

| Test Case | Kết quả quan sát | Provider thực tế | Đánh giá |
| :--- | :--- | :---: | :---: |
| **TC01** | Trả lời trực tiếp, không gọi Tool. | Gemini API | Đạt |
| **TC02** | Gọi đúng `academic_query` với `SV2026001`, nhận `SUCCESS` và tổng hợp câu trả lời. | Gemini API | Đạt |
| **TC03** | Gọi đúng `schedule_appointment` và đặt lịch thành công. | Gemini API | Đạt |
| **TC04** | Gọi `academic_query` với `SV2026002`, lấy cố vấn `TS. Lê Thị B`, rồi gọi `schedule_appointment`. | Gemini API | Đạt |
| **TC05** | Gọi `academic_query` với `SV9999999` và xử lý đúng kết quả `NOT_FOUND`. | Gemini API | Đạt |

- **Tổng số Test Cases đã thực thi:** 5 / 5 test cases.
- **Tổng số Test Cases đạt đúng hành vi mong đợi:** 5 / 5 test cases.
- **Tổng số Test Cases nghiệm thu thành công trên API thật:** 5 / 5 test cases.
- **Số lượt gọi Tool qua MCP Server chính xác từ API thật:** 5 lượt.
- **Kết quả Waterfall Trace:** 10 sự kiện, 0 lỗi API.
- **Kết quả đẩy Repo nộp bài:** [x] Đã Commit và Push mã nguồn thành công lên GitHub cá nhân.

---

> ✅ **HOÀN TẤT NỘP BÀI:** Sao chép đường link GitHub Repository cá nhân của bạn và dán vào ô nộp bài trên hệ thống LMS VLearn để hoàn tất Bài Lab 3!
