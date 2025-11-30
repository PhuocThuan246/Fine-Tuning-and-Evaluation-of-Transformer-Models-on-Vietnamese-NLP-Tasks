# Fine-Tuning-and-Evaluation-of-Transformer-Models-on-Vietnamese-NLP-Tasks
# 🇻🇳 Fine-tune & Đánh giá mô hình Transformer trên dữ liệu tiếng Việt  
# Giới thiệu dự án  
Repository này chứa mã nguồn, báo cáo và tài liệu minh họa cho **ba mô hình Transformer** được fine-tune trên **dữ liệu tiếng Việt**, gồm:

1. **PhoBERT** – Phân loại cảm xúc văn bản  
2. **ViT5** – Dịch máy Anh → Việt  
3. **Qwen3-0.6B** – Hỏi đáp tiếng Việt  

Dự án được xây dựng theo hướng **so sánh mô hình gốc vs. mô hình mở rộng bằng Synthetic Data**, nhằm đánh giá tác động của dữ liệu tổng hợp đối với hiệu quả mô hình.

---

# 1. PhoBERT – Phân loại cảm xúc
### Giới thiệu mô hình  
PhoBERT là mô hình Transformer encoder-only dành cho tiếng Việt, tương tự RoBERTa, được pre-train trên >20GB dữ liệu tiếng Việt.

### Dữ liệu  
- Bộ dữ liệu: **VLSP2016**  
- Train: 4590 mẫu  
- Validation: 510 mẫu  
- Test: 1050 mẫu  
- Ba lớp cân bằng: Positive – Neutral – Negative  

### Synthetic Data  
Sinh bởi **Qwen2-7B-Instruct** với ba kỹ thuật:
- Paraphrase Sentiment  
- Similar Sentiment Generation  
- Dataset Expansion  

### Quy trình fine-tune  
- Learning rate: 2e-5  
- Batch size: 16  
- Epochs: 4  
- Weight decay: 0.01  
- Trainer: HuggingFace Trainer  

### Kết quả  
Đánh giá theo: Accuracy, F1-score  
Kèm đánh giá bởi LLM để kiểm chứng chất lượng ngữ nghĩa.

---

# 2. ViT5 – Dịch máy Anh → Việt  
### Giới thiệu mô hình  
ViT5 (VietAI/vit5-base) là mô hình seq2seq thuần Việt (Encoder–Decoder), dựa trên Google T5.

### Dữ liệu  
- harouzie/vi_en-translation  
- 5000 cặp song ngữ EN–VI  
- Tách tập: Train/Valid/Test  

### Synthetic Data  
Sinh bởi Qwen thông qua:
- Paraphrase + Translate  
- Similar sentence generation  
- Dataset expansion  

### Quy trình fine-tune  
- LR: 2e-4  
- Epochs: 3  
- Gradient accumulation: 4  
- Warmup ratio: 0.03  
- Trainer: Seq2SeqTrainer  
- Prefix input: `translate English to Vietnamese:`

### Kết quả  
Đánh giá theo BLEU + đánh giá LLM: meaning + fluency.

---

# 3. Qwen3-0.6B – Hỏi đáp tiếng Việt  
### Giới thiệu mô hình  
Qwen3-0.6B là mô hình decoder-only (giống GPT) tối ưu cho fine-tune trên GPU tầm trung.

### Dữ liệu  
- UIT-ViQuAD 2.0  
- 6000+ mẫu QA thuộc 31 chủ đề  
- Train: 4500  
- Validation: 500  
- Test: 1000  

### Synthetic Data  
Sinh bằng Qwen2-7B với:
- Sinh đoạn văn mới + câu hỏi mới
- Loại trùng, kiểm tra thủ công  
- Tổng train tăng lên ~6500 mẫu  

### Quy trình fine-tune  
- Epochs: 1  
- LR: 2e-4  
- Max length: 1024  
- Trainer: **SFTTrainer (HuggingFace TRL)**  
- Format dữ liệu: Chat format (user → assistant)

### Kết quả  
Đánh giá:
- Exact Match  
- F1-score  
- Đánh giá bởi LLM (tính mạch lạc, tính hợp lý)

---

# Công nghệ sử dụng  
- Python 3.10  
- PyTorch  
- HuggingFace Transformers  
- HuggingFace Datasets  
- TRL (Transformers Reinforcement Learning)  
- Qwen LLM  
- Matplotlib / Seaborn  

