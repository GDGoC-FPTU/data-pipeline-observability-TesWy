# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** AI20K-2A202600084
**Name:** Huynh Nhut Huy
**Date:** 15/04/2026

---

## 1. Ket qua thi nghiem

Chay `agent_simulation.py` voi 2 bo du lieu va ghi lai ket qua:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | Based on my data, the best choice is Laptop at $1200. |10| Chon dung|
| Garbage Data (`garbage_data.csv`) |Based on my data, the best choice is Nuclear Reactor at $999999. |0| Chon sai|

---

## 2. Phan tich & nhan xet

### Tai sao Agent tra loi sai khi dung Garbage Data?

(Viet nhan xet cua ban o day — it nhat 50 tu)

(Hay phan tich cac van de nhu Duplicate IDs, wrong data types, outliers, null values
va giai thich tai sao chung anh huong den ket qua cua Agent.)
Agent sẽ xử lý bất cứ thứ gì được cung cấp và đưa ra câu trả lời dựa trên đó. Khi dữ liệu đầu vào là garbage_data.csv. Dữ liệu bị lỗi nhiều ở :
    -Duplicate IDs : Các bản ghi bị lặp ID khiến Agent tính toán nhầm tần suất vì vậy ưu tiên một sản phẩm không thực sự phổ biến. Khi một sản phẩm như "Nuclear Reactor" xuất hiện nhiều lần do duplicate, Agent có thể hiểu đây là item nổi bật nhất.
    -Wrong data types: Trường price chứa chuỗi ký tự thay vì số (ví dụ: "nine hundred", "N/A") khiến Agent không thể so sánh giá trị chính xác. Kết quả là logic chọn sản phẩm tốt nhất bị sai hoàn toàn.
    -Outliers (giá trị ngoại lệ): Giá $999,999 cho "Nuclear Reactor" là một outlier cực đoan. Nếu Agent dùng logic "chọn sản phẩm giá cao nhất = tốt nhất", nó sẽ chọn ngay outlier này mà không có bước lọc nào.
    -Null/Empty values: Các trường category hoặc product bị null khiến Agent thiếu ngữ cảnh để đánh giá. Khi tên sản phẩm là None hoặc rỗng, Agent có thể fallback về bản ghi khác có dữ liệu — dù bản ghi đó là rác.
Tóm lại Garbage in = Garbage out. Agent thời điểm hiện tại không có trí thông minh tự nhiên để chọn lọc đầu vào nên chất lượng AI sẽ phụ thuộc hoàn toàn vào input được cung cấp

---

## 3. Ket luan

**Quality Data > Quality Prompt?** (Dong y hay khong? Giai thich ngan gon.)
Đồng ý. 
Một prompt được viết hoàn hảo sẽ vô dụng nếu dữ liệu đầu vào chứa outliers, null values hay sai kiểu dữ liệu. Trong thí nghiệm này, cùng một Agent với cùng một logic xử lý — khi nhận processed_data.csv thì trả lời đúng hoàn toàn (accuracy = 10/10), nhưng khi nhận garbage_data.csv thì cho kết quả vô nghĩa (accuracy = 0/10). Điều đó chứng minh rằng ETL pipeline và data validation là nền tảng bắt buộc trước khi đưa dữ liệu vào bất kỳ AI Agent nào. Prompt engineering chỉ phát huy tác dụng khi dữ liệu đã được làm sạch.
(Viet ket luan cua ban o day)
