# 🧬 Buổi Học: DNA → RNA → Protein — Nền Tảng Sinh Học Phân Tử cho Tin Sinh Học

> **Buổi 1 — Roadmap Applied Bioinformatics**
> Mục tiêu: Hiểu "dòng chảy thông tin di truyền" — nền tảng bắt buộc trước khi làm bất kỳ pipeline tin sinh học nào (sequencing, alignment, gene expression...).

---

## 📖 PHẦN 1 — LÝ THUYẾT NỀN TẢNG: DNA & RNA

### 1.1. DNA là gì?

Hãy tưởng tượng cơ thể bạn là một **nhà máy khổng lồ gồm hàng nghìn tỷ tế bào** (cell). Mỗi tế bào cần "bản thiết kế" để biết phải tạo ra chất gì. Bản thiết kế đó chính là **DNA** (Deoxyribonucleic Acid).

- DNA giống như **"cuốn sách công thức nấu ăn"** của tế bào, được cất trong **nhân tế bào** (nucleus), gần như không bao giờ mang ra ngoài.
- Về cấu trúc, DNA là một **chuỗi xoắn kép** (double helix) — như một cái thang dây bị xoắn lại.
- "Bậc thang" được tạo từ 4 loại **base nitơ** (nitrogenous base):

| Base | Tên đầy đủ | Bắt cặp với |
|---|---|---|
| A | Adenine | T (2 liên kết hydro) |
| T | Thymine | A (2 liên kết hydro) |
| G | Guanine | C (3 liên kết hydro) |
| C | Cytosine | G (3 liên kết hydro) |

Nguyên tắc **bổ sung** (complementary base pairing): A luôn bắt cặp T, G luôn bắt cặp C. Đây là quy tắc nền tảng cho mọi công cụ tin sinh học sau này (alignment, PCR, sequencing...).

> 💡 **Gene là gì?** Một đoạn DNA mang thông tin để tạo ra **một protein cụ thể** gọi là **gene**. Toàn bộ DNA trong 1 tế bào gọi là **genome** (bộ gen).

### 1.2. Từ DNA đến RNA — Phiên mã (Transcription)

DNA quý đến mức **không rời khỏi nhân tế bào**. Vậy thông tin được "mang" ra tế bào chất (cytoplasm) bằng cách nào?

→ Tế bào tạo một **bản sao tạm thời**, gọi là **RNA** (Ribonucleic Acid), cụ thể là **mRNA** (messenger RNA).

🍳 *Ví dụ:* Bạn không mang cả cuốn sách công thức gốc vào bếp — bạn **photocopy 1 trang** cần dùng rồi mang bản photo vào bếp. Bản gốc vẫn nằm yên trong tủ (nhân tế bào).

**RNA khác DNA ở 3 điểm chính:**

| Đặc điểm | DNA | RNA |
|---|---|---|
| Cấu trúc | Mạch đôi (double-stranded) | Mạch đơn (single-stranded) |
| Đường (sugar) | Deoxyribose | Ribose |
| Base thứ 4 | Thymine (T) | Uracil (U) thay T |
| Vị trí | Trong nhân tế bào | Di chuyển ra tế bào chất |

**Quá trình Phiên mã (Transcription):**
1. Enzyme **RNA polymerase** bám vào DNA tại vị trí gene cần đọc.
2. DNA mở xoắn cục bộ, lộ ra 1 mạch làm khuôn (template strand).
3. RNA polymerase đọc mạch khuôn, tổng hợp mRNA bổ sung (A-U, T-A, G-C, C-G).
4. mRNA hoàn chỉnh rời nhân tế bào, đi ra tế bào chất.

> 📌 *Ghi chú nâng cao:* Ở sinh vật nhân thực (eukaryote), mRNA sơ khai còn qua bước **cắt intron/nối exon** (splicing) trước khi trưởng thành — chi tiết sẽ học ở buổi RNA-seq.

---

## ☕ NGHỈ GIẢI LAO — QUIZ NHANH (5 câu)
*(Đáp án ở cuối tài liệu — mục "✅ Đáp án Quiz 1")*

**Câu 1.** DNA có cấu trúc dạng gì?
A. Chuỗi đơn thẳng  B. Chuỗi xoắn kép  C. Hình cầu  D. Chuỗi xoắn ba

**Câu 2.** Trong DNA, Adenine (A) bắt cặp với base nào?
A. Guanine (G)  B. Cytosine (C)  C. Thymine (T)  D. Uracil (U)

**Câu 3.** Đặc điểm nào KHÔNG đúng khi mô tả RNA so với DNA?
A. RNA là mạch đơn  B. RNA dùng đường ribose  C. RNA có base Thymine  D. RNA có base Uracil

**Câu 4.** Enzyme nào chịu trách nhiệm tổng hợp mRNA từ DNA?
A. DNA polymerase  B. RNA polymerase  C. Ribosome  D. tRNA synthetase

**Câu 5.** mRNA có vai trò gì?
A. Lưu trữ thông tin di truyền vĩnh viễn  B. Mang bản sao thông tin từ nhân ra tế bào chất  C. Tạo năng lượng cho tế bào  D. Vận chuyển amino acid

---

## 📖 PHẦN 2 — LÝ THUYẾT TIẾP: CODON & RIBOSOME

### 2.1. Codon là gì?

mRNA đã ra tới "nhà bếp" (tế bào chất), nhưng nó chỉ là chuỗi 4 chữ cái (A,U,G,C). Làm sao chuỗi này biến thành **protein** — xây từ 20 loại **amino acid**?

→ mRNA được đọc theo từng **bộ ba** liên tiếp, gọi là **codon**.

**Tại sao là bộ 3, không phải bộ 1 hay bộ 2?**
Bài toán tổ hợp đơn giản: 4 loại base, cần mã hóa ít nhất 20 amino acid.
- 1 base = 1 mã → 4¹ = 4 tổ hợp → quá ít.
- 2 base = 1 mã → 4² = 16 tổ hợp → vẫn chưa đủ 20.
- 3 base = 1 mã → 4³ = **64 tổ hợp** → dư dùng cho 20 amino acid.

→ Tự nhiên "chọn" bộ 3 (triplet code) vì đó là số nhỏ nhất đủ mã hóa toàn bộ 20 amino acid.

**Bảng mã di truyền (Genetic code)** quy định mỗi codon ứng với amino acid nào:
- **AUG** là **codon mở đầu** (start codon) — mã hóa Methionine (Met) *(ở vi khuẩn là formyl-methionine, fMet)*, đồng thời đánh dấu vị trí bắt đầu dịch mã.
- Có **3 codon kết thúc** (stop codon): **UAA, UAG, UGA** — không mã hóa amino acid nào, mà ra hiệu "dừng lại".
- 64 codon nhưng chỉ 20 amino acid → nhiều codon khác nhau cùng mã hóa **1** amino acid — gọi là **tính thoái hóa của mã di truyền** (degeneracy). Sẽ quay lại điểm này ở câu hỏi cuối buổi.

| Amino acid | Số codon mã hóa | Ví dụ codon |
|---|---|---|
| Methionine (Met) | 1 | AUG |
| Tryptophan (Trp) | 1 | UGG |
| Leucine (Leu) | 6 | UUA, UUG, CUU, CUC, CUA, CUG |
| Serine (Ser) | 6 | UCU, UCC, UCA, UCG, AGU, AGC |
| Arginine (Arg) | 6 | CGU, CGC, CGA, CGG, AGA, AGG |

### 2.2. Ribosome & Dịch mã (Translation)

**Ribosome** là "nhà máy lắp ráp protein" của tế bào — đọc mRNA và ghép amino acid thành chuỗi protein.

- Ribosome gồm **rRNA** (ribosomal RNA) + protein, cấu tạo từ **2 tiểu đơn vị** (subunit) lớn + nhỏ, chỉ ghép lại khi bắt đầu dịch mã *(vi khuẩn: 30S+50S = 70S; sinh vật nhân thực: 40S+60S = 80S — "S" là đơn vị tốc độ lắng Svedberg)*.
- **tRNA** (transfer RNA) là "người giao hàng": mỗi tRNA mang đúng 1 loại amino acid, có đoạn 3-base gọi là **anticodon**, khớp bổ sung với codon trên mRNA (như chìa khóa khớp ổ khóa).

**3 giai đoạn của Dịch mã (Translation):**

| Giai đoạn | Điều gì xảy ra |
|---|---|
| **1. Khởi đầu (Initiation)** | Tiểu đơn vị nhỏ bám vào mRNA, tìm codon AUG; tRNA mang Methionine gắn vào; tiểu đơn vị lớn ghép vào hoàn thiện ribosome. |
| **2. Kéo dài (Elongation)** | Ribosome trượt dọc mRNA từng codon. Mỗi lần, 1 tRNA mang đúng amino acid đi vào, hình thành **liên kết peptide** (peptide bond) nối amino acid mới vào chuỗi. |
| **3. Kết thúc (Termination)** | Gặp stop codon (UAA/UAG/UGA) — không tRNA nào khớp → chuỗi polypeptide (protein) được giải phóng, ribosome tách khỏi mRNA. |

🏭 *Ví dụ:* Ribosome như một **dây chuyền lắp ráp** — mRNA là bản vẽ kỹ thuật chạy qua máy đọc mã vạch (từng codon), tRNA là các "công nhân giao đúng linh kiện" (amino acid) theo đúng thứ tự bản vẽ.

---

## 🔄 TỔNG HỢP DÒNG CHẢY — HỌC THUYẾT TRUNG TÂM (CENTRAL DOGMA)

```mermaid
flowchart LR
    A["DNA<br>(Nhân tế bào)"] -->|"Phiên mã<br>Transcription"| B["mRNA<br>(Tế bào chất)"]
    B -->|"Dịch mã<br>Translation"| C["Protein<br>(Chuỗi amino acid)"]


Dòng chảy **DNA → RNA → Protein** được Francis Crick đặt tên là **Học thuyết Trung tâm của Sinh học phân tử** (Central Dogma of Molecular Biology, 1958).

> 📌 *Ghi chú:* Đây là chiều phổ biến nhất nhưng không tuyệt đối — virus retrovirus (như HIV) dùng enzyme **reverse transcriptase** đi ngược RNA → DNA. Đây là ngoại lệ được công nhận, không phải sai lệch lý thuyết gốc.

**Vì sao tin sinh học cần hiểu dòng chảy này?** Vì hầu hết dữ liệu bạn sẽ xử lý (DNA sequencing, RNA-seq, protein structure...) là "ảnh chụp" một điểm trên dòng chảy này. Hiểu rõ bước nào tạo ra bước nào giúp chọn đúng công cụ và diễn giải đúng kết quả.

---

## 📝 QUIZ TỔNG HỢP (10 câu)
*(Đáp án ở cuối tài liệu — mục "✅ Đáp án Quiz 2")*

**Câu 1.** Một codon gồm bao nhiêu nucleotide?

**Câu 2.** Tổng số codon có thể có là bao nhiêu? Vì sao?

**Câu 3.** Codon nào là codon mở đầu, và nó mã hóa cho amino acid nào?

**Câu 4.** Kể tên 3 codon kết thúc (stop codon).

**Câu 5.** tRNA có vai trò gì trong quá trình dịch mã?

**Câu 6.** Ribosome được cấu tạo từ những thành phần nào?

**Câu 7.** Sắp xếp đúng thứ tự 3 giai đoạn của dịch mã: Kéo dài / Kết thúc / Khởi đầu.

**Câu 8.** Vì sao mã di truyền được gọi là "thoái hóa" (degenerate)?

**Câu 9.** Học thuyết Trung tâm (Central Dogma) mô tả dòng chảy thông tin nào?

**Câu 10.** Nếu biết trình tự DNA của 1 gene, ta có thể suy ra chính xác trình tự protein không? Ngược lại, biết trình tự protein có suy ngược chính xác trình tự DNA gốc được không?

---

## ✅ ĐÁP ÁN

### Đáp án Quiz 1 (5 câu)
1. **B** — Chuỗi xoắn kép (double helix).
2. **C** — Thymine (T); A-T bắt cặp bằng 2 liên kết hydro.
3. **C** — RNA **không** có Thymine, mà thay bằng Uracil (U).
4. **B** — RNA polymerase.
5. **B** — mRNA mang bản sao thông tin di truyền từ nhân ra tế bào chất để tổng hợp protein.

### Đáp án Quiz 2 (10 câu)
1. **3 nucleotide** (bộ ba — triplet).
2. **64 codon** (4³ = 64) — 4 loại base, mỗi codon gồm 3 vị trí, mỗi vị trí có 4 lựa chọn độc lập.
3. **AUG** — mã hóa **Methionine (Met)**, đồng thời là tín hiệu khởi đầu dịch mã.
4. **UAA, UAG, UGA**.
5. tRNA mang amino acid tương ứng đến ribosome; anticodon của nó khớp bổ sung với codon trên mRNA, đảm bảo amino acid gắn đúng vị trí, đúng thứ tự.
6. Ribosome gồm **rRNA (ribosomal RNA)** kết hợp **protein**, tạo thành 2 tiểu đơn vị lớn và nhỏ.
7. Đúng thứ tự: **Khởi đầu (Initiation) → Kéo dài (Elongation) → Kết thúc (Termination)**.
8. Vì bộ mã di truyền có **64 codon** nhưng chỉ cần mã hóa **20 amino acid** + tín hiệu dừng → nhiều codon đồng nghĩa (synonymous codons) cùng mã hóa **1** amino acid (ví dụ Leucine có 6 codon khác nhau).
9. Dòng chảy: **DNA → RNA → Protein** (phiên mã rồi dịch mã).
10. **DNA → Protein: SUY RA ĐƯỢC** (xác định, một chiều). **Protein → DNA: MƠ HỒ**, không suy ngược chính xác được. → Xem giải thích chi tiết bên dưới.

---

## 🔑 CÂU HỎI MỞ: Vì Sao Suy Protein Từ DNA Thì Được, Mà Ngược Lại Thì Mơ Hồ?

Đây là khái niệm nền tảng và hay bị hiểu lầm nhất khi mới học tin sinh học. Bản chất nằm ở **tính thoái hóa của mã di truyền** (degeneracy of the genetic code).

**1. Chiều xuôi (DNA → RNA → Protein) là một "hàm số" xác định (many-to-one, nhưng single-valued theo chiều thuận):**
- Mỗi codon **chỉ** mã hóa cho **đúng 1** amino acid (hoặc tín hiệu dừng) — không có codon nào mã hóa 2 amino acid khác nhau.
- Vì vậy, biết chính xác trình tự DNA/mRNA → tra bảng mã di truyền → suy ra **chính xác 100%** trình tự protein. Không mơ hồ ở chiều này.

**2. Chiều ngược (Protein → RNA → DNA) là bài toán ánh xạ ngược không xác định (one-to-many):**
- Vì 64 codon chỉ mã hóa cho 20 amino acid, **nhiều codon khác nhau cùng cho ra 1 amino acid** (codon đồng nghĩa — synonymous codons). Ví dụ: thấy Leucine trong protein, ta **không biết chắc** codon gốc là UUA, UUG, CUU, CUC, CUA hay CUG — cả 6 khả năng đều hợp lệ.
- Với một chuỗi protein dài gồm nhiều amino acid, **số trình tự DNA khả dĩ tăng theo cấp số nhân**. Gọi $c_i$ là số codon đồng nghĩa của amino acid thứ $i$ trong protein dài $n$ amino acid:

  Số trình tự DNA khả dĩ = $c_1 \times c_2 \times c_3 \times \ldots \times c_n$

  → Chỉ với một đoạn peptide dài 10 amino acid toàn Leucine (6 codon/amino acid), đã có tới $6^{10}$ ≈ **60 triệu** trình tự DNA khả dĩ khác nhau — không thể xác định đâu là bản gốc thật nếu chỉ nhìn vào protein.

**3. Ý nghĩa trong thực hành tin sinh học:**
- Đây là lý do các công cụ *reverse translation* (dịch ngược protein → DNA, dùng khi thiết kế gene tổng hợp — gene synthesis) chỉ đưa ra **một phỏng đoán khả dĩ nhất**, thường dựa trên **codon usage bias** (tần suất dùng codon phổ biến của loài đích), chứ không khôi phục chính xác trình tự gốc.
- Ở sinh vật nhân thực, DNA còn chứa **intron** (bị cắt khi tạo mRNA trưởng thành) và vùng không mã hóa — thông tin này **biến mất hoàn toàn** khỏi trình tự protein cuối, càng khiến việc suy ngược bất khả thi.
- → Đây cũng là lý do khi cần biết chính xác trình tự di truyền, người ta luôn **giải trình tự trực tiếp DNA/RNA** (sequencing) thay vì cố suy ngược từ protein.

> 🎯 **Ghi nhớ cốt lõi:** Mã di truyền là hàm **many-to-one** theo chiều dịch mã, nên **không có hàm ngược duy nhất** (not invertible). Đây là một quy luật toán học đơn giản (hàm không song ánh — not bijective), không phải "bí ẩn sinh học".

---

## 📌 Tóm tắt buổi học

- **DNA** lưu trữ thông tin di truyền dưới dạng chuỗi 4 base (A,T,G,C), nằm trong nhân tế bào.
- **Phiên mã (Transcription):** DNA → mRNA — tạo bản sao để mang ra tế bào chất.
- **Codon:** bộ 3 nucleotide trên mRNA, mã hóa 1 amino acid (64 codon cho 20 amino acid + tín hiệu dừng).
- **Dịch mã (Translation):** Ribosome + tRNA đọc mRNA theo codon, lắp ráp thành chuỗi protein.
- **Central Dogma:** DNA → RNA → Protein — dòng chảy thông tin di truyền cơ bản của sự sống.
- **Mã di truyền thoái hóa** khiến suy protein từ DNA là xác định, nhưng suy ngược DNA từ protein là mơ hồ.

---

*Tài liệu này là buổi học nền tảng số 1 trong roadmap Applied Bioinformatics. Buổi tiếp theo: các định dạng dữ liệu sinh học (FASTA, FASTQ, GFF...) và cách các cơ sở dữ liệu công khai (NCBI, Ensembl, UniProt) lưu trữ thông tin này.*
