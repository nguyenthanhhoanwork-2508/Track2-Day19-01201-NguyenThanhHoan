# Reflection — Lab 19

**Tên:** Nguyen Thanh Hoan - 2A202601201
**Cohort:** A20-K3
**Path đã chạy:** lite

---

## Câu hỏi (≤ 200 chữ)

> Trên golden set 50 queries, mode nào thắng ở loại query nào (`exact` /
> `paraphrase` / `mixed`), và tại sao? Khi nào bạn **không** dùng hybrid
> (i.e. khi nào pure BM25 hoặc pure vector là lựa chọn đúng)?

Số liệu Precision@10 đo được trên 50 golden query: Keyword 77.8%, Semantic
73.2%, Hybrid (RRF k=60) 78.6% — hybrid thắng trung bình tổng thể, đúng
kỳ vọng của rubric.

Nhìn theo slice thì bức tranh không đồng nhất. Ở `exact` (câu hỏi đúng
từ khóa trong tài liệu), Keyword và Hybrid ngang nhau (96.7%), Semantic
tụt lại (88.7%) — match từ khóa chính xác là điểm mạnh tự nhiên của
BM25, và vì RRF cho rank BM25 trọng số cao khi nó đã đúng, Hybrid kế
thừa luôn ưu thế đó. Ở `paraphrase` (diễn đạt lại, không trùng từ khóa),
cả ba mode đều tụt điểm rõ rệt, và bất ngờ là Keyword (33.3%) lại nhỉnh
hơn cả Hybrid (32.0%) lẫn Semantic (24.0%) — cho thấy corpus vẫn còn
nhiều overlap từ vựng ẩn giữa câu paraphrase và tài liệu gốc, nên BM25
"ăn may" đúng từ khóa còn embedding bge-small (384d, tiếng Anh) chưa đủ
mạnh để nắm nghĩa tiếng Việt diễn đạt lại. Ở `mixed`, Hybrid thắng rõ
nhất (100% so với 97–98.5%) vì đây đúng là trường hợp lý tưởng của RRF:
tận dụng được cả tín hiệu từ khóa lẫn tín hiệu ngữ nghĩa cùng lúc.

Kết luận thực dụng: không cần Hybrid khi query gần như chắc chắn là
tra cứu từ khóa/mã sản phẩm chính xác (dùng pure BM25 là đủ, tiết kiệm
một lượt gọi embedding + latency). Pure Semantic một mình thì trong lần
đo này không thắng ở slice nào — chỉ nên dùng riêng khi đổi sang model
embedding mạnh hơn (bge-m3) cho câu hỏi diễn đạt lại nhiều.

---

## Điều ngạc nhiên nhất khi làm lab này

Ở slice `paraphrase`, thuần Keyword (BM25) lại nhỉnh hơn Hybrid — ngược
với giả định ban đầu rằng cứ diễn đạt lại thì semantic/hybrid phải thắng.
Bài học: hybrid chỉ tốt bằng chất lượng của model embedding bên dưới; khi
model yếu (bge-small, tiếng Anh) trên câu hỏi tiếng Việt, RRF không "cứu"
được điểm semantic thấp.

---

## Bonus challenge

- [ ] Đã làm bonus (xem `bonus/`)
- [ ] Pair work với: _<tên đồng đội nếu có>_
