# data/

## benchmark/ — bộ dữ liệu ViLexTime

| Thư mục | Là gì | Dùng ở đâu |
|---|---|---|
| `final_test/` | Bộ test cuối, 722 dòng. Đã khóa hash (`SHA256SUMS`) | Kết quả chính của paper |
| `public_release_v6/` | Bản công bố 6.0.0: dữ liệu, hướng dẫn gán, nhãn gốc. Đã sửa một số lỗi kho so với `final_test/` | Bài dữ liệu, trích dẫn bộ dữ liệu |
| `earlier_test_v1/` | Bộ test đầu tiên (v1.0) | Thực nghiệm sớm, chương 4 và 6 |
| `earlier_test2/` | Bộ test thứ hai. File chính: `test2_release_v2.jsonl` | Thực nghiệm sớm, chương 6 |

## labels/ — nhãn và kiểm tra của người

| Thư mục | Là gì |
|---|---|
| `blind_gold/` | Hai người đọc mù, tự tìm phiên bản đúng (gold A, gold B) và độ đồng thuận |
| `annotation_rounds/` | Phiếu gán nhãn theo từng lô, hướng dẫn 4.0 gửi luật sư |
| `kiem_A.tsv`, `kiem_B.tsv` | Hai người gán kiểm chéo cùng một mẫu |
| `human_scoring/` | Người chấm tay 100 câu trả lời để kiểm tra bộ chấm tự động |
| `error_analysis/` | Phân loại lỗi của câu trả lời sai |
| `missed_amendment_audit/` | Rà các điều khoản kho bỏ sót lần sửa đổi |
| `test2_llm_labels/` | Nhãn do LLM gán cho test 2 |
| `test2_llm_calibration/` | Đo lời dặn LLM so với nhãn người trước khi dùng |

## Khác

- `corpus/`: thống kê kho văn bản (`manifest.json`), kết quả đối chiếu với văn bản hợp nhất.
- `vocab/`: bảng mã của nguồn vbpl (trạng thái hiệu lực, loại quan hệ, lĩnh vực). Code cần để chạy.
