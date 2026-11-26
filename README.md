# Hỏi đáp pháp luật Việt Nam theo thời điểm

Repo này chứa đủ thứ để viết paper và khóa luận: bộ dữ liệu ViLexTime, nhãn, kết quả thực nghiệm và code.

## Cần gì, mở ở đâu

| Cần | Mở |
|---|---|
| Bộ dữ liệu dùng cho kết quả chính (722 dòng) | `data/benchmark/final_test/` |
| Bản công bố của bộ dữ liệu (6.0.0) | `data/benchmark/public_release_v6/` |
| Bảng kết quả chính | `runs/final_test/main/r1/results.md` |
| Con số nào trong paper lấy từ file nào | `docs/paper/ieee/numbers.md` |
| Nhãn người, độ đồng thuận | `data/labels/` |
| Bản thảo paper, khóa luận | `docs/paper/`, `docs/thesis/` |
| Nhật ký từng lần chạy | `runs/LOG.md` |

Mỗi thư mục `data/`, `runs/`, `docs/` có một README ngắn giải thích từng thư mục con.

## Cây thư mục

```
data/       bộ dữ liệu và nhãn
runs/       kết quả thực nghiệm
docs/       paper, khóa luận, quyết định, báo cáo
src/        code hệ hỏi đáp
scripts/    chạy, chấm điểm, dựng số liệu cho paper
prompts/    lời dặn gửi LLM
tests/      kiểm tra code
```

## Đọc tên file kết quả

Trong `runs/`, mỗi hệ là một file `<mã hệ>.jsonl`. Paper dùng tên dễ đọc hơn:

| Mã trong file | Tên trong paper | Làm gì |
|---|---|---|
| `b1` | LLM only | Không truy xuất |
| `b2ctx` | BM25 | Tìm theo từ khóa |
| `b3` | Dense | Tìm theo nghĩa |
| `b4` | Hybrid | Từ khóa + nghĩa |
| `b5` | Hybrid+AmendHop | Hybrid, mở rộng theo quan hệ sửa đổi |
| `b6` | Hybrid+Hierarchy | Hybrid, mở rộng theo cây điều khoản |
| `b7` | +DocDate | Lọc theo ngày hiệu lực của cả văn bản |
| `b7v` | +VersionStore | Dùng lời văn từng phiên bản |
| `b7plus` | +VersionFilter | Lọc theo khoảng hiệu lực của phiên bản |
| `b7pp` | +Propagation | Lọc theo hiệu lực có lan truyền từ điều khoản cha |
| `proposed2` | Full config (TV-RAG) | Cấu hình đầy đủ, chọn trước trên dev |
| `bvnone` | Unfiltered versions | Lời văn phiên bản, không lọc |
| `bvdate` | Dates in prompt | Như trên, in ngày hiệu lực vào prompt |
| `oracle` | Oracle (in force) | Đưa đúng phiên bản có hiệu lực |
| `oracle_now` | Oracle (current) | Đưa phiên bản hiện hành |
| `lightrag` | LightRAG | Baseline đồ thị bên ngoài |

Một số mã khác:

- `r1`, `r2`: lần chạy thứ nhất và thứ hai (gọi API hai lần).
- `giua_khoang`: hỏi ở một ngày giữa khoảng hiệu lực. Bảng chính dùng chế độ này.
- `T1`: câu đối chứng, điều khoản chỉ có một phiên bản. `T2`: có hai phiên bản. `T3`: có nhiều phiên bản. `T6`: điều khoản đã bị bãi bỏ, câu hỏi không có đáp án.
- `F1`–`F7`: bảy giả thuyết kiểm định, khai trước trong `docs/decisions/0006-ke-hoach-phan-tich-bo-test-cuoi.md`.

## Lưu ý khi trích số

- Kết quả chính chạy trên `data/benchmark/final_test/`, chưa chấm lại trên bản 6.0.0.
- Accuracy của test 2 chỉ lấy ở `runs/earlier_tests/generation_test2/rescore/`.
- File kết quả (`.json`, `.jsonl`, log) ghi đường dẫn lúc chạy, tức là tên thư mục cũ. Bảng tên cũ ở cuối `runs/README.md`.

## Không có trong repo

Kho văn bản đầy đủ (khoảng 11 GB) không có ở đây, nên repo đủ để đọc và trích số, chưa đủ để chạy lại.
Kho rút gọn có ở [vilextime_uit](https://github.com/cmtri2005/vilextime_uit) (`corpus/`).
