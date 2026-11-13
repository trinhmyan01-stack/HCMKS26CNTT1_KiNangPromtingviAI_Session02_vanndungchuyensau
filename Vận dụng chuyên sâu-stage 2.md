Thiết Kế Giải Pháp Prompting Ứng Dụng Thực Tế

Bước 1: Thiết kế giải pháp (Input/Output)

Bối cảnh & Mục tiêu

Tự động hóa hệ thống phân tích đánh giá khách hàng tại TechShop, đảm bảo tích hợp khung CREATE, mẫu học nhanh Few-shot, ép định dạng JSON chặt chẽ, và cơ chế xử lý lỗi khi dữ liệu đầu vào bị thiếu hoặc nhiễu.

Xác định Input và Output

Input: Chuỗi đánh giá thô của khách hàng (hoặc chuỗi rỗng / dữ liệu không hợp lệ để kiểm tra khả năng bắt lỗi).

Output: Định dạng cấu trúc JSON nghiêm ngặt với các trường chuẩn hóa, bao gồm cả trường xử lý lỗi (status và error_message) nhằm ngăn chặn tình trạng AI sinh dữ liệu ảo khi gặp dữ liệu trống.

Bước 2: Viết Prompt Hoàn Chỉnh và Chạy Thử

1. Prompt hoàn chỉnh (Áp dụng CREATE, Few-shot & Điều kiện xử lý lỗi)

[Context] Bạn là hệ thống AI cốt lõi trong quy trình Đảm bảo Chất lượng tại TechShop.
[Role] Chuyên gia xử lý dữ liệu NLP và phân tích cảm xúc khách hàng.
[Explicit Instructions] 

Phân tích đánh giá sản phẩm đầu vào.

Xử lý điều kiện lỗi: Nếu Input bị trống, null hoặc không chứa nội 
dung hợp lệ, KHÔNG tự suy diễn mà phải trả về JSON lỗi với status: "error".

Bắt buộc trả về kết quả ĐÚNG định dạng JSON thuần túy (không kèm markdown block phụ hay văn bản giải thích bên ngoài).

[Output Schema Format]
{
  "status": "success" | "error",
  "sentiment": "positive" | "negative" | "neutral" | null,
  "category": "temperature" | "battery" | "shipping" | "general" | null,
  "summary": "...",
  "error_message": "..." 
}

[Few-shot Examples]
Ví dụ 1 (Đầu vào hợp lệ):
Input: "Máy thiết kế đẹp mê ly, mùa đông ôm máy chơi game khỏi cần lò sưởi vì nhiệt độ lên 98 độ C!"
Output: {"status": "success", "sentiment": "negative", "category": "temperature", "summary": "Mỉa mai lỗi quá nhiệt (98 độ C)", "error_message": null}

Ví dụ 2 (Đầu vào bị trống / lỗi):
Input: ""
Output: {"status": "error", "sentiment": null, "category": null, "summary": null, "error_message": "Dữ liệu đầu vào bị trống hoặc không hợp lệ."}

[Task]
Input: "Pin sụt nhanh như tụt huyết áp, sạc 100% rút ra chưa đầy 5 phút sập nguồn."
Output:


2. Kết quả phản hồi mẫu từ AI (Chạy thử)

{
  "status": "success",
  "sentiment": "negative",
  "category": "battery",
  "summary": "Phản ánh lỗi pin ảo, tụt nguồn nhanh chóng",
  "error_message": null
}
