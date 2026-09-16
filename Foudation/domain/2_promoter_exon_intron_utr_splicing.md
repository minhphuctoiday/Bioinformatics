# 🟢 CHỦ ĐỀ 2 — CẤU TRÚC THẬT SỰ CỦA MỘT GENE: PROMOTER, EXON, INTRON, UTR, SPLICING
 
**⏱ 2 giờ**
 
---
 
## 2.0. Chuyển tiếp: Ba lỗ hổng trong mô hình buổi 1 (0:00 – 0:10)
 
Buổi 1, để dễ hiểu, mình đã nói: *"Gene là một đoạn DNA mang thông tin tạo ra một protein, được phiên mã thành mRNA rồi dịch mã thành protein."*
 
Câu đó **không sai**, nhưng nó **thiếu ba thứ quan trọng**. Hãy tự đặt câu hỏi:
 
**❓ Lỗ hổng 1 — AI ra lệnh bắt đầu?**
Buổi 1 ta nói enzyme RNA polymerase "bám vào DNA tại vị trí gene cần đọc". Nhưng bộ gen người dài khoảng 3 tỉ chữ cái. Làm sao RNA polymerase biết **bám vào đâu**? Và làm sao tế bào da biết chỉ bật gene của da, không bật gene của gan, dù cả hai tế bào chứa **y hệt** bộ DNA?
→ Câu trả lời: **PROMOTER**.
 
**❓ Lỗ hổng 2 — Phần "thừa" bị cắt đi là gì?**
Cuối buổi 1 mình có nhắc thoáng qua trong Ghi chú nâng cao: mRNA vừa tạo ra phải cắt bỏ đoạn thừa (**intron**) và nối các đoạn hữu ích (**exon**). Nhưng tại sao lại tồn tại đoạn thừa? Cắt nối kiểu gì? Máy móc nào làm việc đó?
→ Câu trả lời: **EXON, INTRON, SPLICING**.
 
**❓ Lỗ hổng 3 — mRNA có phải toàn bộ đều mã hóa protein không?**
Buổi 1 ta nói ribosome đọc mRNA từ codon AUG đến codon STOP. Vậy phần mRNA **trước** AUG và **sau** STOP — có tồn tại không? Nếu có thì để làm gì?
→ Câu trả lời: **UTR**.
 
> 🗺️ **Bản đồ tư duy cho 2 giờ tới:** Ta sẽ đi theo đúng trình tự mà tế bào thực hiện — từ **tín hiệu khởi động** (promoter) → **bản nháp** (pre-mRNA với exon/intron) → **quá trình biên tập** (splicing) → **bản hoàn chỉnh** (mRNA trưởng thành với UTR) → **ứng dụng tin sinh học**.
 
---
 
## 2.1. Bản đồ tổng quan cấu trúc gene (0:10 – 0:15)
 
Trước khi đi vào từng phần, hãy nhìn toàn cảnh một lần. Đừng lo nếu chưa hiểu hết — ta sẽ bóc từng lớp.
 
```
════════ TRÊN DNA (trong nhân tế bào) ════════
 
  ──[PROMOTER]──▶[Exon1]──(Intron1)──[Exon2]──(Intron2)──[Exon3]──
       ▲          ↑                                            ↑
   Công tắc      TSS                                    Điểm kết thúc
   khởi động   (điểm bắt đầu
   (không       phiên mã)
   được phiên mã)
 
                          ↓ PHIÊN MÃ (Transcription)
 
════════ PRE-mRNA (bản nháp, vẫn trong nhân) ════════
 
       [Exon1]──(Intron1)──[Exon2]──(Intron2)──[Exon3]
 
                          ↓ SPLICING (cắt intron, nối exon)
 
════════ mRNA TRƯỞNG THÀNH (đi ra tế bào chất) ════════
 
  Cap─[5'UTR][===== VÙNG MÃ HÓA (CDS) =====][3'UTR]─AAAAA
              ▲                            ▲
            AUG                          STOP
        (codon mở đầu)               (codon kết thúc)
 
                          ↓ DỊCH MÃ (Translation, ở ribosome)
 
                        PROTEIN
```
 
**Đọc bản đồ này như sau:** DNA có công tắc (promoter) rồi mới tới phần được chép ra. Phần chép ra là bản nháp lẫn lộn đoạn dùng được và đoạn thừa. Sau khi biên tập, bản hoàn chỉnh có phần đầu-đuôi không mã hóa protein (UTR) kẹp lấy phần mã hóa thật sự ở giữa.
 
---
 
## 2.2. PROMOTER — Công tắc khởi động của gene (0:15 – 0:35)
 
### Nó là gì?
 
**Promoter** (vùng khởi động) là **một đoạn DNA nằm ngay phía trước điểm bắt đầu của gene, có nhiệm vụ làm nơi bám cho bộ máy phiên mã**.
 
Điểm cực kỳ quan trọng và hay nhầm: **promoter KHÔNG được phiên mã thành mRNA.** Nó không nằm trong mRNA, không mã hóa amino acid nào. Nó chỉ là **địa chỉ và công tắc**.
 
🎛️ *So sánh trực quan (chỉ là cách hình dung):* Nếu gene là một bài hát trong máy nghe nhạc, thì promoter là **nút Play cùng với nhãn dán tên bài**. Nút Play không phải là một phần của giai điệu, nhưng không có nó thì bài hát không bao giờ phát.
*Còn về cơ chế sinh học thật:* promoter là một trình tự DNA có hình dạng hóa học đặc thù, khiến các protein chuyên biệt nhận ra và bám vào đúng chỗ đó.
 
### Nó giải quyết vấn đề gì? (Cơ chế)
 
**Cơ chế 1 — Định vị bắt đầu.** Trong 3 tỉ base, promoter đánh dấu "gene bắt đầu từ đây, và chép theo hướng này". Nó cũng xác định **TSS** (**Transcription Start Site** — điểm bắt đầu phiên mã, tức chữ cái đầu tiên được chép vào RNA).
 
**Cơ chế 2 — Điều hòa (quan trọng nhất).** Mọi tế bào trong cơ thể bạn chứa **cùng một bộ DNA**. Tế bào gan và tế bào thần kinh khác nhau **không phải** vì DNA khác nhau, mà vì **chúng bật/tắt những gene khác nhau**. Promoter chính là nơi quyết định việc bật/tắt đó.
 
> 🔑 Đây là ý tưởng nền tảng của cả lĩnh vực **điều hòa biểu hiện gene** (gene regulation) — và là lý do tồn tại của các kỹ thuật ATAC-seq, ChIP-seq, và phân tích motif mà bạn sẽ gặp trong roadmap.
 
### Nó hoạt động thế nào?
 
Bộ máy phiên mã không tự tìm đường. Nó cần được dẫn đến:
 **Cơ chế bắt đầu**
1. **Yếu tố phiên mã** (**transcription factor**, viết tắt **TF**) — là **những protein chuyên đi tìm và bám vào các đoạn DNA có trình tự đặc hiệu** trong vùng promoter. Mỗi TF chỉ nhận ra một dạng trình tự ngắn nhất định (gọi là **motif** — mẫu chuỗi đặc trưng, ví dụ `TATAAA`).
2. Các TF bám vào promoter tạo thành một "giàn giáo" protein.
3. Giàn giáo này **tuyển mộ** (recruit) **RNA polymerase** — enzyme chép DNA thành RNA mà ta đã học buổi 1 — đến đúng vị trí TSS.
4. RNA polymerase bắt đầu chép từ TSS đi xuôi về phía cuối gene.
### Một vài thành phần trình tự thường gặp
 
| Thành phần | Mô tả | Lưu ý trung thực |
|---|---|---|
| **TATA box** | Đoạn giàu chữ T và A (dạng `TATAAA`), nằm khoảng 25–30 base **phía trước** TSS ở sinh vật nhân thực | **Không phải gene nào cũng có.** Một tỉ lệ đáng kể promoter ở người không chứa TATA box |
| **CpG island** | Vùng DNA giàu cặp chữ C đi liền G. Nhiều promoter của các gene "chạy thường trực" thuộc dạng này | Là đặc trưng quan trọng, liên quan chặt tới methyl hóa DNA |
| **Enhancer** | Đoạn DNA **tăng cường** mức phiên mã, có thể nằm **rất xa** gene (hàng chục nghìn base), thậm chí phía sau gene | Đây là vùng điều hòa riêng, **không phải** promoter. Nhắc để bạn biết bức tranh rộng hơn |
 
*(Ghi chú: cơ chế promoter ở vi khuẩn khác khá nhiều — dùng các hộp trình tự ở vị trí −10 và −35 và protein sigma factor. Hôm nay ta tập trung vào sinh vật nhân thực vì đó là đối tượng chính của hầu hết dự án tin sinh học người/động vật.)*
 
### 🧬 Vì sao điều này quan trọng với tin sinh học?
 
- **Đột biến trong promoter** có thể gây bệnh mà **không hề làm thay đổi một amino acid nào** trong protein — vì nó chỉ làm gene bật quá nhiều hoặc quá ít. Người mới thường bỏ sót loại đột biến này vì chỉ nhìn vùng mã hóa.
- Các phân tích **motif enrichment** đi tìm xem promoter của một nhóm gene có cùng bị một TF nào đó điều khiển hay không.
- **ATAC-seq** đo xem vùng DNA nào đang "mở" (dễ tiếp cận) — promoter đang hoạt động thường ở trạng thái mở.
---
 
## 2.3. EXON & INTRON — Phần dùng được và phần bị cắt bỏ (0:35 – 0:55)
 
### Sự thật gây bất ngờ
 
RNA polymerase chép gene ra thành RNA. Nhưng bản RNA đầu tiên này — gọi là **pre-mRNA** (tiền-mRNA, tức **bản nháp**) — **chưa dùng được**.
 
Lý do: trong hầu hết gene của sinh vật nhân thực, thông tin mã hóa protein **không nằm liền một mạch**. Nó bị **ngắt quãng** bởi những đoạn không mang thông tin mã hóa.
 
### Định nghĩa chuẩn
 
| Thuật ngữ | Định nghĩa | Số phận |
|---|---|---|
| **Exon** | Đoạn của gene **được giữ lại** trong mRNA trưởng thành | Ở lại |
| **Intron** | Đoạn của gene **bị cắt bỏ** khi tạo mRNA trưởng thành | Bị loại |
 
📌 **Mẹo nhớ:** **Ex**on = **Ex**pressed (được biểu hiện, ở lại). **In**tron = **In**tervening (xen vào giữa, bị cắt).
 
### ⚠️ Hiểu lầm kinh điển — phải sửa ngay từ đầu
 
> ❌ **SAI:** "Exon = vùng mã hóa protein."
> ✅ **ĐÚNG:** "Exon = bất kỳ đoạn nào **được giữ lại trong mRNA trưởng thành**."
 
Đây là hai chuyện khác nhau. **Exon đầu tiên và exon cuối cùng thường chứa cả phần UTR** (phần không mã hóa protein — sẽ học ở mục 2.4). Nói cách khác:
 
$$\text{Exon} \;\supseteq\; \text{Vùng mã hóa}$$
 
Định nghĩa exon dựa trên **"có bị cắt hay không"**, không dựa trên **"có mã hóa protein hay không"**.
 
> 🔑 **Vì sao phải nhớ kỹ?** Vì khi bạn làm việc với file annotation (GTF/GFF) hoặc thiết kế **exome sequencing** (giải trình tự chỉ vùng exon), nhầm hai khái niệm này sẽ dẫn tới đếm sai vùng, chú thích sai biến thể.
 
### Quy mô thực tế — con số gây sốc
 
Ở người, phần intron thường **dài hơn rất nhiều** so với phần exon. Một gene có thể trải dài hàng chục nghìn đến hàng trăm nghìn base trên DNA, nhưng mRNA trưởng thành chỉ còn vài nghìn base.
 
Một hệ quả trực tiếp và rất thực dụng:
 
> **Toàn bộ vùng exon của người (exome) chỉ chiếm khoảng 1–2% bộ gen.** Đây chính là lý do kỹ thuật **Whole Exome Sequencing (WES)** tồn tại: nếu bạn chỉ quan tâm các đột biến làm thay đổi protein, bạn chỉ cần giải trình tự ~1–2% bộ gen → rẻ hơn nhiều so với giải toàn bộ (WGS), đổi lại bạn mất khả năng phát hiện đột biến ở promoter, intron và vùng điều hòa.
 
### Intron dùng để làm gì? — Trả lời trung thực
 
Câu hỏi "vì sao tiến hóa lại giữ lại intron tốn kém như vậy?" **chưa có câu trả lời thống nhất hoàn toàn** trong giới khoa học. Nhưng có những chức năng đã được chứng minh rõ ràng:
 
1. **Cho phép alternative splicing** — một gene tạo ra nhiều loại protein khác nhau (sẽ học ở mục 2.6). Đây là lợi ích rõ ràng và quan trọng nhất.
2. **Chứa các trình tự điều hòa** — một số enhancer và yếu tố điều hòa nằm bên trong intron.
3. **Chứa các RNA chức năng** — một số intron chứa trình tự mã hóa các RNA nhỏ có chức năng riêng (ví dụ một số microRNA, snoRNA).
*(Ngoài ra còn các giả thuyết tiến hóa như "exon shuffling" giúp tái tổ hợp các mô-đun protein — đây là giả thuyết có bằng chứng ủng hộ nhưng vẫn đang được thảo luận, mình nêu để bạn biết chứ không khẳng định là kết luận cuối cùng.)*
 
### Tế bào nhận ra ranh giới exon/intron bằng cách nào?
 
Đây là câu hỏi cơ khí quan trọng. Bộ máy cắt nối không "hiểu nghĩa", nó chỉ đọc **tín hiệu trình tự** đặt ở hai đầu intron:
 
| Vị trí | Tín hiệu | Tên gọi |
|---|---|---|
| Đầu 5' của intron (chỗ bắt đầu cắt) | Hầu như luôn là **GU** (trên RNA; tương ứng **GT** trên DNA) | Splice donor site |
| Cuối intron (chỗ kết thúc cắt) | Hầu như luôn là **AG** | Splice acceptor site |
| Gần cuối intron | Một chữ **A** đặc biệt | Branch point (điểm phân nhánh) |
| Giữa branch point và AG | Đoạn giàu chữ C và U | Polypyrimidine tract |
 
Quy tắc **GU–AG** này (còn gọi là quy tắc GT–AG khi viết trên DNA) đúng với đại đa số intron ở sinh vật nhân thực. Có một nhóm nhỏ intron dùng tín hiệu khác (ví dụ nhóm dùng AU–AC, được xử lý bởi bộ máy splicing phụ) — hiếm, nhưng có thật.
 
> 🚨 **Ứng dụng lâm sàng ngay:** Một đột biến làm hỏng chữ GU hoặc AG ở ranh giới intron được gọi là **splice site mutation**. Nó không nằm trong vùng mã hóa protein, nhưng làm bộ máy cắt nối "mù" → cắt sai → protein hỏng hoàn toàn. Đây là nhóm đột biến gây bệnh quan trọng, và là lý do các công cụ chú thích biến thể (như VEP, SnpEff) luôn có nhãn riêng cho vùng splice site.
 
---
 
## 📝 BÀI TẬP 2 — Xác định exon/intron trên trình tự (0:55 – 1:05)
 
*(Đáp án ở mục "✅ ĐÁP ÁN — Bài tập 2")*
 
Cho một gene giả định (viết theo mạch sense của DNA, tức mạch có trình tự giống mRNA, chỉ khác T↔U). Gene này có **1 intron**:
 
```
5'- ATGGCATTC GTAAGCTTACTGACCTTTGCAG GGTACCTAA -3'
```
 
*(Khoảng trắng chỉ để bạn dễ nhìn; thực tế trình tự liền nhau.)*
 
**Bài 2.1.** Dựa vào quy tắc GT–AG, hãy xác định: đoạn nào là **intron**? Đoạn nào là **exon 1** và **exon 2**?
 
**Bài 2.2.** Viết ra trình tự **mRNA trưởng thành** (sau khi cắt intron, nối exon). Nhớ đổi T → U.
 
**Bài 2.3.** Chia mRNA trưởng thành thành các codon và dịch ra chuỗi amino acid. (Dùng bảng mã di truyền buổi 1. Gợi ý: AUG=Met, GCA=Ala, UUC=Phe, GGU=Gly, ACC=Thr, UAA=Stop)
 
**Bài 2.4.** **Câu hỏi tư duy:** Giả sử một đột biến làm hỏng chữ `GT` ở đầu intron, khiến intron **không bị cắt**. Hãy chia lại toàn bộ trình tự (kể cả intron) thành codon và cho biết chuyện gì xảy ra với protein. Điều này minh họa điều gì về tầm quan trọng của splicing?
 
---
 
## 2.4. UTR — Vùng không dịch mã (1:05 – 1:25)
 
*(Nghỉ ngắn 5 phút trước mục này nếu cần)*
 
### Xuất phát từ một quan sát
 
Buổi 1 ta nói: ribosome bắt đầu đọc tại **AUG** và dừng tại **STOP**. Nhưng mRNA trưởng thành **không bắt đầu đúng ngay chữ A của AUG** và cũng **không kết thúc ngay sau STOP**.
 
Hai phần "dư ra" đó chính là **UTR**.
 
### Định nghĩa
 
**UTR** = **Untranslated Region** = **Vùng không được dịch mã**. Nghĩa là: phần này **có nằm trong mRNA**, **có được phiên mã ra**, nhưng **không được ribosome dịch thành amino acid**.
 
| Tên | Vị trí | Giới hạn |
|---|---|---|
| **5' UTR** | Đầu mRNA, **trước** codon AUG | Từ TSS đến ngay trước AUG |
| **3' UTR** | Cuối mRNA, **sau** codon STOP | Từ ngay sau STOP đến cuối mRNA |
 
*(Ký hiệu 5' và 3' — đọc là "năm phẩy" và "ba phẩy" — là cách đánh dấu **hai đầu khác nhau** của một sợi RNA/DNA, dựa trên cấu trúc hóa học của phân tử đường. Bạn chỉ cần nhớ: RNA luôn được đọc theo chiều **5' → 3'**, nên 5' là "đầu", 3' là "đuôi".)*
 
### ⚠️ Nối lại với mục 2.3
 
**UTR nằm BÊN TRONG exon.** Cụ thể: 5'UTR nằm trong exon đầu tiên (và có thể trải qua vài exon đầu), 3'UTR nằm trong exon cuối cùng. Chúng **không bị cắt bỏ** — vì vậy theo định nghĩa, chúng là exon.
 
Đây chính là lý do mình nhấn mạnh ở mục 2.3: *exon ≠ vùng mã hóa*.
 
Ta có 3 lớp khái niệm lồng nhau, cần phân biệt rạch ròi:
 
```
Gene (trên DNA)  ⊃  Exon (phần giữ lại trong mRNA)  ⊃  CDS (phần thật sự mã hóa protein)
```
 
**CDS** = **Coding Sequence** = phần từ AUG đến STOP, là phần duy nhất được dịch thành amino acid.
 
### UTR dùng để làm gì? — Nó không hề "thừa"
 
Cái tên "không được dịch mã" dễ gây hiểu lầm rằng nó vô dụng. Thực tế UTR là **trung tâm điều khiển** của mRNA.
 
**🔹 Vai trò của 5'UTR:**
 
| Chức năng | Giải thích |
|---|---|
| Nơi ribosome bám vào | Ribosome cần một đoạn "đường băng" để bám vào mRNA trước khi tìm được AUG. Không có 5'UTR, việc khởi đầu dịch mã kém hiệu quả |
| Kiểm soát hiệu suất dịch mã | Trình tự bao quanh AUG (**Kozak sequence** ở sinh vật nhân thực) ảnh hưởng tới việc ribosome có nhận ra AUG đó hay bỏ qua |
| Có thể chứa cấu trúc điều hòa | 5'UTR có thể gấp thành cấu trúc không gian, hoặc chứa "AUG giả" (upstream ORF) làm giảm dịch mã của gene chính |
 
**🔹 Vai trò của 3'UTR:**
 
| Chức năng | Giải thích |
|---|---|
| Quyết định **tuổi thọ** của mRNA | mRNA không sống mãi. 3'UTR chứa tín hiệu quyết định mRNA bị phân hủy nhanh hay tồn tại lâu → ảnh hưởng trực tiếp lượng protein tạo ra |
| Nơi **microRNA** bám vào | **microRNA (miRNA)** là những RNA rất ngắn (~22 nucleotide) không mã hóa protein. Chúng bám vào 3'UTR theo nguyên tắc bắt cặp bổ sung, làm **giảm** dịch mã hoặc phân hủy mRNA đó. Đây là một cơ chế điều hòa gene cực kỳ phổ biến |
| Chứa tín hiệu polyadenyl hóa | Trình tự `AAUAAA` báo hiệu nơi cắt và gắn đuôi poly(A) |
 
> 🔑 **Điểm mấu chốt:** Cùng một CDS (cùng một protein), nhưng UTR khác nhau → lượng protein tạo ra khác nhau rất nhiều. Đột biến trong UTR hoàn toàn có thể gây bệnh dù protein không đổi một chữ nào.
 
### Hai "phụ kiện" gắn vào hai đầu mRNA
 
Trong quá trình trưởng thành, mRNA còn được gắn thêm:
 
- **5' cap** (mũ đầu 5'): một phân tử đặc biệt (7-methylguanosine) gắn ở đầu 5'. Chức năng: **bảo vệ** mRNA khỏi bị enzyme phân hủy, và **giúp ribosome nhận ra** mRNA để bắt đầu dịch mã.
- **Đuôi poly(A)** (poly-A tail): một chuỗi dài gồm hàng chục đến hàng trăm chữ **A** gắn ở đầu 3'. Chức năng: **bảo vệ** đầu đuôi và **ảnh hưởng tới độ bền** của mRNA.
🧬 **Ứng dụng tin sinh học ngay:** Nhiều quy trình chuẩn bị mẫu RNA-seq dùng hạt gắn chuỗi chữ T để "câu" các mRNA có đuôi poly(A) (gọi là **poly(A) selection**). Hệ quả thực tế bạn phải biết: cách làm này **chỉ bắt được các RNA có đuôi poly(A)** — các RNA không có đuôi này sẽ bị bỏ sót khỏi dữ liệu của bạn. Nếu đề tài của bạn quan tâm nhóm RNA đó, bạn phải chọn quy trình khác (ví dụ ribo-depletion).
 
---
 
## 2.5. SPLICING — Cơ chế cắt nối (1:25 – 1:45)
 
### Nó là gì?
 
**Splicing** (cắt–nối RNA) là **quá trình loại bỏ intron ra khỏi pre-mRNA và nối các exon lại với nhau** để tạo mRNA trưởng thành.
 
🎬 *So sánh trực quan:* Giống **dựng phim**. Quay xong bạn có một đoạn băng thô lẫn cả cảnh dùng được và cảnh hỏng. Người dựng cắt bỏ cảnh hỏng, ghép các cảnh dùng được lại thành phim hoàn chỉnh.
*Cơ chế sinh học thật:* việc "cắt" và "nối" là các phản ứng hóa học phá vỡ và tạo mới liên kết trong chuỗi RNA, do một cỗ máy phân tử thực hiện.
 
### Ai làm việc đó? — Spliceosome
 
**Spliceosome** (thể cắt nối) là **cỗ máy phân tử thực hiện splicing**. Nó không phải một protein đơn lẻ mà là **một phức hợp lớn gồm nhiều RNA nhỏ và protein**.
 
Thành phần chính là các **snRNP** (đọc là "snurp", viết tắt của *small nuclear ribonucleoprotein*) — mỗi snRNP gồm một đoạn RNA nhỏ trong nhân kết hợp với các protein. Các snRNP chính tham gia là **U1, U2, U4, U5, U6**.
 
> 💡 **Điểm thú vị đáng chú ý:** Chính **RNA** trong spliceosome (chứ không phải protein) đóng vai trò trung tâm trong việc xúc tác phản ứng cắt nối. Điều này song song với ribosome ở buổi 1 — nơi rRNA cũng là thành phần xúc tác chính. RNA không chỉ là "vật mang tin", nó còn **làm việc**.
 
### Cơ chế hoạt động — 4 bước
 
**Bước 1 — Nhận diện.** snRNP **U1** bám vào vị trí **GU** ở đầu intron. snRNP **U2** bám vào **branch point** (chữ A đặc biệt gần cuối intron). Lúc này hai đầu intron đã được "đánh dấu".
 
**Bước 2 — Lắp ráp.** Các snRNP còn lại (U4, U5, U6) được tuyển thêm vào, kéo hai đầu intron lại gần nhau, uốn intron thành một vòng.
 
**Bước 3 — Cắt lần 1.** Chữ **A** ở branch point tấn công vào vị trí **GU** ở đầu intron, cắt rời exon 1 ra khỏi intron. Intron lúc này bị uốn thành hình **vòng thòng lọng** (gọi là **lariat** — giống cái dây thòng lọng của cao bồi).
 
**Bước 4 — Cắt lần 2 và nối.** Đầu vừa được giải phóng của exon 1 tấn công vào vị trí **AG** ở cuối intron: intron (dạng lariat) bị cắt rời hẳn ra và sau đó bị phân hủy, đồng thời **exon 1 được nối trực tiếp với exon 2**.
 
Lặp lại cho mọi intron còn lại → mRNA trưởng thành chỉ còn các exon nối liền nhau.
 
```
pre-mRNA:   [Exon1]──GU~~~~A~~~AG──[Exon2]
                      ↑     ↑   ↑
                     U1    U2  (acceptor)
 
Sau splicing:  [Exon1][Exon2]   +   intron dạng lariat (bị phân hủy)
```
 
### 🧬 Hệ quả cực lớn cho tin sinh học: bài toán alignment
 
Đây là điểm bạn sẽ đụng tới **ngay** khi chạy pipeline RNA-seq đầu tiên.
 
Khi bạn giải trình tự RNA, bạn thu được reads từ **mRNA trưởng thành** — tức là **đã cắt intron rồi**. Nhưng bạn lại phải **ánh xạ (align)** các reads đó **về bộ gen tham chiếu** — nơi intron **vẫn còn nguyên**.
 
Hậu quả: một read nằm vắt qua ranh giới hai exon (gọi là **junction read**) sẽ **không khớp liên tục** ở bất kỳ đâu trên bộ gen. Nửa đầu read khớp ở exon 1, nửa sau khớp ở exon 2, và giữa chúng là một khoảng trống dài hàng nghìn base (chính là intron).
 
```
Read (từ mRNA):        AAAAAAAAAA|BBBBBBBBBB
                            ↓            ↓
Genome (DNA):   ...[Exon1 ...AAAAAAAAAA]────(Intron dài)────[BBBBBBBBBB... Exon2]...
```
 
> 🚨 **Kết luận thực hành bắt buộc nhớ:** RNA-seq **phải** dùng công cụ align **splice-aware** (biết về splicing) như **STAR** hoặc **HISAT2** — những công cụ cho phép "nhảy qua" intron. Nếu bạn dùng công cụ align DNA thông thường (như BWA, Bowtie2 ở chế độ end-to-end thông thường), các junction read sẽ bị mất hoặc bị align sai → **đếm thiếu biểu hiện gene một cách có hệ thống**.
>
> Đây là một trong những lỗi phổ biến nhất của người mới làm RNA-seq, và nó **không báo lỗi** — pipeline vẫn chạy trơn tru, chỉ có kết quả là sai.
 
---
 
## 2.6. ALTERNATIVE SPLICING — Một gene, nhiều protein (1:45 – 1:55)
 
### Bài toán nó giải quyết
 
Bộ gen người có khoảng **20.000** gene mã hóa protein. Nhưng cơ thể người tạo ra số loại protein **nhiều hơn con số đó rất nhiều**. Làm sao?
 
→ Một phần lớn câu trả lời nằm ở **alternative splicing** (cắt nối luân phiên).
 
### Nó là gì?
 
**Alternative splicing** là hiện tượng **cùng một pre-mRNA có thể được cắt nối theo nhiều cách khác nhau**, tạo ra nhiều mRNA trưởng thành khác nhau → nhiều protein khác nhau, từ **cùng một gene**.
 
🍔 *So sánh trực quan:* Cùng một công thức bánh mì gồm nhiều lớp nguyên liệu. Khách A lấy đủ 3 lớp, khách B bỏ lớp giữa, khách C chọn lớp phô mai thay vì thịt. Cùng một "gene công thức", ra nhiều sản phẩm khác nhau.
 
### Các kiểu cắt nối luân phiên chính
 
| Kiểu | Mô tả |
|---|---|
| **Exon skipping** (bỏ qua exon) | Một exon bị cắt bỏ cùng với intron hai bên. Đây là kiểu phổ biến nhất ở sinh vật nhân thực |
| **Alternative 5' splice site** | Dùng điểm cắt đầu intron ở vị trí khác → exon dài/ngắn hơn |
| **Alternative 3' splice site** | Dùng điểm cắt cuối intron ở vị trí khác |
| **Mutually exclusive exons** | Hai exon "loại trừ nhau" — chỉ một trong hai được giữ, không bao giờ cả hai |
| **Intron retention** (giữ lại intron) | Intron không bị cắt mà ở lại trong mRNA |
 
### Quy mô thực tế
 
Các nghiên cứu giải trình tự transcriptome quy mô lớn ước tính rằng **phần lớn các gene người có nhiều exon đều trải qua alternative splicing** — các ước lượng được trích dẫn rộng rãi đặt con số này ở mức rất cao (khoảng 90–95%). Mình nêu đây là **ước lượng**, vì con số cụ thể phụ thuộc vào mô được khảo sát và độ sâu giải trình tự.
 
**Hệ quả quan trọng về mặt khái niệm:**
 
> Một gene **không** tương ứng với đúng một protein. Một gene tương ứng với **nhiều biến thể phiên mã** (gọi là **transcript** hoặc **isoform**), mỗi biến thể có thể cho ra một protein hơi khác nhau.
 
### 🧬 Hệ quả cho tin sinh học
 
1. **File annotation (GTF/GFF) liệt kê transcript, không chỉ gene.** Một gene sẽ có nhiều dòng transcript con. Bạn phải biết mình đang đếm ở mức **gene** hay mức **transcript**.
2. **Bài toán quantification khó hơn tưởng tượng.** Khi một read rơi vào exon dùng chung giữa nhiều isoform, bạn không biết nó đến từ isoform nào. Các công cụ như **Salmon**, **kallisto**, **RSEM** dùng mô hình **thống kê** (ước lượng khả năng tối đa / EM algorithm) để phân bổ read — và đây chính là nơi kiến thức xác suất ở chủ đề 1 gặp lại sinh học của chủ đề 2.
3. **Splicing bất thường gây bệnh.** Một số bệnh di truyền phát sinh do splicing sai chứ không do đột biến trong CDS. Phân tích **differential splicing** là một hướng phân tích riêng, khác với differential expression.
---
 
## 🔄 TỔNG KẾT DÒNG CHẢY ĐẦY ĐỦ (1:55 – 2:00)
 
Đây là bản nâng cấp của sơ đồ buổi 1, giờ đã có đủ chi tiết:
 
```
① DNA:        [PROMOTER]→[Ex1]─(In1)─[Ex2]─(In2)─[Ex3]
                  ↑
            TF bám vào, tuyển RNA polymerase
                  ↓
② PHIÊN MÃ (Transcription) — trong nhân
                  ↓
③ pre-mRNA:   [Ex1]─(In1)─[Ex2]─(In2)─[Ex3]      ← bản nháp
                  ↓
④ TRƯỞNG THÀNH RNA — trong nhân:
      · Gắn 5' cap
      · SPLICING (spliceosome cắt intron, nối exon)
      · Gắn đuôi poly(A)
                  ↓
⑤ mRNA trưởng thành:  Cap─[5'UTR][=== CDS ===][3'UTR]─AAAAA
                  ↓
            Rời nhân → ra TẾ BÀO CHẤT
                  ↓
⑥ DỊCH MÃ (Translation) — ribosome đọc CDS từ AUG đến STOP
                  ↓
⑦ PROTEIN
```
 
**Ba câu hỏi mở đầu chủ đề 2, giờ đã có đáp án:**
 
| Câu hỏi | Đáp án |
|---|---|
| Ai ra lệnh bắt đầu? | **Promoter** + transcription factors tuyển RNA polymerase đến TSS |
| Phần thừa bị cắt là gì? | **Intron**, bị **spliceosome** cắt bỏ qua quá trình **splicing**; phần giữ lại là **exon** |
| mRNA có phải toàn bộ đều mã hóa? | Không. Chỉ **CDS** được dịch. Hai đầu là **5'UTR** và **3'UTR** — không mã hóa nhưng điều khiển hiệu suất và tuổi thọ mRNA |
 
---
 
## 📝 QUIZ CHỦ ĐỀ 2 (10 câu)
 
*(Đáp án ở mục "✅ ĐÁP ÁN — Quiz chủ đề 2")*
 
**Câu 1.** Promoter có được phiên mã thành mRNA không? Vai trò chính của nó là gì?
 
**Câu 2.** TSS là viết tắt của gì và nó đánh dấu điểm nào?
 
**Câu 3.** Định nghĩa exon và intron. Tiêu chí phân biệt là gì (mã hóa hay không, hay bị cắt hay không)?
 
**Câu 4.** Câu sau đúng hay sai, giải thích: *"Exon chính là vùng mã hóa protein."*
 
**Câu 5.** Quy tắc GU–AG nói về điều gì? Nó nằm ở đâu?
 
**Câu 6.** 5'UTR và 3'UTR nằm ở vị trí nào so với codon AUG và STOP? Nêu 1 chức năng của mỗi loại.
 
**Câu 7.** Spliceosome là gì? Thành phần chính của nó là gì?
 
**Câu 8.** Alternative splicing là gì? Nó giải thích được điều gì về số lượng protein so với số lượng gene?
 
**Câu 9.** Vì sao RNA-seq bắt buộc phải dùng aligner splice-aware (STAR/HISAT2) thay vì aligner DNA thông thường? Điều gì sẽ xảy ra nếu dùng sai?
 
**Câu 10.** Sắp xếp các khái niệm sau theo quan hệ bao hàm từ lớn đến nhỏ: **CDS**, **Gene**, **Exon**. Giải thích ngắn.
 
---
---
 
## ✅ ĐÁP ÁN — Bài tập 2 (Exon/Intron)
 
Trình tự gốc: `ATGGCATTC GTAAGCTTACTGACCTTTGCAG GGTACCTAA`
 
**Bài 2.1.** Áp dụng quy tắc GT–AG (trên DNA):
- **Exon 1** = `ATGGCATTC`
- **Intron** = `GTAAGCTTACTGACCTTTGCAG` — bắt đầu bằng **GT** ✓, kết thúc bằng **AG** ✓
- **Exon 2** = `GGTACCTAA`
**Bài 2.2.** Nối exon 1 + exon 2, đổi T → U:
```
DNA sau splicing:  ATGGCATTCGGTACCTAA
mRNA trưởng thành: AUGGCAUUCGGUACCUAA
```
 
**Bài 2.3.** Chia codon:
```
AUG   GCA   UUC   GGU   ACC   UAA
Met - Ala - Phe - Gly - Thr - STOP
```
→ Protein gồm **5 amino acid**: Met–Ala–Phe–Gly–Thr, rồi dừng lại tại codon UAA.
 
**Bài 2.4.** Nếu intron **không bị cắt**, ribosome đọc toàn bộ trình tự:
```
AUG GCA UUC GUA AGC UUA CUG ACC UUU GCA GGG UAC CUA A
Met Ala Phe Val Ser Leu Leu Thr Phe Ala Gly Tyr Leu ...
```
**Chuyện gì xảy ra:**
- Sau 3 amino acid đầu (Met-Ala-Phe) giống bản đúng, mọi amino acid tiếp theo đều **sai hoàn toàn** so với protein mong muốn.
- Codon STOP (UAA) đúng vị trí đã **biến mất khỏi khung đọc** — ribosome đọc tiếp qua nó. Protein bị kéo dài bất thường và có trình tự vô nghĩa.
**Minh họa điều gì:** Splicing không phải bước phụ trợ mà là bước **bắt buộc và chính xác đến từng base**. Chỉ cần một đột biến ở chữ `GT` — nằm **hoàn toàn bên ngoài** vùng mã hóa protein — cũng đủ phá hủy toàn bộ protein. Đây chính xác là cơ chế của nhóm bệnh do **splice site mutation**, và là lý do phân tích biến thể không được phép chỉ nhìn vào vùng CDS.
 
---
 
## ✅ ĐÁP ÁN — Quiz chủ đề 2
 
**1.** **Không.** Promoter không được phiên mã, không nằm trong mRNA. Vai trò: làm **nơi bám** cho transcription factors và RNA polymerase, xác định điểm bắt đầu và hướng phiên mã, đồng thời là **trung tâm điều hòa** quyết định gene được bật hay tắt trong từng loại tế bào.
 
**2.** **TSS = Transcription Start Site** (điểm bắt đầu phiên mã) — đánh dấu **base đầu tiên** được chép vào RNA.
 
**3.** **Exon** = đoạn được **giữ lại** trong mRNA trưởng thành. **Intron** = đoạn bị **cắt bỏ**. Tiêu chí phân biệt là **bị cắt hay không**, **không phải** có mã hóa protein hay không.
 
**4.** **SAI.** Exon bao gồm cả vùng UTR — phần nằm trong mRNA trưởng thành nhưng không được dịch thành amino acid. Exon đầu tiên chứa 5'UTR, exon cuối chứa 3'UTR. Đúng phải là: **Exon ⊇ CDS**.
 
**5.** Quy tắc **GU–AG** mô tả tín hiệu trình tự ở **hai đầu intron**: intron hầu như luôn bắt đầu bằng **GU** (splice donor) và kết thúc bằng **AG** (splice acceptor) trên RNA — tương ứng GT và AG trên DNA. Đây là tín hiệu để spliceosome nhận ra ranh giới cần cắt.
 
**6.** **5'UTR** nằm **trước** AUG; **3'UTR** nằm **sau** codon STOP. Chức năng (nêu 1 mỗi loại): 5'UTR giúp ribosome bám vào và kiểm soát hiệu suất khởi đầu dịch mã; 3'UTR quyết định tuổi thọ mRNA và là nơi microRNA bám vào để điều hòa.
 
**7.** **Spliceosome** là cỗ máy phân tử thực hiện splicing. Thành phần chính là các **snRNP** (U1, U2, U4, U5, U6) — mỗi snRNP gồm RNA nhỏ trong nhân kết hợp với protein. Đáng chú ý: chính RNA đóng vai trò xúc tác trung tâm.
 
**8.** **Alternative splicing** = cùng một pre-mRNA được cắt nối theo nhiều cách khác nhau → nhiều mRNA/protein khác nhau từ **một gene**. Nó giải thích vì sao ~20.000 gene người có thể tạo ra số loại protein lớn hơn nhiều: **một gene → nhiều transcript/isoform**.
 
**9.** Vì reads RNA-seq đến từ mRNA **đã cắt intron**, nhưng phải align về genome **còn nguyên intron**. Read nằm vắt qua ranh giới exon-exon (junction read) không khớp liên tục ở bất kỳ vị trí nào trên genome. Aligner splice-aware biết "nhảy qua" intron. **Nếu dùng sai:** junction reads bị mất hoặc align sai → **đếm thiếu biểu hiện gene một cách có hệ thống**, và pipeline **không báo lỗi** nên rất khó phát hiện.
 
**10.** Thứ tự bao hàm:
$$\textbf{Gene} \supset \textbf{Exon} \supset \textbf{CDS}$$
- **Gene** = toàn bộ vùng trên DNA, gồm cả exon và intron (thường tính cả vùng điều hòa liên quan tùy định nghĩa được dùng).
- **Exon** = phần của gene được giữ lại trong mRNA trưởng thành (gồm cả UTR).
- **CDS** = phần của exon thật sự được dịch thành protein, từ AUG đến STOP.
---
## 📌 BẢN ĐỒ TƯ DUY CUỐI BUỔI
**Chủ đề 2 — Cấu trúc gene:**
- **Promoter**: công tắc + địa chỉ, không được phiên mã, quyết định gene bật/tắt.
- **Exon/Intron**: phân biệt theo **bị cắt hay không**, không theo mã hóa hay không.
- **UTR**: nằm trong exon, không dịch thành protein nhưng điều khiển hiệu suất dịch mã và tuổi thọ mRNA.
- **Splicing**: spliceosome cắt intron theo tín hiệu GU–AG. Sai một base = hỏng cả protein.
- **Alternative splicing**: một gene → nhiều transcript. Đây là lý do quantification RNA-seq là bài toán thống kê, không phải đếm đơn giản.
**🔗 Điểm hai chủ đề gặp nhau:** Bài toán gán read cho isoform trong RNA-seq quantification (Salmon, kallisto, RSEM) đòi hỏi **cả** hiểu biết về cấu trúc gene (chủ đề 2) **lẫn** mô hình xác suất (chủ đề 1). Đây không phải hai môn học tách rời.
 
---
