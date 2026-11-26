# Temporal-Aware RAG cho pháp luật Việt Nam — dữ liệu, code và kết quả

Repo gom những gì cần để viết paper và khóa luận: bộ ViLexTime, nhãn, kết quả thực nghiệm, code chạy và chấm.
Đường dẫn giữ nguyên như repo gốc, nên mọi tham chiếu trong `docs/paper/*/numbers.md` và `docs/thesis/` vẫn đúng.

```
.
├── src/vilexgraph/          hệ hỏi đáp: kho phiên bản, truy xuất, sinh
├── scripts/                 eval/ (chạy, chấm), paper/ (dựng số liệu), check/ (kiểm dữ liệu), pipeline/
├── prompts/                 lời dặn LLM
├── tests/
├── data/
│   ├── vilextime_final/     bộ test cuối, 722 dòng — kết quả chính chạy trên bản này (SHA256SUMS)
│   ├── vilextime_v6/        bản phát hành 6.0.0 (data + annotation), đã sửa lỗi kho
│   ├── vilextime_dataset/   test v1 (E1, E3)
│   ├── vilextime/           test 2 (test2_release_v2.jsonl)
│   ├── review/              nhãn người, gold A/B, đồng thuận, phiếu chấm tay
│   └── derived/corpus/      manifest của kho văn bản
├── runs/                    kết quả; sổ chạy ở runs/README.md
│   ├── final_g4_20261112/   kết quả chính (r1, r2)
│   ├── posthoc_g4_20261113/ quy kết, chấm tay, sửa T1
│   ├── lightrag_20261113/   baseline LightRAG
│   ├── qwen_chutes_20261116/ model mở
│   └── E1*, E3*, B2_dev, …  thực nghiệm cũ và ablation trên dev
└── docs/
    ├── paper/               ieee/, acm/ (main.tex, numbers.md), figures/
    ├── thesis/              chương 1–7
    ├── decisions/           ADR (0005, 0006: kế hoạch phân tích)
    ├── reports/             audit, đồng thuận, phân rã lỗi
    └── vilextime-*.md       định nghĩa, hướng dẫn gán, datasheet, schema
```

## Lưu ý khi trích số

- Kết quả trong `runs/final_g4_20261112/` chạy trên `data/vilextime_final/`, chưa chấm lại trên 6.0.0.
- Accuracy của test 2 chỉ lấy ở `runs/E3_test2/rescore/`.
- Kiểm hash: `cd data/vilextime_final && sha256sum -c SHA256SUMS`.

## Không có trong repo

Kho văn bản dẫn xuất (`data/derived/`, khoảng 11 GB) và dữ liệu cào gốc không có ở đây.
Muốn chấm lại hoặc chạy lại thì cần kho đó (`scripts/pull_snapshot.sh`).
Kho phiên bản rút gọn có ở repo phát hành [vilextime_uit](https://github.com/cmtri2005/vilextime_uit) (`corpus/`).
