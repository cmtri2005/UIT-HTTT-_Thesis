# runs/

Mỗi thư mục là một lần thực nghiệm. Trong đó, mỗi hệ có ba file:
`<mã hệ>.jsonl` (câu trả lời), `<mã hệ>.run.json` (cấu hình, commit, hash) và `*.score.json` (điểm).
Bảng mã hệ ở `README.md` gốc. Nhật ký chi tiết từng lần chạy ở `LOG.md`.

## final_test/ — trên bộ test cuối (kết quả chính)

| Thư mục | Là gì |
|---|---|
| `main/` | Kết quả chính. `r1/`, `r2/` là hai lần gọi API. Đọc `r1/results.md` trước |
| `extra_analysis/` | Phân tích thêm sau khi chạy: quy kết lợi ích, chấm tay, sửa đáp án ngắn của T1 |
| `lightrag_baseline/` | So với LightRAG (thăm dò) |
| `open_model_qwen/` | Chạy lại bằng model mở Qwen (thăm dò) |
| `blind_gold_subset/` | Chấm lại chỉ trên các dòng có gold đọc mù |
| `gold_fix/` | Chấm lại sau khi sửa gold |
| `version_recheck/` | Đồng thuận khi kiểm lại phiên bản |
| `retrieval_funnel/` | Câu trả lời mất ở bước nào: truy xuất, lọc hay sinh |

## earlier_tests/ — trên hai bộ test cũ

| Thư mục | Là gì |
|---|---|
| `retrieval_test_v1/`, `retrieval_test2/` | Chỉ đo truy xuất |
| `generation_test_v1/`, `generation_test2/` | Đo cả câu trả lời. Accuracy test 2 chỉ lấy ở `generation_test2/rescore/` |
| `domain_breakdown/` | Kết quả theo lĩnh vực |

## dev_tuning/ — chọn cấu hình trên tập dev (không chạm test)

| Thư mục | Là gì |
|---|---|
| `pilot_generation/` | Chạy thử sinh câu trả lời bằng Qwen3-4B |
| `hybrid_retrieval/` | Thử kết hợp từ khóa và nghĩa, mở rộng lên điều khoản cha |
| `rebuild_rerun/` | Chạy lại mọi hệ sau khi dựng lại đồ thị; chọn cấu hình đề xuất |
| `repeal_fix/` | Kiểm tra sau khi sửa cách xử lý bãi bỏ |
| `reranker_pool/` | Thử reranker (cuối cùng không dùng) |
| `verifier/` | Đo bộ kiểm trích dẫn |

## Tên cũ

Các file kết quả ghi đường dẫn lúc chạy, tức là tên cũ:

| Cũ | Mới |
|---|---|
| `runs/final_g4_20261112` | `final_test/main` |
| `runs/posthoc_g4_20261113` | `final_test/extra_analysis` |
| `runs/lightrag_20261113` | `final_test/lightrag_baseline` |
| `runs/qwen_chutes_20261116` | `final_test/open_model_qwen` |
| `runs/gold_final_subset_20261115`, `gold_fix_20261116`, `version_recheck_20261117`, `gap_analysis_20261117` | `final_test/blind_gold_subset`, `gold_fix`, `version_recheck`, `retrieval_funnel` |
| `runs/E1`, `runs/E1_test2` | `earlier_tests/retrieval_test_v1`, `retrieval_test2` |
| `runs/E3_v1`, `runs/E3_test2` | `earlier_tests/generation_test_v1`, `generation_test2` |
| `runs/domain_e3` | `earlier_tests/domain_breakdown` |
| `runs/B2_dev`, `hybrid_20261105`, `rebuild_20261110_dev`, `repeal_persist_dev_20261112`, `rerank_pool_dev_20261115`, `x4_verifier_dev` | `dev_tuning/pilot_generation`, `hybrid_retrieval`, `rebuild_rerun`, `repeal_fix`, `reranker_pool`, `verifier` |
| `data/vilextime_final` | `data/benchmark/final_test` |
| `data/vilextime_dataset`, `data/vilextime` | `data/benchmark/earlier_test_v1`, `earlier_test2` |
| `data/review/…` | `data/labels/…` (`gold_final` → `blind_gold`, `the4` → `annotation_rounds`, `human_score_g4` → `human_scoring`, `e7` → `error_analysis`) |
