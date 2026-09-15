# Nhật ký học tập

* **Ngày:** 15/09/2026
* **Chủ đề / Môn học:** Applied Bioinformatics — Buổi 1: DNA → RNA → Protein (Central Dogma)
* **Thời gian học:** 2 giờ

---

## 🎯 Mục tiêu buổi học
- [x] Hiểu cấu trúc DNA, RNA và dòng chảy thông tin di truyền (Central Dogma).
- [x] Nắm rõ cơ chế phiên mã (Transcription) và dịch mã (Translation).
- [x] Phân biệt rõ codon, anticodon, vai trò của ribosome và tRNA.
- [x] Hiểu bản chất tính thoái hóa của mã di truyền và lý do bài toán dịch ngược (Protein → DNA) bị mơ hồ.

---

## 🧬 Kiến thức cốt lõi

### 1. DNA (Bản thiết kế gốc)
* Cấu trúc chuỗi xoắn kép (double helix) gồm **2 mạch polynucleotide đối song song**, liên kết theo nguyên tắc bổ sung ($A-T$ với 2 liên kết hydro, $G-C$ với 3 liên kết hydro).
* Ở sinh vật nhân thực, DNA nằm cố định trong nhân tế bào nhằm bảo toàn thông tin di truyền.

### 2. Phiên mã (Transcription: DNA → mRNA)
* Enzyme **RNA polymerase** bám vào vùng điều hòa, mở xoắn DNA và dùng một mạch làm khuôn để tổng hợp phân tử **mRNA** mạch đơn theo nguyên tắc bổ sung ($A-U$, $T-A$, $G-C$, $C-G$).
* mRNA hoàn chỉnh rời nhân tế bào, đi ra tế bào chất để tham gia dịch mã.

### 3. Dịch mã (Translation: mRNA → Protein)
* **Ribosome ("Máy lắp ráp"):** Cấu tạo từ **rRNA + protein**, gồm 2 tiểu đơn vị (lớn và nhỏ). Ribosome trượt dọc theo phân tử mRNA theo chiều $5' \to 3'$.
* **Codon (Khung đọc bộ ba):** Phân tử mRNA là một **sợi đơn liên tục** (không bị cắt vụn). Ribosome đọc mRNA theo từng cụm 3 nucleotide liên tiếp gọi là **codon**.
* **tRNA ("Người vận chuyển"):** Mỗi phân tử tRNA chỉ liên kết đặc hiệu với **1 loại amino acid** ở đầu $3'$. Đầu đối diện mang bộ ba đối mã (**anticodon**) bắt cặp bổ sung và ngược chiều với codon trên mRNA.
* **Hình thành chuỗi polypeptide:** Khi anticodon khớp chính xác với codon, liên kết peptide được hình thành giữa các amino acid, chuỗi protein dài dần ra.

### 4. Quy tắc mã di truyền
* **Codon mở đầu:** `AUG` (mã hóa Methionine, đồng thời là tín hiệu khởi động dịch mã).
* **Codon kết thúc:** `UAA`, `UAG`, `UGA` (không mã hóa amino acid, là tín hiệu dừng dịch mã và giải phóng chuỗi protein).
* **Tính thoái hóa (Degeneracy):** Có 64 tổ hợp codon ($4^3$) nhưng chỉ mã hóa 20 loại amino acid $\to$ nhiều codon đồng nghĩa (synonymous codons) cùng mã hóa 1 amino acid.

---

## ⚠️ Khó khăn & Lỗi sai đã khắc phục

| Lỗi / Quan niệm ban đầu | Bản chất sinh học chính xác |
|---|---|
| *Nghĩ rằng mRNA sau khi bị cắt còn 3 nucleotide thì thành codon.* | **mRNA là mạch dài liên tục.** Codon là đơn vị khung đọc (reading frame) 3 nucleotide liền kề do ribosome quét qua. *(Cắt nếu có chỉ là cắt bỏ intron, nối exon - splicing).* |
| *Nghĩ rằng mRNA trượt trên ribosome và tạo ra "khuôn mới".* | **Ribosome trượt dọc theo mRNA.** Sản phẩm tạo ra là chuỗi polypeptide (protein), không phải khuôn. |
| *Dùng từ "2 vòng nu xoắn".* | Chuẩn xác là **2 mạch polynucleotide xoắn kép** (tránh nhầm với cấu trúc DNA dạng vòng / plasmid ở vi khuẩn). |

---

## 💡 Góc nhìn Tin sinh học (Bioinformatics Insight)

* **Vì sao DNA → Protein là xác định, còn Protein → DNA là mơ hồ?**
  * Chiều xuôi là ánh xạ **many-to-one**: mỗi codon chỉ quy định đúng 1 amino acid $\to$ biết trình tự mRNA/DNA thì suy ra chính xác 100% trình tự protein.
  * Chiều ngược là ánh xạ **one-to-many**: 1 amino acid có thể tương ứng với nhiều codon khác nhau (ví dụ: Leucine có 6 codon). Số lượng trình tự DNA khả dĩ tăng theo cấp số nhân ($c_1 \times c_2 \times \dots \times c_n$).
  * Do đó, công cụ *reverse translation* chỉ dự đoán trình tự khả dĩ nhất dựa vào **codon usage bias**, không thể khôi phục lại DNA gốc. Muốn biết chính xác phải giải trình tự trực tiếp (Sequencing).

---

## 📊 Đánh giá & Kế hoạch tiếp theo

* **Mức độ hiểu bài:** [ ] 1/5  [ ] 2/5  [ ] 3/5  [x] 4/5  [ ] 5/5
* **Nhiệm vụ cho buổi sau:** 
  * Exon, intron, promoter, UTR, splicing.
  * Xác suất, biến ngẫu nhiên, binomial, Poisson.
