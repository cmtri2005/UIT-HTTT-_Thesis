# Temporal-Aware RAG cho pháp luật Việt Nam — dữ liệu, code và kết quả

Repo gom những gì cần để viết paper và khóa luận: bộ ViLexTime, nhãn, kết quả thực nghiệm, code chạy và chấm.

```
src/vilexgraph/        hệ hỏi đáp: kho phiên bản, truy xuất, sinh
scripts/               eval/ chạy và chấm · paper/ dựng số liệu · check/ kiểm dữ liệu · pipeline/ dựng bộ · review/ gán nhãn
prompts/  tests/  notebooks/

data/
  benchmark/
    test_final/        bộ test cuối, 722 dòng — kết quả chính chạy trên bản này
    release_v6/        bản phát hành 6.0.0 (data + annotation), đã sửa lỗi kho
    test_v1/           bộ test đầu (thực nghiệm sớm)
    test2/             bộ test 2
  labels/
    blind_gold/        gold đọc mù A/B và đồng thuận
    annotation_rounds/ phiếu gán theo lô, hướng dẫn 4.0
    human_scoring/     chấm tay câu trả lời
    test2_llm_labels/  nhãn LLM của test 2
    test2_llm_calibration/  hiệu chuẩn lời dặn LLM
    error_analysis/    phân rã lỗi
    missed_amendment_audit/ rà sót sửa đổi
    kiem_A.tsv, kiem_B.tsv  kiểm chéo hai người gán
  corpus/              manifest kho văn bản, kiểm bản hợp nhất
  vocab/               bảng mã trạng thái, loại quan hệ, lĩnh vực

runs/
  final/               bộ test cuối
    main/              kết quả chính (r1, r2: hai lần gọi API)
    posthoc/           quy kết, chấm tay, sửa đáp án ngắn
    lightrag_baseline/ open_model_qwen/
    blind_gold_subset/ gold_fix/ version_recheck/ retrieval_funnel/
  early/               test v1 và test 2: retrieval_*, generation_*, domain_breakdown
  dev/                 chọn cấu hình trên dev: pilot_generation, hybrid_retrieval,
                       rebuild_rerun, repeal_fix, reranker_pool, verifier
  README.md            sổ chạy: mỗi lần chạy, mục đích, kết quả

docs/
  paper/               ieee/, acm/ (main.tex, numbers.md), abstract_vi/, figures/
  thesis/              chương 1–7
  decisions/           ADR (0005, 0006: kế hoạch phân tích)
  reports/             audit, đồng thuận, chẩn đoán
  vilextime-*.md       định nghĩa, hướng dẫn gán, datasheet, schema
  de_cuong_kltn.docx   đề cương
```

## Lưu ý khi trích số

- Kết quả trong `runs/final/main/` chạy trên `data/benchmark/test_final/`, chưa chấm lại trên 6.0.0.
- Accuracy của test 2 chỉ lấy ở `runs/early/generation_test2/rescore/`.
- Kiểm hash: `cd data/benchmark/test_final && sha256sum -c SHA256SUMS`.

## Tên cũ

File bằng chứng (`*.json`, `*.jsonl`, log, `SHA256SUMS`, `data/benchmark/test_final/README.md`) giữ nguyên đường dẫn cũ lúc chạy.
Đối chiếu:

| Cũ | Mới |
|---|---|
| `data/vilextime_final` | `data/benchmark/test_final` |
| `data/vilextime_dataset` | `data/benchmark/test_v1` |
| `data/vilextime` | `data/benchmark/test2` |
| `data/review/*` | `data/labels/*` (`gold_final` → `blind_gold`, `the4` → `annotation_rounds`, `human_score_g4` → `human_scoring`, `e7` → `error_analysis`) |
| `runs/final_g4_20261112` | `runs/final/main` |
| `runs/posthoc_g4_20261113` | `runs/final/posthoc` |
| `runs/lightrag_20261113`, `runs/qwen_chutes_20261116` | `runs/final/lightrag_baseline`, `runs/final/open_model_qwen` |
| `runs/E1`, `runs/E1_test2` | `runs/early/retrieval_test_v1`, `runs/early/retrieval_test2` |
| `runs/E3_v1`, `runs/E3_test2` | `runs/early/generation_test_v1`, `runs/early/generation_test2` |
| `runs/B2_dev` | `runs/dev/pilot_generation` |
| `runs/*_dev*`, `runs/hybrid_20261105` | `runs/dev/*` |

## Không có trong repo

Kho văn bản dẫn xuất (`data/derived/`, khoảng 11 GB) và dữ liệu cào gốc không có ở đây.
Muốn chấm lại hoặc chạy lại thì cần kho đó (`scripts/pull_snapshot.sh`).
Kho phiên bản rút gọn có ở repo phát hành [vilextime_uit](https://github.com/cmtri2005/vilextime_uit) (`corpus/`).
