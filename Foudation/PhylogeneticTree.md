# Đọc lịch sử tiến hóa từ chuỗi DNA


Cây phát sinh loài, căn chỉnh đa trình tự (MSA), Maximum Likelihood và IQ-TREE, giải thích từ gốc cho người chưa từng học sinh học.


> Công thức, con số, lệnh và giá trị mặc định của phần mềm trong bài đã được đối chiếu với tài liệu chính thức của IQ-TREE, MAFFT, trimAl và các bài báo gốc (tra cứu ngày 18/09/2026). Nguồn được liệt kê ở cuối mỗi phần. Chỗ nào giới khoa học còn tranh luận, bài nói rõ là đang tranh luận.


> 🖼️ **[Hình minh họa — chỉ xem được trong bản artifact HTML gốc]**
>
> *Mô tả hình:* Một cây phát sinh loài bốn lá ở bên trái, nối tới bốn chuỗi DNA đã căn chỉnh ở bên phải, mỗi nucleotide một màu
>
> Hình quen thuộc trong mọi bài báo phát sinh loài: cây ở bên trái, các chuỗi đã căn chỉnh ở bên phải, mỗi màu là một loại nucleotide. Bốn chuỗi này là dữ liệu tự tạo để học; chúng sẽ được dùng lại ở phần 9.


## Mục lục


- **0.** [Bốn chủ đề, một quy trình](#map)
- **1.** [DNA và dấu vết lịch sử](#s1)
- **2.** [Cây phát sinh loài](#s2)
- **3.** [Căn chỉnh đa trình tự (MSA)](#s3)
- **4.** [Bùng nổ số lượng cây](#s4)
- **5.** [Maximum likelihood](#s5)
- **6.** [Mô hình thay thế](#s6)
- **7.** [Tính likelihood trên cây](#s7)
- **8.** [Tìm cây tốt nhất](#s8)
- **9.** [Độ tin cậy của nhánh](#s9)
- **10.** [IQ-TREE thực hành](#s10)
- **11.** [Giới hạn và checklist](#s11)
- **12.** [Thuật ngữ và tài liệu](#s12)


---


## 0. Bốn chủ đề, một quy trình

Bốn chủ đề cây phát sinh loài (Phylogenetic Tree), MSA, IQ-TREE và maximum likelihood. Nhìn qua thì giống bốn bài riêng. Thực ra chúng là bốn khâu nối tiếp nhau của cùng một công việc: **dựng lại quan hệ họ hàng giữa các loài từ chuỗi DNA của chúng**.

1. **Chuỗi DNA thô** — Cùng một gen, lấy từ nhiều loài.
2. **Căn chỉnh** — Xếp các chuỗi thẳng hàng theo vị trí tương ứng. _[MSA]_
3. **Chấm điểm một cây** — Mô hình xác suất cho biết cây giải thích dữ liệu tốt đến đâu. _[Maximum likelihood]_
4. **Tìm và kiểm định** — Tìm cây điểm cao nhất, đo độ tin cậy từng nhánh. _[IQ-TREE]_
5. **Kết quả** — Cây họ hàng kèm con số hỗ trợ. _[Cây phát sinh loài]_

| Chủ đề | Vai trò trong quy trình | Câu hỏi nó trả lời |
|---|---|---|
| Cây phát sinh loài | Sản phẩm cuối cùng | Các loài có quan hệ họ hàng với nhau như thế nào? |
| MSA | Chuẩn bị dữ liệu | Vị trí nào trong chuỗi này tương ứng với vị trí nào trong chuỗi kia? |
| Maximum likelihood | Tiêu chí chấm điểm | Trong rất nhiều cây có thể có, cây nào giải thích dữ liệu tốt nhất? |
| IQ-TREE | Công cụ thực thi | Làm sao tính toán và tìm kiếm tất cả những điều trên trong thời gian chấp nhận được? |

Bài học đi đúng theo thứ tự dữ liệu chạy qua quy trình. Nhưng trước khi vào khâu đầu tiên, bạn cần ba ý nền mà mọi khâu phía sau đều dựa vào: DNA là gì, DNA thay đổi thế nào qua các thế hệ, và vì sao sự thay đổi ấy để lại dấu vết của lịch sử. Phần 1 dạy đúng ba ý đó, không hơn.

> **💡 Lưu ý:** **Cách đọc bài này.** Mỗi phần kết thúc bằng một *trạm dừng*: tóm lại điều vừa biết và lý do cần bước tiếp theo. Khung viền nét đứt là *ví von*, chỉ để dễ hình dung; khung viền nét liền là *cơ chế thật*. Mục “Kiến thức bổ sung” có thể bỏ qua ở lần đọc đầu. Các khung tương tác cho phép bạn tự đổi số và quan sát điều gì thay đổi.


---


## 1. DNA, đột biến và dấu vết của lịch sử

### DNA là một chuỗi ký tự

Mọi sinh vật đều mang trong tế bào một phân tử rất dài gọi là **DNA**. DNA chứa *thông tin di truyền*: thông tin được truyền từ cha mẹ sang con cái và quyết định nhiều đặc điểm của sinh vật.

Về hóa học, DNA được nối từ bốn loại đơn vị nhỏ gọi là **nucleotide**. Bốn loại này được ký hiệu bằng bốn chữ cái `A` `C` `G` `T`. Với tin sinh học, điều quan trọng nhất cần nhớ là: **ta có thể coi DNA như một chuỗi văn bản chỉ dùng bốn chữ cái**, ví dụ `ATGCTAGCTA`.

Một đoạn DNA đảm nhận một chức năng cụ thể gọi là **gen** *(gene)*. Khi dựng cây phát sinh loài, ta thường lấy *cùng một gen* từ nhiều loài khác nhau rồi so sánh chúng.

#### 📎 Kiến thức bổ sung (chuỗi protein)

Nhiều gen được tế bào dùng làm “bản thiết kế” để tạo ra *protein*. Protein cũng là một chuỗi, nhưng dùng 20 loại ký tự (20 loại amino acid). IQ-TREE xử lý được cả chuỗi DNA, chuỗi protein và một số kiểu dữ liệu khác. Bài này dùng DNA cho đơn giản; logic với protein là như nhau, chỉ khác bảng chữ cái và mô hình.

### DNA được sao chép, và đôi khi chép sai

Khi sinh vật sinh sản, DNA được sao chép để truyền cho thế hệ sau. Việc sao chép rất chính xác nhưng không hoàn hảo. Thỉnh thoảng có một thay đổi xảy ra; thay đổi đó gọi là **đột biến** *(mutation)*. Ba kiểu đột biến quan trọng cho bài này:

| Kiểu | Điều xảy ra | Ví dụ (trước → sau) |
|---|---|---|
| Thay thế *(substitution)* | Một chữ đổi thành chữ khác | `ATGCTA` → `ATGTTA` |
| Chèn *(insertion)* | Thêm chữ mới vào chuỗi | `ATGCTA` → `ATGGCTA` |
| Mất *(deletion)* | Một chữ biến mất khỏi chuỗi | `ATGCTA` → `ATGTA` |

Chèn và mất thường được gọi chung là **indel** (ghép từ *insertion* và *deletion*). Hãy nhớ indel: chính nó là lý do ta cần MSA ở phần 3.

### Vì sao chuỗi DNA “nhớ” được lịch sử

Đột biến được di truyền: con cháu nhận bản DNA đã mang thay đổi. Giờ hãy hình dung một loài tổ tiên tách thành hai nhánh, chẳng hạn vì hai quần thể bị cách ly về địa lý. Từ lúc tách ra, mỗi nhánh tích lũy đột biến của riêng mình, độc lập với nhánh kia.

Hệ quả: **hai loài tách nhau càng lâu thì DNA của chúng thường càng khác nhau; hai loài mới tách nhau gần đây thì DNA còn giống nhau nhiều.** Vì vậy, mức giống và khác giữa các chuỗi hôm nay mang thông tin về thứ tự tách nhánh trong quá khứ. Toàn bộ ngành phát sinh loài phân tử dựa trên ý này.

Chữ “thường” ở trên là có chủ đích. Đây là xu hướng trung bình, không phải quy luật tuyệt đối: tốc độ thay đổi có thể khác nhau giữa các nhánh và giữa các vị trí trong gen. Phần 6 sẽ xử lý điều này bằng mô hình xác suất.

> **Ví von:** Một cuốn sách được chép tay qua nhiều đời. Người chép nào cũng có thể mắc vài lỗi, và lỗi đó được người sau chép lại nguyên xi. Nhìn các bản còn sót lại hôm nay, bạn đoán được bản nào chép từ bản nào nhờ những lỗi mà chúng có chung.
>
> **Cơ chế thật:** Đột biến xảy ra trong DNA của một cá thể. Để trở thành khác biệt giữa hai loài, thay đổi đó phải lan rộng và trở nên phổ biến trong cả quần thể qua nhiều thế hệ. Khi ấy các nhà tiến hóa gọi nó là một **thay thế** *(substitution)*. Các mô hình trong IQ-TREE mô tả tốc độ của những thay thế này dọc theo các dòng dõi, không mô tả từng đột biến riêng lẻ.

### Tương đồng: chỉ so sánh những thứ cùng nguồn gốc

Khái niệm cuối cùng của phần nền. Hai vị trí (hoặc hai gen) ở hai loài được gọi là **tương đồng** *(homologous)* nếu chúng cùng bắt nguồn từ một vị trí (hoặc một gen) ở tổ tiên chung.

So sánh chỉ có ý nghĩa tiến hóa khi ta so những thứ tương đồng. Đem chữ thứ 5 của một gen ở người so với chữ thứ 5 của một gen chẳng liên quan ở chuột thì không nói lên gì về lịch sử, kể cả khi hai chữ đó tình cờ giống nhau.

#### 📎 Kiến thức bổ sung (ortholog và paralog)

Gen tương đồng có hai kiểu chính. **Ortholog** là cùng một gen ở hai loài, tách ra khi loài tổ tiên tách thành hai loài. **Paralog** là hai bản sao tách ra vì gen bị nhân đôi bên trong một bộ gen. Muốn dựng cây phản ánh lịch sử *loài*, người ta thường chọn ortholog; trộn lẫn paralog có thể cho ra cây phản ánh lịch sử nhân đôi gen thay vì lịch sử loài.

#### 🚩 Trạm dừng sau phần 1

**Giờ ta đã biết**

- DNA có thể xem là chuỗi bốn chữ A, C, G, T.
- Đột biến (thay thế, chèn, mất) tích lũy và được di truyền.
- Mức giống và khác giữa các chuỗi phản ánh lịch sử tách nhánh.
- Chỉ so sánh các vị trí tương đồng mới có nghĩa.

**Vì sao cần bước tiếp**

Ta đã có nguyên liệu (chuỗi DNA) và lý do nó chứa lịch sử. Giờ cần một cách *biểu diễn* lịch sử tách nhánh đó, và hiểu chính xác từng bộ phận của nó. Đó là cây phát sinh loài.

**Nguồn đã đối chiếu**

- Phần này là kiến thức nền được trình bày thống nhất trong các giáo trình sinh học phân tử và tiến hóa; không có số liệu cần trích dẫn.
- Các kiểu dữ liệu IQ-TREE hỗ trợ (DNA, protein, codon, nhị phân, hình thái): [IQ-TREE Beginner’s Tutorial](https://iqtree.github.io/doc/Tutorial).


---


## 2. Cây phát sinh loài

### Ý tưởng

Nếu mọi loài hôm nay đều đi xuống từ tổ tiên chung qua các lần tách nhánh, thì lịch sử ấy có hình một cái cây: gốc là tổ tiên xa nhất, mỗi chỗ rẽ nhánh là một lần tách, mỗi đầu ngọn là một loài (hoặc một chuỗi) mà ta quan sát được hôm nay. Sơ đồ đó gọi là **cây phát sinh loài** *(phylogenetic tree, gọi tắt là phylogeny)*.

Cần nói rõ ngay từ đầu: cây phát sinh loài là một **giả thuyết** được suy luận từ dữ liệu. Không ai quan sát trực tiếp được các lần tách nhánh trong quá khứ. Phần còn lại của bài học là câu chuyện về cách suy luận giả thuyết đó cho chặt chẽ, và cách đo xem nên tin nó đến đâu.

### Giải phẫu một cây

> 🖼️ **[Hình minh họa — chỉ xem được trong bản artifact HTML gốc]**
>
> *Mô tả hình:* Sơ đồ một cây phát sinh loài năm lá, chỉ ra gốc, nút trong, nhánh, lá, một clade và thước đo độ dài nhánh
>
> Cây có gốc với năm lá. Chiều ngang biểu diễn lượng thay đổi (độ dài nhánh, xem thước ở dưới); chiều dọc chỉ để trải các lá ra cho dễ nhìn và không mang ý nghĩa.

- **Lá *(leaf, tip)*** — Chuỗi hay loài mà ta thực sự có dữ liệu. Đây là phần duy nhất của cây được quan sát trực tiếp.
- **Nút trong *(internal node)*** — Tổ tiên chung giả định của các nhánh đi ra từ nó. Ta không có DNA của tổ tiên này; ta suy luận rằng nó đã tồn tại.
- **Nhánh *(branch)*** — Một dòng dõi nối tổ tiên với hậu duệ. Đột biến tích lũy dọc theo nhánh.
- **Độ dài nhánh *(branch length)*** — Trong cây dựng bằng maximum likelihood, đơn vị thường là *số thay thế kỳ vọng trên mỗi vị trí*. Nhánh dài 0,05 nghĩa là trung bình mỗi vị trí trải qua 0,05 thay thế; với một gen dài 1000 vị trí thì kỳ vọng khoảng 50 thay thế. Đây không phải số năm: muốn đổi sang thời gian cần một phân tích định tuổi riêng.
- **Gốc *(root)*** — Tổ tiên chung của tất cả các lá. Gốc cho cây một chiều thời gian: từ gốc đi ra là đi về phía hiện tại.
- **Clade** — Một tổ tiên cùng *toàn bộ* hậu duệ của nó. Trong hình, Loài 2 và Loài 3 cùng nút chung của chúng tạo thành một clade.
- **Topology** — “Hình dạng” phân nhóm của cây: ai được gom với ai, bỏ qua độ dài nhánh. Hai cây có cùng topology nhưng khác độ dài nhánh vẫn nói cùng một câu chuyện về quan hệ họ hàng.

### Hai lỗi đọc cây rất hay gặp

**Lỗi thứ nhất: nghĩ rằng xoay nhánh làm đổi cây.** Tại mỗi nút, bạn có thể xoay các nhánh con quanh nút như xoay một chiếc chuông gió; quan hệ họ hàng không đổi. Hai hình dưới đây là cùng một cây.

> 🖼️ **[Hình minh họa — chỉ xem được trong bản artifact HTML gốc]**
>
> *Mô tả hình:* Hai cách vẽ khác nhau của cùng một cây ba lá A, B, C
>
> Ở cả hai hình, A và B có tổ tiên chung gần nhất riêng, còn C tách ra sớm hơn. Thứ tự từ trên xuống dưới của các tên không mang thông tin.

**Lỗi thứ hai: nghĩ rằng hai tên đứng cạnh nhau là họ hàng gần nhất.** Họ hàng gần được xác định bằng *tổ tiên chung gần nhất*. Ở hình bên phải, C đứng ngay trên B, nhưng B gần A hơn nhiều vì B và A có chung một nút tổ tiên mà C không thuộc về.

### Cây có gốc và cây không gốc

Nhiều phương pháp, gồm cả maximum likelihood với các mô hình thông dụng, chỉ suy ra được **cây không gốc** *(unrooted tree)*: nó cho biết cấu trúc phân nhóm nhưng không cho biết đâu là tổ tiên xa nhất. Lý do toán học sẽ rõ ở phần 7 (nguyên lý ròng rọc). Tài liệu IQ-TREE nói rõ: chương trình luôn xuất cây không gốc, kể cả khi nó vẽ một loài ở vị trí trông như gốc, vì nó không biết gì về bối cảnh sinh học của dữ liệu.

Cách đặt gốc phổ biến nhất là dùng **nhóm ngoài** *(outgroup)*: đưa vào phân tích một loài mà ta biết chắc, từ các bằng chứng khác, là nằm ngoài nhóm đang nghiên cứu. Gốc được đặt trên nhánh nối tới nhóm ngoài. Các cách khác gồm đặt gốc ở điểm giữa đường dài nhất *(midpoint rooting)*, dùng đồng hồ phân tử, hoặc dùng mô hình không khả nghịch.

### Mỗi nhánh trong là một cách chia đôi các lá

Cắt một nhánh bất kỳ, cây vỡ thành hai mảnh, và tập lá bị chia thành hai nhóm. Mỗi cách chia như vậy gọi là một **split** *(bipartition)*, thường viết dạng `AB|CD`: A và B ở một phía, C và D ở phía kia. Một cây không gốc được xác định hoàn toàn bởi tập các split của nó. Hãy giữ khái niệm này: ở phần 9, độ tin cậy được đo *cho từng split*.

### Cây trong máy tính: định dạng Newick

Máy tính lưu cây bằng một dòng văn bản gọi là **định dạng Newick**. Dấu ngoặc gom nhóm, số sau dấu hai chấm là độ dài nhánh, dấu chấm phẩy kết thúc cây.

```bash
((A:0.1,B:0.2):0.05,C:0.3,D:0.25);
```

Đọc là: A và B tạo một nhóm (hai nhánh dài 0,1 và 0,2); nhóm đó nối vào nút trung tâm bằng nhánh dài 0,05; C và D cũng nối vào nút trung tâm. Ba nhánh cùng nối vào một nút ở cấp ngoài cùng là cách viết thường gặp của cây không gốc. File `.treefile` mà IQ-TREE xuất ra dùng đúng định dạng này; khi có giá trị hỗ trợ, IQ-TREE ghi chúng làm nhãn của nút, ngay sau dấu ngoặc đóng. Các phần mềm như FigTree hay iTOL đọc file này và vẽ thành hình.

#### 📎 Kiến thức bổ sung (cây gen và cây loài)

Cây dựng từ một gen là *cây gen*. Vì nhiều quá trình sinh học, các gen khác nhau trong cùng bộ gen có thể cho ra những cây khác nhau, và cây gen không nhất thiết trùng với *cây loài*. IQ-TREE 3 có công cụ “concordance factor” để đo mức đồng thuận giữa các gen và giữa các vị trí. Với đồ án một gen, bạn chỉ cần nhớ: kết luận của bạn là về lịch sử *của gen đó*.

#### 🚩 Trạm dừng sau phần 2

**Giờ ta đã biết**

- Lá là dữ liệu; nút trong là tổ tiên suy luận; độ dài nhánh đo lượng thay đổi.
- Đọc họ hàng qua tổ tiên chung gần nhất, không qua vị trí trên hình.
- Maximum likelihood cho cây không gốc; muốn có gốc thì dùng nhóm ngoài.
- Mỗi nhánh trong tương ứng một split; cây được lưu dạng Newick.

**Vì sao cần bước tiếp**

Để dựng cây từ DNA, ta phải so sánh các chuỗi *theo từng vị trí tương đồng*. Nhưng indel làm các vị trí bị xô lệch, và các chuỗi thậm chí dài ngắn khác nhau. Trước khi làm gì khác, phải xếp chúng thẳng hàng. Đó là MSA.

**Nguồn đã đối chiếu**

- IQ-TREE luôn xuất cây không gốc; file `.treefile` ở định dạng Newick, xem bằng FigTree hoặc iTOL: [IQ-TREE Beginner’s Tutorial](https://iqtree.github.io/doc/Tutorial).
- Các cách đặt gốc (nhóm ngoài, midpoint, đồng hồ phân tử, mô hình không khả nghịch) và lý do phần mềm thường cho cây không gốc: Bettisworth & Stamatakis, [RootDigger](https://publikationen.bibliothek.kit.edu/1000133165/114520687).
- Khái niệm split: Francis (2019), [Phylogenetic Networks](https://arxiv.org/pdf/1906.01586), arXiv:1906.01586.
- Concordance factor trong IQ-TREE 3: [trang chủ IQ-TREE](https://iqtree.github.io/); Wong et al. (2026), Mol. Biol. Evol.


---


## 3. Căn chỉnh đa trình tự (MSA)

### Vấn đề: indel làm lệch các vị trí

Giả sử hai chuỗi chỉ khác nhau đúng một sự kiện: chuỗi thứ hai bị mất chữ C ở vị trí 4. Nếu so thẳng từng chữ theo thứ tự, ta thấy gì?

So thẳng theo thứ tự chữ cái

```
S1     A T G C T A G T
S2     A T G T A G T
khớp?  ✓ ✓ ✓ ✗ ✗ ✗ ✗ ✗
```

Chèn một khoảng trống vào S2

```
S1     A T G C T A G T
S2     A T G - T A G T
khớp?  ✓ ✓ ✓ gap ✓ ✓ ✓ ✓
```

So thẳng cho năm vị trí “sai khác”, trong khi thực tế chỉ có một sự kiện mất chữ. Chèn một ký hiệu `-` vào đúng chỗ, mọi thứ khớp lại. Ký hiệu đó gọi là **gap** (khoảng trống): nó biểu diễn giả thuyết rằng tại cột này đã có một indel, tức là chuỗi này bị mất một chữ, hoặc các chuỗi khác đã được chèn thêm một chữ.

### MSA là gì

**Căn chỉnh đa trình tự** *(multiple sequence alignment, MSA)* là việc xếp ba chuỗi trở lên thành các hàng và chèn gap sao cho mỗi *cột* chứa những ký tự được giả định là tương đồng. Kết quả là một bảng: mỗi hàng là một chuỗi, mỗi cột là một vị trí tương đồng. Bảng này chính là đầu vào của IQ-TREE; tài liệu IQ-TREE nói rõ nếu bạn có chuỗi thô chưa căn chỉnh thì phải chạy một chương trình căn chỉnh như MAFFT trước.

> **⚠️ Cảnh báo:** **Điểm quan trọng nhất của phần này:** MSA là một *giả thuyết* về tương đồng, không phải sự thật quan sát được. Mọi lỗi trong MSA chảy thẳng vào cây, và không có bước nào phía sau tự động sửa được nó.

> 🔧 **Thử nghiệm: trước và sau khi căn chỉnh** *(khung tương tác — chỉ hoạt động trong bản artifact HTML gốc, ở đây là phần mô tả)*
>
> Năm chuỗi tự tạo, mỗi chuỗi khác S1 đúng một sự kiện. Bấm để chuyển giữa hai trạng thái.

### Máy tính căn chỉnh như thế nào

**Bước nền: căn chỉnh hai chuỗi.** Máy tính cho điểm mỗi cách căn chỉnh: cộng điểm khi hai chữ khớp, trừ điểm khi không khớp, trừ điểm khi mở gap. Với hai chuỗi, kỹ thuật *quy hoạch động* *(dynamic programming)* tìm được chính xác cách căn chỉnh điểm cao nhất: thuật toán Needleman–Wunsch (1970) cho căn chỉnh toàn chuỗi và Smith–Waterman (1981) cho căn chỉnh cục bộ.

**Vấn đề khi có nhiều chuỗi.** Mở rộng quy hoạch động lên $N$ chuỗi dài $L$ thì khối lượng tính toán tăng cỡ $L^N$. Wang và Jiang (1994) chứng minh rằng tìm MSA tối ưu theo điểm tổng các cặp *(sum-of-pairs)* là bài toán NP-complete. Nói đơn giản: không ai biết thuật toán nào vừa nhanh vừa đảm bảo tìm ra đáp án tốt nhất cho mọi bộ dữ liệu, và giới khoa học tin rằng thuật toán như vậy không tồn tại.

**Lời giải thực tế: căn chỉnh lũy tiến** *(progressive alignment)*, một phương pháp *heuristic*, tức là phương pháp tìm đáp án tốt trong thời gian hợp lý nhưng không đảm bảo tối ưu tuyệt đối:

1. Tính khoảng cách giữa từng cặp chuỗi.
2. Từ các khoảng cách, dựng một cây thô gọi là **cây dẫn đường** *(guide tree)*.
3. Căn chỉnh theo thứ tự của cây: các chuỗi gần nhau được căn trước, rồi ghép dần các nhóm xa hơn vào.

Nhược điểm: gap đã chèn ở bước sớm thì các bước sau không gỡ ra được, nên lỗi sớm lan xuống. Nhiều công cụ thêm bước *tinh chỉnh lặp* *(iterative refinement)* để sửa bớt các lỗi này.

> **💡 Lưu ý:** **Đừng nhầm:** cây dẫn đường chỉ là một cây thô để quyết định thứ tự căn chỉnh. Nó *không phải* cây phát sinh loài mà bạn sẽ báo cáo.

### Công cụ MSA phổ biến

| Công cụ | Ý chính | Ghi chú khi dùng | Trích dẫn |
|---|---|---|---|
| MAFFT | Nhiều chiến lược, từ rất nhanh (lũy tiến) đến chính xác (có tinh chỉnh lặp) | Tùy chọn `--auto` tự chọn giữa L-INS-i, FFT-NS-i và FFT-NS-2 theo kích thước dữ liệu. L-INS-i được tài liệu mô tả là có lẽ chính xác nhất, khuyên dùng cho dưới khoảng 200 chuỗi. | Katoh & Standley 2013 |
| MUSCLE | Ước lượng khoảng cách nhanh bằng đếm k-mer, căn chỉnh lũy tiến, rồi tinh chỉnh | Bài báo gốc so sánh với MAFFT, T-Coffee, ClustalW trên các bộ chuẩn và đạt độ chính xác cao nhất hoặc đồng hạng cao nhất. | Edgar 2004 |
| Clustal Omega | Căn chỉnh nhanh, mở rộng được cho số lượng lớn chuỗi protein | Có sẵn dạng web service ở nhiều nơi. | Sievers et al. 2011 |

```bash
# Để MAFFT tự chọn chiến lược theo kích thước dữ liệu
mafft --auto sequences.fasta > aligned.fasta

# L-INS-i: chậm hơn, chính xác hơn, phù hợp khi có dưới khoảng 200 chuỗi
mafft --localpair --maxiterate 1000 sequences.fasta > aligned.fasta
```

Hai định dạng file bạn sẽ gặp nhiều nhất là FASTA (mỗi chuỗi có một dòng tiêu đề bắt đầu bằng `>`) và PHYLIP (dòng đầu ghi số chuỗi và độ dài). IQ-TREE đọc được cả hai, cùng NEXUS, CLUSTAL và MSF.

### Kiểm tra và cắt tỉa alignment

**Luôn mở alignment ra xem bằng mắt**, ví dụ bằng AliView. Những dấu hiệu đáng ngờ: đầu và cuối lởm chởm vì các chuỗi dài ngắn khác nhau; những vùng gần như toàn gap; một chuỗi lệch hẳn khỏi các chuỗi khác (có thể nó bị đảo chiều, hoặc không thực sự tương đồng).

**Cắt tỉa** *(trimming)* là bỏ bớt các cột căn chỉnh kém tin cậy trước khi dựng cây. Công cụ phổ biến là trimAl; tùy chọn `-automated1` được các tác giả tối ưu cho việc dựng cây maximum likelihood.

```bash
trimal -in aligned.fasta -out trimmed.fasta -automated1
```

> **⚠️ Cảnh báo:** **Chỗ chưa có đồng thuận.** Bài báo trimAl (2009) báo cáo rằng cắt tỉa giúp cây tốt hơn trong các thử nghiệm của họ. Nhưng một đánh giá quy mô lớn sau đó (Tan et al., 2015, *Systematic Biology*) lại thấy các phương pháp lọc alignment tự động *thường làm cây một gen kém đi*. Vì vậy, với đồ án, cách làm thận trọng là: dựng cây cả với alignment gốc lẫn alignment đã cắt, so sánh, và báo cáo rõ bạn đã chọn gì và vì sao. IQ-TREE 3 cũng có chức năng cắt vị trí dựa trên likelihood nếu bạn muốn tìm hiểu thêm.

#### 🚩 Trạm dừng sau phần 3

**Giờ ta đã biết**

- MSA biến các chuỗi thành bảng: hàng là chuỗi, cột là vị trí tương đồng.
- Gap biểu diễn indel; MSA là giả thuyết, có thể sai.
- MSA tối ưu là bài toán NP-complete, nên dùng heuristic (lũy tiến, tinh chỉnh).
- Công cụ: MAFFT, MUSCLE, Clustal Omega; cắt tỉa là lựa chọn còn tranh luận.

**Vì sao cần bước tiếp**

Giờ ta đã có bảng dữ liệu sạch. Câu hỏi tự nhiên: sao không thử tất cả các cây có thể có, rồi chọn cây khớp dữ liệu nhất? Phần 4 cho thấy vì sao cách đó bất khả thi, và từ đó ta hiểu vì sao cần cả một tiêu chí chấm điểm lẫn một chiến lược tìm kiếm.

**Nguồn đã đối chiếu**

- IQ-TREE nhận MSA làm đầu vào; định dạng PHYLIP, FASTA, NEXUS, CLUSTAL, MSF: [IQ-TREE Beginner’s Tutorial](https://iqtree.github.io/doc/Tutorial).
- Wang L., Jiang T. (1994). On the complexity of multiple sequence alignment. *J. Comput. Biol.* [doi:10.1089/cmb.1994.1.337](https://doi.org/10.1089/cmb.1994.1.337).
- Tùy chọn MAFFT (`--auto`, L-INS-i): [trang hướng dẫn MAFFT](https://manpages.ubuntu.com/manpages/noble/man1/fftns.1.html); Katoh K., Standley D.M. (2013) *Mol. Biol. Evol.* 30:772–780.
- Edgar R.C. (2004) MUSCLE. *Nucleic Acids Res.* 32:1792–1797. Sievers F. et al. (2011) Clustal Omega. *Mol. Syst. Biol.* 7:539.
- Capella-Gutiérrez S. et al. (2009) trimAl. *Bioinformatics* 25:1972–1973, [PMC2712344](https://pmc.ncbi.nlm.nih.gov/articles/2712344). Tan G. et al. (2015) *Syst. Biol.* 64:778–791, [doi:10.1093/sysbio/syv033](https://doi.org/10.1093/sysbio/syv033).
- Needleman–Wunsch (1970), Smith–Waterman (1981) và độ phức tạp $O(L^N)$: tổng quan tại [IGI Global (bài tổng quan MSA)](https://new.igi-global.com/article/using-a-bio-inspired-algorithm-to-resolve-the-multiple-sequence-alignment-problem/160742). Căn chỉnh lũy tiến và vai trò cây dẫn đường: [Zhan et al. (2015), BMC Bioinformatics](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4402577/).


---


## 4. Bùng nổ số lượng cây

Ý tưởng đơn giản nhất để dựng cây: liệt kê mọi cây có thể có cho các loài của ta, chấm điểm từng cây, chọn cây điểm cao nhất. Để biết cách này có khả thi không, ta cần đếm xem có bao nhiêu cây.

### Đếm số cây bằng cách thêm lá từng cái một

Với 3 loài chỉ có đúng 1 cây không gốc (một nút ở giữa, ba nhánh tỏa ra). Cây đó có 3 nhánh. Muốn thêm loài thứ 4, ta gắn nó vào giữa một trong 3 nhánh đó, nên có 3 cây 4 lá. Mỗi cây 4 lá có 5 nhánh, nên loài thứ 5 có 5 chỗ để gắn: $3 \times 5 = 15$ cây. Cứ thế, mỗi lần thêm một lá, số nhánh để gắn tăng thêm 2. Kết quả là công thức:

$$ \text{Số cây nhị phân không gốc với } n \text{ lá} = (2n-5)!! = (2n-5)\times(2n-7)\times\cdots\times 3\times 1 $$

Ký hiệu $!!$ gọi là *giai thừa kép*: nhân các số giảm dần mỗi lần 2 đơn vị. Nếu cần cây có gốc, con số là $(2n-3)!!$, vì mỗi cây không gốc có $2n-3$ nhánh, và gốc có thể đặt trên bất kỳ nhánh nào.

> 🖼️ **[Hình minh họa — chỉ xem được trong bản artifact HTML gốc]**
>
> *Mô tả hình:* Ba cây không gốc khả dĩ cho bốn loài A, B, C, D
>
> Với bốn loài có đúng ba cây không gốc. Mỗi cây được đặt tên theo split của nhánh ở giữa. Ba cây này sẽ xuất hiện lại ở phần 8 (tìm kiếm) và phần 9 (bootstrap).

| Số loài $n$ | Số cây không gốc $(2n-5)!!$ | Số cây có gốc $(2n-3)!!$ |
|---|---|---|
| 4 | 3 | 15 |
| 5 | 15 | 105 |
| 10 | 2.027.025 | 34.459.425 |
| 20 | khoảng $2{,}2\times10^{20}$ | khoảng $8{,}2\times10^{21}$ |
| 50 | khoảng $2{,}8\times10^{74}$ | khoảng $2{,}8\times10^{76}$ |

> 🔧 **Thử nghiệm: bạn có bao nhiêu loài?** *(khung tương tác — chỉ hoạt động trong bản artifact HTML gốc, ở đây là phần mô tả)*
>
> Nhập số loài trong đồ án của bạn (từ 3 đến 1000). Máy tính đếm chính xác bằng số nguyên lớn.

### Hệ quả: không thể thử hết, phải tìm kiếm thông minh

Chỉ với 20 loài, số cây đã vượt xa khả năng thử hết của mọi máy tính. Hơn nữa, bài toán tìm cây maximum likelihood đã được chứng minh là NP-hard (Chor & Tuller; Roch, các chứng minh độc lập công bố khoảng 2005–2006). Vì vậy mọi phần mềm maximum likelihood thực tế, kể cả IQ-TREE, đều dùng heuristic: tìm kiếm có chiến lược, cho kết quả rất tốt trong thực hành nhưng không đảm bảo tuyệt đối tìm ra cây tốt nhất.

Để tìm kiếm, ta cần đúng hai thứ, và phần còn lại của bài lần lượt xây dựng chúng:

- **Một cách chấm điểm một cây cụ thể**: maximum likelihood (phần 5), cần một mô hình tiến hóa (phần 6) và một thuật toán tính toán hiệu quả (phần 7).
- **Một chiến lược di chuyển giữa các cây** để tìm cây điểm cao: phần 8.

#### 🚩 Trạm dừng sau phần 4

**Giờ ta đã biết**

- Số cây không gốc với $n$ loài là $(2n-5)!!$, tăng nhanh khủng khiếp.
- Tìm cây ML là NP-hard, nên phần mềm dùng heuristic.
- Cần hai thành phần: hàm chấm điểm và chiến lược tìm kiếm.

**Vì sao cần bước tiếp**

Trước khi tìm kiếm, phải định nghĩa “cây tốt” nghĩa là gì. Maximum likelihood trả lời câu đó. Ta sẽ hiểu nó qua một ví dụ không dính gì đến sinh học: tung đồng xu.

**Nguồn đã đối chiếu**

- Công thức $(2n-5)!!$ và $(2n-3)!!$: [MathWorld, Phylogenetic Tree](https://mathworld.wolfram.com/PhylogeneticTree.html) (theo Semple & Steel 2003); bảng số cây: [UConn MCB](https://carrot.mcb.uconn.edu/mcb396_41/tree_number.html); lập luận thêm lá từng cái: [HITS, bài giảng 6](https://cme.h-its.org/exelixis/web/teaching/lectures23_24/lecture6.pdf). Các số trong bảng đã được tính lại bằng máy.
- Tìm cây ML là NP-hard: Chor B., Tuller T., [Finding the Maximum Likelihood Tree is Hard](https://courses.cs.tau.ac.il/bdida/06a/tamirtul/ML.pdf); Roch S., [A Short Proof that Phylogenetic Tree Reconstruction by Maximum Likelihood is Hard](https://web3.arxiv.org/pdf/math/0504378), arXiv:math/0504378.


---


## 5. Maximum likelihood

### Hai câu hỏi ngược chiều nhau

|   | Xác suất *(probability)* | Likelihood |
|---|---|---|
| Điều đã biết | Tham số: đồng xu cân đối, xác suất ra ngửa $p = 0{,}5$ | Dữ liệu: đã tung 10 lần, thấy 7 lần ngửa |
| Điều đang hỏi | Khả năng tung 10 lần ra đúng 7 ngửa là bao nhiêu? | Với từng giá trị $p$, kết quả đã thấy dễ xảy ra đến mức nào? |
| Thứ thay đổi | Dữ liệu | Tham số |

Hai câu hỏi dùng chung một biểu thức toán học, chỉ đọc theo hai chiều khác nhau. Với dữ liệu $D$ và tham số $\theta$:

$$ L(\theta \mid D) = P(D \mid \theta) $$

**Likelihood** của một giá trị tham số là xác suất mà giá trị đó gán cho dữ liệu ta đã thực sự quan sát. **Maximum likelihood** (hợp lý cực đại) là nguyên tắc: **chọn giá trị tham số làm cho dữ liệu đã quan sát có xác suất cao nhất**. Giá trị được chọn gọi là ước lượng hợp lý cực đại, ký hiệu có dấu mũ, ví dụ $\hat p$.

> **💡 Lưu ý:** **Một cái bẫy về ngôn từ:** likelihood *không phải* xác suất của tham số. Cộng likelihood qua mọi giá trị của $p$ không cho ra 1. Vì vậy không được nói “cây này có xác suất đúng là …” chỉ dựa vào likelihood. Muốn nói về xác suất của một giả thuyết, cần khung thống kê Bayes, một hướng tiếp cận khác không nằm trong bài này.

### Ví dụ đồng xu, làm đến cùng

Mỗi lần tung độc lập, ngửa với xác suất $p$. Quan sát 7 ngửa, 3 sấp. Likelihood là:

$$ L(p) = p^{7}\,(1-p)^{3} $$

(Công thức đầy đủ có thêm hệ số $\binom{10}{7}$, nhưng hệ số này không phụ thuộc $p$ nên không làm dịch chuyển vị trí đỉnh; ta bỏ nó đi.) Lấy logarit, đạo hàm, cho bằng 0:

$$ \ln L(p) = 7\ln p + 3\ln(1-p), \qquad \frac{d\,\ln L}{dp} = \frac{7}{p} - \frac{3}{1-p} = 0 \;\;\Longrightarrow\;\; \hat p = \frac{7}{10} = 0{,}7 $$

So sánh hai giả thuyết: $L(0{,}7) \approx 2{,}22\times10^{-3}$ còn $L(0{,}5)\approx 9{,}77\times10^{-4}$. Giả thuyết $p = 0{,}7$ làm dữ liệu dễ xảy ra hơn khoảng 2,3 lần so với giả thuyết đồng xu cân đối.

> 🔧 **Thử nghiệm: đường cong likelihood** *(khung tương tác — chỉ hoạt động trong bản artifact HTML gốc, ở đây là phần mô tả)*
>
> Tung 10 lần. Chọn số lần ngửa đã quan sát, rồi kéo $p$ để xem likelihood thay đổi. Đỉnh luôn nằm ở $k/10$.

### Vì sao phần mềm luôn dùng log-likelihood

Ở phần 7 bạn sẽ thấy likelihood của cả một alignment là *tích* của likelihood từng cột. Nhân hàng nghìn số nhỏ hơn 1 với nhau cho ra một số nhỏ đến mức máy tính làm tròn thành 0 (hiện tượng *underflow*). Lấy logarit giải quyết việc này, vì logarit biến tích thành tổng: $\ln(ab) = \ln a + \ln b$. Logarit cũng là hàm đồng biến, nên đỉnh của $\ln L$ nằm đúng chỗ đỉnh của $L$.

Vì vậy IQ-TREE báo cáo **LogL**, luôn là số âm. Ví dụ một lần chạy mẫu với 17 chuỗi dài 1998 vị trí cho LogL cỡ −21149. Quy tắc đọc: **LogL càng gần 0 càng tốt**, và **chỉ so sánh LogL giữa các cây hay mô hình trên cùng một alignment**. Đem LogL của hai bộ dữ liệu khác nhau ra so là vô nghĩa.

### Từ đồng xu sang cây phát sinh loài

|   | Đồng xu | Cây phát sinh loài |
|---|---|---|
| Dữ liệu $D$ | Dãy kết quả ngửa, sấp | Bảng MSA |
| Tham số | $p$ | Topology $T$, các độ dài nhánh $\mathbf{b}$, tham số của mô hình $\theta$ |
| Mô hình | Các lần tung độc lập, xác suất ngửa $p$ | Mô hình thay thế nucleotide (phần 6) |
| Cách tìm đỉnh | Đạo hàm, giải trực tiếp | Tối ưu số cho độ dài nhánh và tham số (phần 7), tìm kiếm heuristic cho topology (phần 8) |

$$ L(T, \mathbf{b}, \theta \mid D) = P(D \mid T, \mathbf{b}, \theta) $$

**Cây maximum likelihood** là bộ $(T, \mathbf{b}, \theta)$ làm cho biểu thức này lớn nhất. Nhưng để tính được vế phải, ta phải trả lời một câu hỏi rất cụ thể: *một chữ A ở tổ tiên, sau khi đi dọc một nhánh dài 0,1, có xác suất bao nhiêu để trở thành chữ G?* Câu trả lời đến từ mô hình thay thế.

> **Ví von:** Một thám tử có nhiều kịch bản về vụ án. Anh chọn kịch bản mà nếu nó đúng thì những manh mối đang có trong tay là dễ xảy ra nhất.
>
> **Cơ chế thật:** Maximum likelihood so sánh các giả thuyết qua $P(D \mid \text{giả thuyết})$, *dưới một mô hình xác suất cụ thể*. Kết quả chỉ đáng tin tới mức mô hình phản ánh đúng quá trình tiến hóa thật. Mô hình sai có thể dẫn tới một cây sai nhưng trông rất chắc chắn.

#### 📎 Kiến thức bổ sung (maximum likelihood không phải cách duy nhất)

| Họ phương pháp | Ý tưởng | Ghi chú |
|---|---|---|
| Khoảng cách | Tính khoảng cách từng cặp chuỗi, ghép dần các cặp gần nhất | Ví dụ neighbor-joining (Saitou & Nei 1987); rất nhanh |
| Parsimony | Chọn cây cần ít lần thay đổi nhất để giải thích dữ liệu | Tìm cây parsimony tối ưu là NP-complete (Graham & Foulds 1982). Felsenstein (1978) chỉ ra parsimony có thể sai một cách nhất quán trong một số tình huống, gọi là hiện tượng hút nhánh dài |
| Maximum likelihood | Chọn cây làm dữ liệu có xác suất cao nhất dưới một mô hình | IQ-TREE, RAxML, PhyML |
| Bayes | Tính xác suất hậu nghiệm của các cây, thường bằng lấy mẫu MCMC | Cho phép nói về “xác suất của cây” theo nghĩa Bayes |

#### 🚩 Trạm dừng sau phần 5

**Giờ ta đã biết**

- Likelihood cố định dữ liệu, so sánh các giá trị tham số.
- Maximum likelihood chọn tham số làm dữ liệu có xác suất cao nhất.
- Phần mềm dùng log-likelihood; càng gần 0 càng tốt, chỉ so trên cùng alignment.
- Với cây, tham số gồm topology, độ dài nhánh và tham số mô hình.

**Vì sao cần bước tiếp**

Công thức $P(D \mid T,\mathbf{b},\theta)$ chưa tính được nếu ta không có quy tắc xác suất cho việc một chữ biến thành chữ khác dọc một nhánh. Quy tắc đó là mô hình thay thế.

**Nguồn đã đối chiếu**

- Định nghĩa likelihood của cây là xác suất quan sát dữ liệu khi biết cây: [Felsenstein’s tree-pruning algorithm](https://en.wikipedia.org/wiki/Felsenstein%27s_tree-pruning_algorithm). Các giá trị $L(0{,}7)$, $L(0{,}5)$ đã được tính lại bằng máy.
- Ví dụ LogL của một lần chạy 17 chuỗi, 1998 vị trí: [báo cáo IQ-TREE trên Galaxy Europe](https://usegalaxy.eu/api/histories/5a48e61995d3a0c5/contents/4838ba20a6d86765ca0031abffa950d8/display).
- Parsimony là NP-complete (Graham & Foulds 1982) và các so sánh với ML: Chor & Tuller (xem phần 4). Felsenstein (1978) và hiện tượng hút nhánh dài: [bài tổng quan về di sản của Felsenstein 1981](https://pmc.ncbi.nlm.nih.gov/articles/PMC7803665). Neighbor-joining (Saitou & Nei 1987) và MCMC trong phát sinh loài Bayes: xem các bài tổng quan đã dẫn ở phần 3 và [A Variational Approach to Bayesian Phylogenetic Inference, arXiv:2204.07747](https://arxiv.org/pdf/2204.07747). IQ-TREE so sánh với RAxML và PhyML: [trang chủ IQ-TREE](https://iqtree.github.io/).


---


## 6. Mô hình thay thế nucleotide

### Mô hình để làm gì

**Mô hình thay thế** *(substitution model)* là một bộ quy tắc xác suất trả lời đúng câu hỏi cuối phần 5: tại một vị trí, sau một nhánh dài $t$, chữ $x$ trở thành chữ $y$ với xác suất bao nhiêu. Ta ký hiệu xác suất đó là $P_{xy}(t)$.

Các mô hình dùng trong maximum likelihood thường dựa trên vài giả định đơn giản hóa:

- **Các vị trí tiến hóa độc lập với nhau.** Nhờ vậy likelihood của cả alignment bằng tích likelihood của từng cột.
- **Tính Markov.** Khả năng chữ tiếp theo là gì chỉ phụ thuộc chữ hiện tại, không phụ thuộc chuỗi thay đổi trước đó.
- **Dừng và khả nghịch theo thời gian.** Tỷ lệ bốn chữ ổn định theo thời gian, và quá trình nhìn xuôi hay nhìn ngược thời gian có thống kê như nhau.

Đây là những đơn giản hóa có chủ đích để việc tính toán khả thi. Ngay cả giả định độc lập giữa các vị trí, được Felsenstein dùng từ năm 1981 và vẫn phổ biến đến nay, cũng được thừa nhận là không thực tế về mặt sinh học. Mô hình là một phép xấp xỉ hữu ích, không phải bản mô tả chính xác của tự nhiên.

### Mô hình đơn giản nhất: Jukes–Cantor (JC69)

JC69 giả định mọi kiểu thay thế có tốc độ như nhau và bốn chữ có tần suất bằng nhau (mỗi chữ $1/4$). Ngoài độ dài nhánh, nó không có tham số tự do nào. Với $t$ là độ dài nhánh tính bằng số thay thế kỳ vọng trên mỗi vị trí:

$$ P_{\text{giữ nguyên}}(t) = \frac14 + \frac34\,e^{-4t/3} \qquad\qquad P_{\text{thành một chữ cụ thể khác}}(t) = \frac14 - \frac14\,e^{-4t/3} $$

Đừng học thuộc, hãy tự kiểm tra ba điều để thấy công thức hợp lý:

- Khi $t = 0$: xác suất giữ nguyên bằng 1, xác suất thành chữ khác bằng 0. Nhánh dài 0 thì chưa có gì thay đổi.
- Khi $t$ rất lớn: cả bốn xác suất tiến về $1/4$. Sau thời gian vô cùng dài, chữ ban đầu không còn để lại dấu vết gì.
- Tổng luôn bằng 1: $P_{\text{giữ nguyên}} + 3\,P_{\text{thành chữ khác}} = 1$.

Ví dụ với $t = 0{,}1$: xác suất giữ nguyên khoảng 0,906; xác suất thành mỗi chữ khác khoảng 0,031.

### Bão hòa: vì sao đếm số khác biệt là chưa đủ

Một vị trí có thể thay đổi nhiều lần: A thành G rồi G lại thành A, hoặc A thành C rồi thành T. Khi so hai chuỗi hôm nay, ta chỉ thấy *kết quả cuối cùng*, nên những lần thay đổi chồng lên nhau bị che mất. Theo JC69, tỷ lệ vị trí khác nhau mà ta quan sát được là:

$$ p(t) = \frac34\left(1 - e^{-4t/3}\right) \qquad\Longleftrightarrow\qquad t = -\frac34\,\ln\!\left(1 - \frac43\,p\right) $$

Công thức bên phải gọi là *khoảng cách Jukes–Cantor*: nó “sửa” tỷ lệ khác biệt đếm được thành số thay thế thực sự đã xảy ra. Ví dụ đếm thấy 30% vị trí khác nhau ($p = 0{,}30$) thì ước lượng đã có khoảng 0,383 thay thế trên mỗi vị trí. Khi $p$ tiến gần 0,75, $t$ tiến ra vô cùng: chuỗi đã bị xáo trộn gần như ngẫu nhiên và thông tin lịch sử gần như mất hết. Hiện tượng này gọi là **bão hòa** *(saturation)*. Hệ quả thực hành: gen thay đổi quá nhanh không phù hợp để suy luận các lần tách nhánh rất cổ.

> 🔧 **Thử nghiệm: đường bão hòa của JC69** *(khung tương tác — chỉ hoạt động trong bản artifact HTML gốc, ở đây là phần mô tả)*
>
> Kéo độ dài nhánh $t$. Đường màu đỏ là tỷ lệ khác biệt ta *quan sát* được; đường nét đứt là số thay thế *thực sự* đã xảy ra.

### Thực tế hơn: transition và transversion

Bốn nucleotide chia làm hai nhóm theo cấu trúc hóa học: **purine** (A và G, cấu trúc hai vòng) và **pyrimidine** (C và T, cấu trúc một vòng). Thay thế trong cùng một nhóm (A↔G, C↔T) gọi là **transition**; thay thế giữa hai nhóm gọi là **transversion**. Trong dữ liệu thật, transition thường xảy ra nhiều hơn transversion, nên các mô hình phức tạp hơn JC69 cho hai loại này tốc độ riêng. Các mô hình phổ biến tạo thành một “thang” từ đơn giản đến phức tạp:

| Mô hình | Tốc độ thay thế | Tần suất bốn chữ |
|---|---|---|
| JC69 (Jukes & Cantor 1969) | Tất cả bằng nhau | Bằng nhau |
| F81 (Felsenstein 1981) | Tất cả bằng nhau | Khác nhau, ước lượng từ dữ liệu |
| K80, còn gọi K2P (Kimura 1980) | Hai tốc độ: transition và transversion | Bằng nhau |
| HKY85 (Hasegawa và cộng sự 1985) | Hai tốc độ: transition và transversion | Khác nhau |
| SYM | Sáu tốc độ riêng cho sáu cặp chữ | Bằng nhau |
| GTR (General Time Reversible) | Sáu tốc độ riêng cho sáu cặp chữ | Khác nhau |

Vì sao GTR chỉ có 6 tham số tốc độ, chứ không phải 12 cho 12 hướng thay đổi? Vì giả định khả nghịch: mỗi cặp chữ (ví dụ A và G) dùng chung một tham số cho cả hai chiều. Tài liệu IQ-TREE còn mô tả mô hình bằng mã sáu chữ số cho sáu cặp A-C, A-G, A-T, C-G, C-T, G-T: mã `010010` nghĩa là A-G và C-T có chung một tốc độ (transition), bốn cặp còn lại chung một tốc độ khác, tức K80 hoặc HKY; mã `012345` là sáu tốc độ riêng, tức SYM hoặc GTR.

### Các vị trí tiến hóa với tốc độ khác nhau

Không phải vị trí nào trong gen cũng thay đổi nhanh như nhau. Vị trí quan trọng cho chức năng thường hiếm khi thay đổi, vì thay đổi ở đó hay gây hại; vị trí ít bị ràng buộc thì thay đổi tự do hơn. Mô hình xử lý điều này bằng các “phần gắn thêm” vào tên mô hình:

| Phần gắn | Ý nghĩa |
|---|---|
| `+I` | Một tỷ lệ vị trí không bao giờ thay đổi (invariable sites). |
| `+G`, thường `+G4` | Tốc độ của các vị trí phân bố theo phân phối gamma, được chia thành một số nhóm tốc độ, thường là 4 (Yang 1994). Tham số hình dạng $\alpha$ nhỏ nghĩa là tốc độ giữa các vị trí rất chênh lệch; $\alpha$ lớn nghĩa là gần như đồng đều. |
| `+R`, ví dụ `+R3` | Mô hình FreeRate: các nhóm tốc độ và tỷ trọng của chúng được ước lượng tự do, không ép theo hình dạng gamma (Soubrier và cộng sự 2012). ModelFinder đưa loại này vào danh sách xét chọn. |
| `+F` | Tần suất bốn chữ lấy theo tần suất quan sát được trong dữ liệu. |

Đọc thử tên mô hình: `GTR+F+I+G4` là GTR, tần suất lấy từ dữ liệu, có tỷ lệ vị trí bất biến, và tốc độ theo gamma chia 4 nhóm. `TIM2+I+G4`, mô hình mà ModelFinder chọn cho file ví dụ trong hướng dẫn IQ-TREE, dùng TIM2, một mô hình trong họ GTR có số tham số tốc độ nhiều hơn HKY nhưng ít hơn GTR.

#### 📎 Kiến thức bổ sung (một tranh luận gần đây về +G)

Ferretti và cộng sự (2024) báo cáo rằng mô hình gamma rời rạc có thể ước lượng độ dài nhánh dài hơn thực tế, và đề nghị thay bằng FreeRate. Họ cũng nêu FreeRate khó ước lượng hơn. Đây là vấn đề còn đang được thảo luận; với đồ án, bạn chỉ cần biết rằng ModelFinder xét cả +G lẫn +R và để tiêu chí thông tin quyết định.

### Chọn mô hình: vì sao không cứ lấy mô hình phức tạp nhất

Thêm tham số thì LogL không bao giờ giảm, giống như một đường cong được phép uốn nhiều hơn thì càng khớp các điểm dữ liệu. Nhưng khớp quá mức *(overfitting)* nghĩa là mô hình học cả nhiễu ngẫu nhiên. *Tiêu chí thông tin* cân bằng hai điều này bằng cách phạt số tham số:

$$ \mathrm{AIC} = -2\ln L + 2k \qquad\qquad \mathrm{BIC} = -2\ln L + k\ln n $$

Trong đó $k$ là số tham số tự do và $n$ là kích thước mẫu (với alignment, thường là số vị trí). Mô hình có tiêu chí **nhỏ nhất** được chọn. Khi $n$ lớn, BIC phạt thêm tham số nặng hơn AIC, nên thường chọn mô hình gọn hơn.

**ModelFinder** là thành phần chọn mô hình của IQ-TREE. Theo tài liệu, nó tính LogL của rất nhiều mô hình trên một cây parsimony ban đầu, tính AIC, AICc và BIC, rồi mặc định chọn mô hình có BIC nhỏ nhất; bạn đổi sang AIC hoặc AICc bằng tùy chọn `-AIC` hoặc `-AICc`. Từ phiên bản 1.5.4, ModelFinder chạy mặc định. Trang chủ IQ-TREE cho biết ModelFinder nhanh hơn jModelTest và ProtTest từ 10 đến 100 lần.

#### 📎 Kiến thức bổ sung (mô hình cho protein)

Với chuỗi protein, mô hình là các ma trận thay thế 20×20 đã được ước lượng sẵn từ các bộ dữ liệu lớn, ví dụ LG hay WAG. Tùy chọn `-mset WAG,LG` giới hạn ModelFinder chỉ xét các mô hình thuộc những họ này để tiết kiệm thời gian.

#### 🚩 Trạm dừng sau phần 6

**Giờ ta đã biết**

- Mô hình cho ta $P_{xy}(t)$; JC69 là mô hình đơn giản nhất.
- Thay đổi chồng lên nhau gây bão hòa; khoảng cách JC sửa lỗi đếm thiếu.
- Thang mô hình JC69 → GTR; phần gắn +I, +G, +R, +F xử lý tốc độ khác nhau giữa các vị trí.
- ModelFinder chọn mô hình theo BIC (mặc định).

**Vì sao cần bước tiếp**

Giờ ta có công thức cho *một nhánh*. Nhưng cây có nhiều nhánh, và chữ ở các tổ tiên thì không biết. Phần 7 ghép các xác suất nhánh lại thành likelihood của cả cây, bằng tay trên một ví dụ nhỏ trước, rồi bằng thuật toán của Felsenstein.

**Nguồn đã đối chiếu**

- Công thức xác suất chuyển của JC69 và khoảng cách Jukes–Cantor: [Rice COMP571, GTR and nested models](https://cs.rice.edu/~ogilvie/comp571/gtr-models/) (theo Phylogenetic Handbook); đơn vị độ dài nhánh: [Models of DNA evolution](https://en.wikipedia.org/wiki/Models_of_DNA_evolution). Các giá trị số đã được tính lại bằng máy.
- Giả định độc lập giữa các vị trí được thừa nhận là không thực tế: [bài tổng quan về Felsenstein (1981)](https://pmc.ncbi.nlm.nih.gov/articles/PMC7803665).
- Thang mô hình JC69, F81, K80, HKY, SYM, GTR và +I, +G: [evomics.org](https://evomics.org/resources/substitution-models/nucleotide-substitution-models/); [Abadi et al. (2019), Nat. Commun.](https://www.nature.com/articles/s41467-019-08822-w). Mã sáu chữ số: [IQ-TREE Substitution Models](https://iqtree.github.io/doc/Substitution-Models).
- Gamma rời rạc (Yang 1994), số nhóm mặc định thường là 4: [FSU BSC5936, Lecture 8](https://people.sc.fsu.edu/~pbeerli/BSC-5936/09-26-05/Lecture8.pdf); ý nghĩa tham số $\alpha$: [PHASE manual](https://www.bioinf.manchester.ac.uk/resources/phase/manual/node81.html). FreeRate (Soubrier et al. 2012) và tranh luận về +G: [BEAST2 blog (2024)](https://www.beast2.org/2024/09/01/use-free-rates-or-not.html); [Ferretti et al. (2024)](https://www.ndm.ox.ac.uk/publications/publication_modal/2422385).
- ModelFinder: cách hoạt động, BIC mặc định, `-AIC`/`-AICc`, mặc định từ 1.5.4, `-mset`, ví dụ TIM2+I+G4: [IQ-TREE Beginner’s Tutorial](https://iqtree.github.io/doc/Tutorial); Kalyaanamoorthy S. et al. (2017) *Nat. Methods* 14:587–589.


---


## 7. Tính likelihood trên một cây

### Từng cột một, rồi nhân lại

Nhờ giả định các vị trí độc lập, ta tính likelihood cho từng cột của alignment rồi nhân lại. Với $N$ cột:

$$ L = \prod_{i=1}^{N} L_i \qquad\Longleftrightarrow\qquad \ln L = \sum_{i=1}^{N} \ln L_i $$

Tại một cột, ta biết chữ ở các lá, nhưng *không biết* chữ ở các nút trong, vì đó là tổ tiên. Khi không biết, xác suất cho ta một cách xử lý chuẩn: cộng qua mọi khả năng, mỗi khả năng nhân với xác suất của nó. Với cây ba lá có một nút ở giữa mang chữ $x$ chưa biết:

$$ L_{\text{cột}} = \sum_{x \in \{A,C,G,T\}} \pi_x \; P_{x s_1}(t_1)\; P_{x s_2}(t_2)\; P_{x s_3}(t_3) $$

Ở đây $s_1, s_2, s_3$ là chữ quan sát được ở ba lá, $t_1, t_2, t_3$ là độ dài ba nhánh, và $\pi_x$ là tần suất nền của chữ $x$ (với JC69 bằng $1/4$).

### Khoan: cây không gốc thì đặt tổ tiên ở đâu?

Công thức trên coi nút ở giữa như gốc. Felsenstein (1981) chỉ ra rằng với mô hình khả nghịch và dừng, đặt gốc ở bất kỳ đâu trên cây cũng cho cùng một likelihood. Kết quả này gọi là **nguyên lý ròng rọc** *(pulley principle)*. Nhờ vậy ta được phép chọn gốc tùy ý để tính toán. Và cũng chính vì vậy, maximum likelihood với các mô hình này không thể tự tìm ra gốc thật: mọi vị trí gốc đều cho cùng một điểm số. Đây là lời giải thích đã hẹn ở phần 2.

> 🔧 **Thử nghiệm: tính likelihood của một cột bằng tay** *(khung tương tác — chỉ hoạt động trong bản artifact HTML gốc, ở đây là phần mô tả)*
>
> Cây ba lá, mô hình JC69. Chọn chữ ở mỗi lá và độ dài mỗi nhánh. Bảng liệt kê bốn khả năng cho chữ ở tổ tiên; likelihood của cột là tổng cột cuối.

**Kết quả với giá trị mặc định** (S1 = A, S2 = A, S3 = G; nhánh 0,1; 0,1; 0,3): likelihood của cột là khoảng 0,01715, tức $\ln L \approx -4{,}066$. Khả năng “tổ tiên mang A” đóng góp khoảng 98,7% tổng này. Diễn giải: cách giải thích hợp lý nhất là tổ tiên mang A, và một thay thế A→G đã xảy ra trên nhánh dài dẫn tới S3.

Ba thí nghiệm nên tự làm với khung trên:

1. **Rút ngắn t₃ xuống 0,05.** Likelihood giảm còn khoảng 0,00355. Một thay đổi trên nhánh ngắn thì khó xảy ra hơn.
2. **Kéo dài t₃ lên 2.** Likelihood lại *tăng*, lên khoảng 0,048. Với một cột duy nhất có khác biệt, nhánh càng dài càng dễ giải thích sự khác biệt. Trong alignment thật, hàng trăm cột khác, nơi S3 giống S1 và S2, sẽ “kéo” nhánh đó ngắn lại. Độ dài nhánh tối ưu là điểm cân bằng giữa hai lực này, và IQ-TREE tìm điểm đó bằng tối ưu số.
3. **Đổi S3 thành A** với các nhánh mặc định. Likelihood tăng lên khoảng 0,155, vì một cột không có thay đổi nào là rất dễ xảy ra.

### Cây lớn hơn: thuật toán cắt tỉa của Felsenstein

Với cây nhiều lá, cách “liệt kê mọi khả năng” sụp đổ. Một cây có gốc với $n$ lá có $n-1$ nút trong, mỗi nút mang một trong bốn chữ, nên có $4^{n-1}$ tổ hợp phải cộng, cho *mỗi* cột. Chỉ với 12 chuỗi đã là $4^{11} = 4.194.304$ tổ hợp.

**Thuật toán cắt tỉa** *(pruning algorithm, Felsenstein 1981)* tránh việc liệt kê bằng cách đi từ các lá lên gốc. Tại mỗi nút $v$, nó lưu một vector bốn số $L_v(x)$: xác suất của phần dữ liệu nằm dưới nút $v$, *nếu* nút $v$ mang chữ $x$. Với nút có hai con $c_1, c_2$ nối bằng nhánh dài $t_1, t_2$:

$$ L_v(x) = \Big[\sum_{y} P_{xy}(t_1)\, L_{c_1}(y)\Big]\times\Big[\sum_{z} P_{xz}(t_2)\, L_{c_2}(z)\Big] $$

Tại lá: $L_{\text{lá}}(x) = 1$ nếu $x$ đúng là chữ quan sát được, ngược lại bằng 0. Tại gốc: $L_{\text{cột}} = \sum_x \pi_x\, L_{\text{gốc}}(x)$. Mỗi nút chỉ tính một lần, nên chi phí tăng tuyến tính theo số lá thay vì theo lũy thừa. Ở các lĩnh vực khác, cùng ý tưởng này được gọi là thuật toán sum-product hay message passing.

> **Ví von:** Đếm dân số một tỉnh: không ai đi gõ cửa từng nhà rồi cộng mọi khả năng. Mỗi xã tổng hợp số của mình, gửi lên huyện; huyện tổng hợp gửi lên tỉnh. Mỗi cấp chỉ làm việc với con số đã tóm tắt từ cấp dưới.
>
> **Cơ chế thật:** Vector $L_v(x)$ tóm tắt toàn bộ thông tin mà cây con dưới $v$ cần truyền lên trên. Công thức ở nút cha chỉ dùng hai vector của hai con và hai ma trận xác suất nhánh, không cần biết chi tiết bên dưới.

**Gap được xử lý thế nào?** Theo tài liệu IQ-TREE, gap (`-`) và ký tự thiếu (`?`, hoặc `N` với DNA) được coi là ký tự chưa biết, không mang thông tin: likelihood của cột bằng likelihood trên cây con chỉ gồm các chuỗi không có gap ở cột đó. Trong thuật toán cắt tỉa, cách làm tương ứng là gán vector $(1,1,1,1)$ cho lá đó. RAxML và PhyML cũng xử lý theo cách này.

### Rồi tối ưu: từ “tính điểm” thành “điểm tốt nhất của một topology”

Với một topology cố định, IQ-TREE điều chỉnh các độ dài nhánh và tham số mô hình bằng các thuật toán tối ưu số để LogL lớn nhất. Giá trị LogL tốt nhất đạt được chính là **điểm của topology đó**. Đây là hàm chấm điểm mà phần 4 đã yêu cầu.

#### 🚩 Trạm dừng sau phần 7

**Giờ ta đã biết**

- Likelihood của alignment là tích likelihood các cột (tổng các $\ln$).
- Chữ ở tổ tiên chưa biết thì cộng qua mọi khả năng.
- Nguyên lý ròng rọc: vị trí gốc không đổi likelihood, nên ML cho cây không gốc.
- Thuật toán cắt tỉa tính nhanh theo từng nút; gap được coi là không có thông tin.

**Vì sao cần bước tiếp**

Ta đã chấm điểm được một topology. Nhưng có tới $(2n-5)!!$ topology. Phần 8 trình bày cách IQ-TREE di chuyển khéo léo trong không gian khổng lồ đó.

**Nguồn đã đối chiếu**

- Likelihood là tích theo các cột; thuật toán cắt tỉa đi từ lá lên gốc; ví dụ $4^{11}$ tổ hợp với 12 chuỗi: [bài tổng quan về Felsenstein (1981)](https://pmc.ncbi.nlm.nih.gov/articles/PMC7803665); [Felsenstein’s tree-pruning algorithm](https://en.wikipedia.org/wiki/Felsenstein%27s_tree-pruning_algorithm); tên gọi sum-product, message passing: [arXiv:2405.09327](https://arxiv.org/pdf/2405.09327). Felsenstein J. (1981) *J. Mol. Evol.* 17:368–376.
- Nguyên lý ròng rọc: [arXiv:2204.07747](https://arxiv.org/pdf/2204.07747); [UBC CPSC536A, ghi chú bài giảng](https://www.cs.ubc.ca/labs/algorithms/Courses/CPSC536A-02/phylonotes.txt).
- Xử lý gap và ký tự thiếu: [IQ-TREE FAQ](https://iqtree.github.io/doc/Frequently-Asked-Questions).
- Mọi con số trong ví dụ và các thí nghiệm đã được tính lại bằng máy theo công thức JC69.


---


## 8. Tìm cây tốt nhất

Đến đây ta có một *hàm chấm điểm*: đưa vào một topology, phần 7 trả về LogL tốt nhất của nó. Phần 4 cho biết không thể chấm hết mọi topology. Vậy phải **tìm kiếm**: bắt đầu từ một cây nào đó, rồi lần lượt thử những cây “gần” nó, giữ lại cây nào có điểm cao hơn. Phần này trả lời hai câu hỏi: thế nào là hai cây “gần nhau”, và IQ-TREE đi trong không gian cây theo chiến lược nào.

### Hàng xóm của một cây: các phép biến đổi cây

Một **phép biến đổi cây** *(tree rearrangement)* là một thao tác nhỏ biến cây này thành cây khác. Các cây tạo ra được từ cây hiện tại bằng đúng một thao tác gọi là **hàng xóm** *(neighbors)* của nó. Hai phép quan trọng nhất:

**NNI** *(nearest neighbor interchange, hoán đổi láng giềng gần nhất)*. Chọn một nhánh trong. Nhánh này nối hai nút, và quanh nó có bốn “cây con”, gọi là W, X, Y, Z (mỗi cái có thể chỉ là một lá, hoặc cả một nhóm lá). Cây hiện tại gom W với X và Y với Z. NNI đổi chỗ một cây con ở bên này với một cây con ở bên kia, tạo ra hai cách gom còn lại.

> 🖼️ **[Hình minh họa — chỉ xem được trong bản artifact HTML gốc]**
>
> *Mô tả hình:* Ba cách sắp xếp bốn cây con W, X, Y, Z quanh một nhánh trong; hai cách sau là hai hàng xóm NNI của cách đầu
>
> NNI quanh nhánh trong màu xanh. Mọi thứ bên trong W, X, Y, Z giữ nguyên; chỉ cách gom quanh nhánh đó thay đổi. Khi W, X, Y, Z là bốn lá A, B, C, D, ba hình này chính là ba cây 4 lá của phần 4.

Mỗi nhánh trong cho đúng 2 hàng xóm NNI. Một cây không gốc nhị phân với $n$ lá có $n-3$ nhánh trong, nên nó có $2(n-3)$ hàng xóm NNI. Với 50 loài, đó là 94 hàng xóm, một con số nhỏ xíu so với khoảng $2{,}8\times10^{74}$ cây. Đó là lý do NNI nhanh: mỗi bước chỉ phải xét vài chục cây.

**SPR** *(subtree pruning and regrafting, cắt và ghép cây con)*. Cắt rời một cây con khỏi cây, rồi ghép nó vào giữa một nhánh khác ở bất kỳ đâu. SPR tạo ra nhiều hàng xóm hơn NNI rất nhiều, nên mỗi bước “nhảy” được xa hơn nhưng tốn công hơn. Trong IQ-TREE, SPR được dùng ở giai đoạn dựng các cây khởi đầu (tùy chọn bán kính SPR mặc định là 6); giai đoạn tìm kiếm ML chính dùng NNI.

#### 📎 Kiến thức bổ sung (TBR)

TBR *(tree bisection and reconnection)* tổng quát hơn nữa: cắt cây làm hai mảnh rồi nối lại bằng một cặp nhánh bất kỳ, mỗi nhánh thuộc một mảnh. Theo thứ tự từ nhỏ đến lớn, vùng hàng xóm của NNI nằm trong SPR, và SPR nằm trong TBR. Vùng hàng xóm càng lớn thì càng ít bị mắc kẹt, nhưng mỗi bước càng tốn thời gian.

### Leo đồi và cái bẫy cực đại địa phương

Chiến lược đơn giản nhất gọi là **leo đồi** *(hill climbing)*: từ cây hiện tại, xét các hàng xóm; nếu có hàng xóm điểm cao hơn thì chuyển sang đó; lặp lại cho đến khi không hàng xóm nào tốt hơn.

Điểm dừng của leo đồi là một **cực đại địa phương** *(local optimum)*: tốt hơn mọi hàng xóm của nó, nhưng chưa chắc là cây tốt nhất trong toàn bộ không gian, tức **cực đại toàn cục** *(global optimum)*. Không gian cây thật thường có nhiều cực đại địa phương, nên kết quả leo đồi phụ thuộc nhiều vào điểm xuất phát.

> **Ví von:** Leo núi trong sương mù dày, chỉ nhìn thấy chỗ đặt chân kế bên. Bạn luôn bước lên chỗ cao hơn. Khi mọi hướng đều đi xuống, bạn dừng, nhưng có thể đang đứng trên một ngọn đồi nhỏ, trong khi đỉnh núi thật ở chỗ khác mà bạn không nhìn thấy.
>
> **Cơ chế thật:** “Vị trí” là một topology; “độ cao” là LogL tốt nhất của topology đó (sau khi tối ưu độ dài nhánh và tham số mô hình); “bước chân” là một phép NNI. Không gian này rời rạc và có số chiều rất lớn, không phải một đường cong liên tục như hình minh họa bên dưới.

> 🔧 **Thử nghiệm: leo đồi, xáo trộn và quy tắc dừng** *(khung tương tác — chỉ hoạt động trong bản artifact HTML gốc, ở đây là phần mô tả)*
>
> Đây là *ví von* một chiều cho không gian cây: mỗi điểm trên trục ngang là một “cây”, hai điểm cạnh nhau là hàng xóm. Hãy tự thấy leo đồi bị kẹt thế nào, và xáo trộn giúp thoát ra ra sao.

Trong ví von này, khi cho máy chạy thử hàng chục nghìn lần thì thấy: leo đồi một lần từ điểm ngẫu nhiên chỉ tới được đỉnh cao nhất khoảng 21% số lần; thêm bước xáo trộn và quy tắc dừng sau 10 lần không cải thiện, tỷ lệ này lên khoảng 82%. Vẫn không phải 100%. Đó chính là tình thế của mọi phần mềm ML: tìm kiếm tốt hơn nhiều so với leo đồi đơn thuần, nhưng không có gì đảm bảo tuyệt đối.

### Chiến lược của IQ-TREE

Thuật toán tìm kiếm của IQ-TREE (Nguyen và cộng sự, 2015) kết hợp leo đồi bằng NNI với một bước **xáo trộn ngẫu nhiên** *(stochastic perturbation)* để thoát khỏi cực đại địa phương, và luôn giữ một nhóm nhỏ các cây tốt nhất gọi là **tập cây ứng viên** *(candidate tree set)*. Bạn cần biết thêm hai khái niệm dùng ở bước khởi đầu. *Cây parsimony* là cây cần ít lần thay đổi nhất để giải thích dữ liệu; dựng rất nhanh và thường là điểm xuất phát khá tốt. *BIONJ* là một biến thể của neighbor-joining, dựng cây nhanh từ bảng khoảng cách giữa các cặp chuỗi.

| Bước | Việc IQ-TREE làm | Tùy chọn và mặc định |
|---|---|---|
| 1. Tạo điểm xuất phát | Dựng 100 cây parsimony (bằng chiến lược giống RAxML) cùng một cây BIONJ, để có nhiều điểm xuất phát khác nhau. | `-ninit 100` |
| 2. Sàng lọc | Ước lượng nhanh LogL của các topology khác nhau, chọn 20 cây tốt nhất, leo đồi bằng NNI trên từng cây đến cực đại địa phương. | `-ntop 20` |
| 3. Lập tập ứng viên | Giữ 5 cây có LogL cao nhất làm tập ứng viên. | `-nbest 5` |
| 4. Vòng lặp chính | Lấy một cây trong tập ứng viên, xáo trộn nó bằng các phép NNI ngẫu nhiên, rồi leo đồi NNI lại. Nếu cây mới đủ tốt, nó thế chỗ một cây kém hơn trong tập. | Cường độ xáo trộn `-pers 0.5` |
| 5. Dừng | Dừng khi 100 vòng lặp liên tiếp không tìm thấy cây tốt hơn cây tốt nhất hiện có. Cây tốt nhất là cây ML được báo cáo. | `-nstop 100` |

Trong bài báo gốc, khi cho cùng một lượng thời gian máy tính, IQ-TREE tìm được LogL cao hơn RAxML và PhyML ở khoảng 62–87% số alignment được thử. Con số này cho thấy chiến lược tìm kiếm quan trọng thế nào: cùng một hàm chấm điểm, nhưng cách đi khác nhau cho ra những cây khác nhau.

> **⚠️ Cảnh báo:** **Kết quả tìm kiếm không được đảm bảo là tối ưu.** Tài liệu IQ-TREE nói rõ: các giá trị mặc định được xác định bằng thực nghiệm và có thể không phù hợp với mọi bộ dữ liệu. Nếu nghi ngờ quá trình tìm kiếm bị kẹt, nên chạy lại ít nhất 10 lần độc lập; hai tùy chọn đáng chỉnh nhất là `-pers` và `-nstop`. Ví dụ, với dữ liệu có nhiều chuỗi ngắn, tài liệu gợi ý giảm cường độ xáo trộn và tăng số vòng dừng, như `-pers 0.2 -nstop 500`.

#### 🚩 Trạm dừng sau phần 8

**Giờ ta đã biết**

- Tìm kiếm đi từ cây sang cây hàng xóm; NNI cho $2(n-3)$ hàng xóm, SPR cho nhiều hơn.
- Leo đồi dừng ở cực đại địa phương, chưa chắc là cây tốt nhất.
- IQ-TREE: 100 cây parsimony và BIONJ, tập 5 ứng viên, xáo trộn rồi leo lại, dừng sau 100 vòng không cải thiện.
- Kết quả là heuristic; khi nghi ngờ, chạy nhiều lần.

**Vì sao cần bước tiếp**

Giờ ta có một cây ML. Nhưng dữ liệu chỉ là một mẫu hữu hạn các vị trí. Nếu có một mẫu khác, liệu các nhánh có còn như vậy? Cần đo độ tin cậy của *từng nhánh*.

**Nguồn đã đối chiếu**

- Thuật toán tìm kiếm (100 cây parsimony, 20 cây tốt nhất leo đồi NNI, tập 5 ứng viên, xáo trộn ngẫu nhiên) và so sánh với RAxML, PhyML: Nguyen L.-T., Schmidt H.A., von Haeseler A., Minh B.Q. (2015) *Mol. Biol. Evol.* 32:268–274, [doi:10.1093/molbev/msu300](https://doi.org/10.1093/molbev/msu300).
- Giá trị mặc định `-ninit`, `-ntop 20`, `-nbest 5`, `-nstop 100`, `-pers 0.5`, bán kính SPR 6, cây khởi đầu mặc định “100 cây parsimony + cây BIONJ”, khuyến nghị chạy ít nhất 10 lần và ví dụ `-pers 0.2 -nstop 500`: [IQ-TREE Command Reference](https://github.com/Cibiv/IQ-TREE/wiki/Command-Reference); [trang man iqtree (Debian)](https://manpages.debian.org/testing/iqtree/iqtree.1.en.html).
- Số hàng xóm NNI $2(n-3)$ suy ra trực tiếp từ việc mỗi nhánh trong cho 2 cách sắp xếp khác, và cây không gốc nhị phân có $n-3$ nhánh trong; phép NNI được mô tả quanh một nhánh với hai cấu hình thay thế: [Guindon et al. (2010)](https://hal-lirmm.ccsd.cnrs.fr/lirmm-00511784/document).


---


## 9. Độ tin cậy của nhánh

### Vì sao một cây ML là chưa đủ

Cây ML là ước lượng tốt nhất *từ dữ liệu đang có*. Nhưng alignment của bạn chỉ là một mẫu hữu hạn các vị trí. Giống như thăm dò ý kiến 100 người: kết quả có thể lệch chỉ vì tình cờ chọn trúng những người ấy. Nếu một nhánh chỉ được vài vị trí ủng hộ, một mẫu vị trí khác có thể đã cho ra cách gom khác.

Vì vậy, bên cạnh cây, ta cần một con số cho **từng nhánh trong**, tức từng split (phần 2), trả lời câu hỏi: split này ổn định đến đâu trước sự dao động ngẫu nhiên của dữ liệu? Con số đó gọi là **giá trị hỗ trợ** *(branch support)*. IQ-TREE cung cấp nhiều loại; phần này giải thích ba loại quan trọng nhất cho người mới.

### Dữ liệu ví dụ: bốn chuỗi ở đầu trang

Đây là bốn chuỗi tự tạo trong hình ở đầu bài, dài 16 vị trí. Hàng cuối phân loại từng cột:

```
cột    1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16
Loài A A T G C C T A A G T C G A T C G
Loài B A T G C T T A A G T C G G T C A
Loài C A T A C C T G A G C C G A T T G
Loài D A T A C C C G A G C C A G T T A
loại   – – AB – · · AB – – AB – · AC – AB AC
```

- **–** cột hằng: cả bốn loài giống nhau. Cột này không phân biệt được ba cây.
- **·** cột chỉ có một loài khác biệt. Cột này cũng không phân biệt được ba cây: cây nào cũng giải thích nó bằng đúng một thay đổi trên nhánh dẫn tới loài đó.
- **AB**, **AC** cột có hai chữ, mỗi chữ xuất hiện ở hai loài. Cột như vậy gọi là **vị trí thông tin** *(parsimony-informative site)*, vì nó “bỏ phiếu” cho một split: cột 3 (G, G, A, A) ủng hộ AB|CD.

Có 4 cột ủng hộ AB|CD và 2 cột ủng hộ AC|BD; không cột nào ủng hộ AD|BC. Tính bằng máy với mô hình JC69 và tối ưu độ dài nhánh, LogL tốt nhất của ba cây là khoảng −60,0 (AB|CD), −65,1 (AC|BD) và −66,5 (AD|BC). Vậy cây ML là AB|CD. Câu hỏi của phần này: *ta nên tin split AB|CD đến đâu*, khi có tới 2 cột phản đối nó?

### Bootstrap: tạo ra “những bộ dữ liệu khác” từ chính dữ liệu của mình

Lý tưởng nhất là lấy thêm nhiều mẫu dữ liệu mới rồi xem cây có đổi không. Điều đó thường không làm được. **Bootstrap** không tham số, do Felsenstein đưa vào phát sinh loài năm 1985, mô phỏng việc đó bằng một mẹo:

1. Tạo một alignment giả có cùng số cột bằng cách rút ngẫu nhiên các cột từ alignment gốc, **có hoàn lại** *(with replacement)*: rút xong một cột thì “trả lại”, nên một cột có thể được rút nhiều lần, cột khác có thể không được rút lần nào.
2. Dựng cây từ alignment giả đó.
3. Lặp lại nhiều lần, ví dụ $B = 1000$ lần, được 1000 cây.
4. Với mỗi split của cây ML, đếm tỷ lệ số cây bootstrap chứa split đó.

$$ \text{Hỗ trợ bootstrap của split } s = \frac{\text{số bản lặp có split } s}{B}\times 100\% $$

Ý tưởng: sự dao động giữa các alignment giả bắt chước sự dao động mà ta sẽ thấy nếu thực sự lấy được mẫu dữ liệu mới. Split được hầu hết các bản lặp giữ lại là split không phụ thuộc vào vài cột tình cờ.

> 🔧 **Thử nghiệm: tự chạy bootstrap trên bốn chuỗi** *(khung tương tác — chỉ hoạt động trong bản artifact HTML gốc, ở đây là phần mô tả)*
>
> Mỗi bản lặp rút ngẫu nhiên các cột có hoàn lại, rồi chọn cây. Để chạy tức thì trong trình duyệt, khung này chọn cây bằng cách đếm các vị trí thông tin ủng hộ từng split (với bốn loài, đây chính là tiêu chí parsimony); nếu hòa thì chia đều phiếu. IQ-TREE dùng ML cho bước này, nhưng logic lấy mẫu lại là như nhau.

**Điều bạn sẽ thấy.** Với 16 cột gốc, sau vài nghìn bản lặp, AB|CD thắng khoảng 79% số lần (giá trị chính xác, tính bằng máy: 79,4%). Chọn “32 cột” (giả sử gen dài gấp đôi nhưng tỷ lệ các kiểu cột y hệt), con số lên khoảng 88%; với 64 cột, khoảng 95%. Bài học: **giá trị hỗ trợ phản ánh lượng bằng chứng**. Cùng một tỷ lệ 4 phiếu thuận, 2 phiếu chống, dữ liệu càng nhiều thì kết luận càng khó bị lật bởi tình cờ.

### Vấn đề của bootstrap chuẩn, và ultrafast bootstrap

Bootstrap chuẩn bắt ta chạy lại toàn bộ việc tìm cây ML cho từng bản lặp, tức làm lại cả phần 8 hàng trăm hoặc hàng nghìn lần. Với dữ liệu thật, việc này rất tốn thời gian. Tài liệu IQ-TREE khuyến nghị tối thiểu 100 bản lặp cho bootstrap chuẩn (tùy chọn `-b 100`).

**Ultrafast bootstrap** *(UFBoot; Minh và cộng sự 2013, bản cải tiến UFBoot2 của Hoang và cộng sự 2018)* là một phép xấp xỉ nhanh hơn nhiều. Thay vì tìm cây lại từ đầu cho từng bản lặp, nó tận dụng các cây đã gặp trong quá trình tìm kiếm ML, và đánh giá nhanh chúng trên từng alignment lấy mẫu lại bằng kỹ thuật RELL *(resampling estimated log-likelihoods)*. README của IQ-TREE cho biết UFBoot nhanh hơn rapid bootstrap của RAxML từ 10 đến 40 lần. Tùy chọn là `-B 1000`; 1000 là số bản lặp tối thiểu được khuyến nghị.

> **⚠️ Cảnh báo:** **UFBoot và bootstrap chuẩn được đọc theo hai thang khác nhau.** Theo FAQ của IQ-TREE, bootstrap chuẩn có xu hướng *thận trọng* (đánh giá thấp), và người ta thường bắt đầu tin một clade khi nó đạt trên khoảng 80%. UFBoot thì ít lệch hơn: trong các mô phỏng của tác giả, hỗ trợ 95% ứng với xác suất khoảng 95% là clade đó đúng. Vì vậy với UFBoot, chỉ nên bắt đầu tin một clade khi hỗ trợ **từ 95% trở lên**, và **không được** so trực tiếp con số UFBoot với con số bootstrap chuẩn.

UFBoot có thể đánh giá quá cao hỗ trợ khi mô hình bị vi phạm nghiêm trọng. Từ phiên bản 1.6, IQ-TREE có tùy chọn `-bnni`: mỗi cây bootstrap được tối ưu thêm bằng leo đồi NNI trên chính alignment lấy mẫu lại của nó, để giảm nguy cơ này. Tài liệu khuyên thêm `-bnni` khi nghi ngờ có vi phạm mô hình nghiêm trọng.

### SH-aLRT: một phép kiểm định cho từng nhánh

Cách thứ hai không lấy mẫu lại để dựng cây mới. Nhớ lại phần 8: quanh mỗi nhánh trong có đúng ba cách gom bốn cây con, là cây hiện tại và hai hàng xóm NNI. **aLRT** *(approximate likelihood-ratio test; Anisimova & Gascuel 2006)* so sánh LogL của cây hiện tại với LogL của hàng xóm NNI tốt nhất quanh nhánh đó. Khoảng cách càng lớn thì nhánh càng được dữ liệu ủng hộ rõ. Phép tính nhanh, vì chỉ tối ưu lại nhánh đang xét và bốn nhánh kề nó. **SH-aLRT** (Guindon và cộng sự 2010) là phiên bản dùng thủ tục kiểu Shimodaira–Hasegawa để tính giá trị hỗ trợ. Trong IQ-TREE, tùy chọn là `-alrt 1000`, với 1000 là số bản lặp tối thiểu được khuyến nghị.

FAQ của IQ-TREE khuyên chạy cả hai cùng lúc. Mỗi nhánh khi đó mang hai con số dạng `SH-aLRT/UFBoot`, và người ta thường bắt đầu tin một clade khi **SH-aLRT ≥ 80% và UFBoot ≥ 95%**.

| Phương pháp | Ý tưởng | Tùy chọn IQ-TREE | Ngưỡng thường dùng |
|---|---|---|---|
| Bootstrap chuẩn (Felsenstein 1985) | Lấy mẫu lại cột, tìm lại cây ML cho từng bản lặp | `-b 100` (tối thiểu 100) | Bắt đầu tin khi trên khoảng 80%; thang thận trọng |
| UFBoot (Minh 2013; Hoang 2018) | Lấy mẫu lại cột, đánh giá nhanh các cây đã gặp bằng RELL | `-B 1000` (tối thiểu 1000) | Bắt đầu tin khi ≥ 95% |
| SH-aLRT (Guindon 2010) | So cây hiện tại với hai hàng xóm NNI quanh từng nhánh | `-alrt 1000` (tối thiểu 1000) | ≥ 80%, dùng kèm UFBoot |

### Đọc giá trị hỗ trợ trên một cây thật

Trong hướng dẫn chính thức của IQ-TREE, file ví dụ gồm chuỗi DNA ty thể của nhiều loài động vật. Khi chạy với cả `-alrt 1000` và `-B 1000`, file `.iqtree` ghi mỗi nhánh một cặp số trong ngoặc, dạng SH-aLRT/UFBoot. Hướng dẫn nhận xét: vị trí của Sphenodon (39/51) và Turtle (85/72) trong nhóm bò sát, cũng như vị trí của Seal trong nhóm thú (68,3/75), được hỗ trợ kém; các nhánh khác được hỗ trợ tốt. Để ý trường hợp Turtle: SH-aLRT 85% vượt ngưỡng 80%, nhưng UFBoot 72% dưới ngưỡng 95%, nên theo quy tắc “cả hai cùng đạt”, clade đó chưa đủ tin cậy.

> **Ví von:** Hỗ trợ bootstrap giống như hỏi: “Nếu tôi làm lại cuộc khảo sát với một nhóm người khác, kết luận này có giữ nguyên không?” Nó đo độ ổn định của kết luận, không phải việc câu hỏi khảo sát có được đặt đúng hay không.
>
> **Cơ chế thật:** Bootstrap và SH-aLRT đo ảnh hưởng của *sai số lấy mẫu* (có ít vị trí). Chúng không bảo vệ được trước *sai số hệ thống*: nếu mô hình hoặc alignment sai, thêm dữ liệu có thể làm hỗ trợ cho một cây sai tăng lên. Phần 11 quay lại vấn đề này.

> **💡 Lưu ý:** **Hỗ trợ là của split, không phải của cả cây.** Một cây có thể có vài nhánh 100% và vài nhánh 50%. Khi viết báo cáo, hãy bàn về từng clade quan trọng với con số của nó, thay vì nói “cây này đáng tin”.

#### 🚩 Trạm dừng sau phần 9

**Giờ ta đã biết**

- Giá trị hỗ trợ đo độ ổn định của từng split trước dao động ngẫu nhiên của dữ liệu.
- Bootstrap: lấy mẫu lại cột có hoàn lại, đếm tỷ lệ bản lặp chứa split.
- UFBoot (`-B 1000`) tin khi ≥ 95%; SH-aLRT (`-alrt 1000`) tin khi ≥ 80%; không so UFBoot với bootstrap chuẩn.
- Hỗ trợ cao không chống được sai số hệ thống.

**Vì sao cần bước tiếp**

Bạn đã hiểu mọi mảnh ghép lý thuyết: MSA, mô hình, likelihood, tìm kiếm, độ tin cậy. Giờ là lúc gõ lệnh thật: cài IQ-TREE, chạy một phân tích đầy đủ, và đọc từng file kết quả.

**Nguồn đã đối chiếu**

- Tùy chọn `-B` (tối thiểu 1000), `-b` (tối thiểu 100), `-alrt` (tối thiểu 1000), `-bnni` từ bản 1.6, ví dụ Sphenodon, Turtle, Seal: [IQ-TREE Beginner’s Tutorial](https://iqtree.github.io/doc/Tutorial).
- Diễn giải UFBoot (95% ứng với xác suất khoảng 95%), bootstrap chuẩn trên 80%, quy tắc SH-aLRT ≥ 80% và UFBoot ≥ 95%: [IQ-TREE FAQ](https://github.com/Cibiv/IQ-TREE/wiki/Frequently-Asked-Questions); [FAQ trên trang chính](https://iqtree.github.io/doc/Frequently-Asked-Questions).
- Minh B.Q., Nguyen M.A.T., von Haeseler A. (2013) *Mol. Biol. Evol.* 30:1188–1195, [bài báo](https://academic.oup.com/mbe/article/30/5/1188/997508). Hoang D.T. et al. (2018) UFBoot2, *Mol. Biol. Evol.* 35:518–522, [doi:10.1093/molbev/msx281](https://doi.org/10.1093/molbev/msx281). Tốc độ 10–40 lần so với RAxML rapid bootstrap: [README IQ-TREE 3](https://github.com/iqtree/iqtree3).
- aLRT so sánh cây hiện tại với cấu hình NNI tốt nhì quanh nhánh, chỉ tối ưu nhánh đó và bốn nhánh kề: Anisimova M., Gascuel O. (2006) *Syst. Biol.* 55:539–552, [bản lưu trữ HAL](https://hal-lirmm.ccsd.cnrs.fr/lirmm-00136658). SH-aLRT: Guindon S. et al. (2010) *Syst. Biol.* 59:307–321, [doi:10.1093/sysbio/syq010](https://doi.org/10.1093/sysbio/syq010). Khi mô hình bị vi phạm nghiêm trọng, aLRT, aBayes và xác suất hậu nghiệm Bayes có thể cho tỷ lệ dương tính giả cao hơn: [Anisimova et al. (2011)](https://pmc.ncbi.nlm.nih.gov/articles/3158332), *Syst. Biol.* 60:685–699.
- Felsenstein J. (1985) Confidence limits on phylogenies: an approach using the bootstrap. *Evolution* 39:783–791.
- Dữ liệu bốn chuỗi là tự tạo. LogL của ba cây (JC69, tối ưu độ dài nhánh) và tỷ lệ bootstrap chính xác 79,4% / 87,9% / 95,2% cho 16 / 32 / 64 cột được tính bằng máy (liệt kê đầy đủ mọi kết quả lấy mẫu theo phân phối đa thức).


---


## 10. IQ-TREE thực hành

**IQ-TREE** là phần mềm mã nguồn mở dựng cây phát sinh loài bằng maximum likelihood. Nó gói mọi thứ bạn đã học vào một chương trình: ModelFinder chọn mô hình (phần 6), thuật toán cắt tỉa tính likelihood (phần 7), thuật toán tìm kiếm ngẫu nhiên (phần 8), UFBoot và SH-aLRT đo độ tin cậy (phần 9). IQ-TREE là chương trình *dòng lệnh*: bạn gõ lệnh trong cửa sổ Terminal (Windows gọi là Command Prompt), không bấm chuột vào biểu tượng.

### Cài đặt

| Cách | Lệnh hoặc địa chỉ | Ghi chú |
|---|---|---|
| Conda (Bioconda) | `conda install -c bioconda iqtree` | Tiện nếu bạn đã dùng Conda cho các công cụ tin sinh khác |
| Tải bản dựng sẵn | [iqtree.github.io](https://iqtree.github.io/) | Có bản cho Windows, macOS, Linux; chép file trong thư mục `bin` vào đường dẫn hệ thống |
| Web server | Máy chủ của CIBIV (Áo), Los Alamos (Mỹ), CIPRES | Không cần cài; phù hợp để thử với bộ dữ liệu nhỏ |

> **💡 Lưu ý:** **Tên lệnh khác nhau theo phiên bản.** IQ-TREE 3 biên dịch ra chương trình tên `iqtree3`; IQ-TREE 2 là `iqtree2`; tài liệu hướng dẫn thường viết chung là `iqtree`. Bài này dùng `iqtree3`. Nếu máy bạn cài bản khác, chỉ cần đổi tên lệnh; các tùy chọn dưới đây theo cú pháp từ bản 2 trở đi (bản 1.x dùng `-bb` thay cho `-B`, `-nt` thay cho `-T`, `-pre` thay cho `--prefix`). Gõ `iqtree3 -h` để xem toàn bộ tùy chọn.

### Một quy trình đầy đủ, từ chuỗi thô đến cây

```bash
# Bước 1. Căn chỉnh các chuỗi thô (phần 3)
mafft --auto sequences.fasta > aligned.fasta

# Bước 2 (tùy chọn, còn tranh luận). Cắt tỉa alignment (phần 3)
trimal -in aligned.fasta -out trimmed.fasta -automated1

# Bước 3. Chọn mô hình + tìm cây ML + UFBoot + SH-aLRT, trong một lệnh
iqtree3 -s aligned.fasta -m MFP -B 1000 -alrt 1000 -T AUTO --prefix run1
```

Đọc từng phần của lệnh ở bước 3:

| Tùy chọn | Ý nghĩa | Liên hệ với bài |
|---|---|---|
| `-s aligned.fasta` | File alignment đầu vào (bắt buộc). Nhận PHYLIP, FASTA, NEXUS, CLUSTAL, MSF. | Phần 3 |
| `-m MFP` | ModelFinder Plus: chọn mô hình tốt nhất theo BIC rồi dùng nó để dựng cây. Từ bản 1.5.4 đây là mặc định, nên có thể bỏ, nhưng ghi rõ giúp người đọc lệnh hiểu bạn đã làm gì. | Phần 6 |
| `-B 1000` | Ultrafast bootstrap với 1000 bản lặp (tối thiểu khuyến nghị). | Phần 9 |
| `-alrt 1000` | SH-aLRT với 1000 bản lặp (tối thiểu khuyến nghị). | Phần 9 |
| `-T AUTO` | Tự đo và chọn số nhân CPU hiệu quả nhất. Dùng thêm `-ntmax 8` nếu muốn giới hạn tối đa 8 nhân. | Tốc độ |
| `--prefix run1` | Đặt tiền tố cho mọi file kết quả. Mặc định tiền tố là tên file alignment; đặt tên riêng giúp nhiều lần chạy trong cùng thư mục không ghi đè lên nhau. | Quản lý file |

Một số tùy chọn khác hay dùng:

| Tùy chọn | Khi nào dùng |
|---|---|
| `-m TIM2+I+G` | Khi đã biết mô hình (ví dụ từ lần chạy trước) và không muốn chọn lại. |
| `-m MF` | Chỉ chọn mô hình, không dựng cây. |
| `-o TenLoai` | Chỉ định nhóm ngoài để đặt gốc; cây trong `.treefile` sẽ được đặt gốc theo đó (phần 2). Mặc định IQ-TREE vẽ theo chuỗi đầu tiên trong alignment, nhưng đó chỉ là cách vẽ, cây vẫn không gốc. |
| `-bnni` | Đi kèm `-B` khi nghi ngờ mô hình bị vi phạm nghiêm trọng (phần 9). |
| `-redo` | Chạy lại và ghi đè kết quả cũ. Nếu không có tùy chọn này, IQ-TREE từ chối chạy lại một phân tích đã hoàn tất. |
| `-pers`, `-nstop` | Chỉnh chiến lược tìm kiếm khi nghi ngờ bị kẹt ở cực đại địa phương (phần 8). |

### Các file kết quả

Sau khi chạy lệnh ở bước 3, thư mục sẽ có một loạt file bắt đầu bằng `run1.`. Không phải file nào cũng cần đọc:

| File | Nội dung | Mức độ cần đọc |
|---|---|---|
| `.iqtree` | Báo cáo chính, đọc được bằng mắt: mô hình được chọn, tham số, LogL, cây vẽ bằng ký tự kèm giá trị hỗ trợ. | Luôn đọc |
| `.treefile` | Cây ML dạng Newick, có giá trị hỗ trợ làm nhãn nút. Mở bằng FigTree hoặc iTOL. | Luôn dùng |
| `.log` | Nhật ký toàn bộ lần chạy (cũng là những gì in ra màn hình), gồm cả các cảnh báo. | Đọc lướt, tìm WARNING |
| `.contree` | Cây đồng thuận từ các cây bootstrap, độ dài nhánh tối ưu lại trên alignment gốc. Chỉ có khi dùng `-B`. | Tham khảo |
| `.splits.nex` | Tỷ lệ hỗ trợ của mọi split xuất hiện trong các cây bootstrap, kể cả những split mâu thuẫn không lọt vào cây đồng thuận; xem được bằng SplitsTree. | Khi muốn tìm hiểu tín hiệu mâu thuẫn |
| `.ckp.gz` | File checkpoint để chạy tiếp khi bị ngắt. Chạy lại đúng lệnh cũ, IQ-TREE sẽ tiếp tục từ chỗ dừng. Không sửa file này. | Không cần mở |
| `.model.gz` | LogL của mọi mô hình đã thử; cũng dùng làm checkpoint cho bước chọn mô hình. Tài liệu cũ ghi tên là `.model`. | Khi muốn xem bảng so sánh mô hình |
| `.mldist`, `.bionj` | Ma trận khoảng cách ML giữa các cặp chuỗi, và cây BIONJ dùng làm một điểm xuất phát (phần 8). | Hiếm khi cần |
| `.uniqueseq.phy` | Alignment sau khi bỏ bớt các chuỗi giống hệt nhau; chỉ xuất hiện khi dữ liệu có chuỗi trùng. | Hiếm khi cần |

### Đọc file `.iqtree` theo thứ tự

1. **Mô hình được chọn.** Tìm dòng ghi mô hình tốt nhất theo BIC, ví dụ `TIM2+I+G4`. Đọc tên theo cách ở phần 6. Đây là thông tin bắt buộc phải ghi trong báo cáo.
2. **Tham số của mô hình.** Các tốc độ thay thế, tần suất nucleotide, tỷ lệ vị trí bất biến, tham số $\alpha$ của gamma. Ví dụ $\alpha$ nhỏ nghĩa là tốc độ giữa các vị trí rất chênh lệch.
3. **LogL và tổng độ dài cây.** LogL chỉ dùng để so sánh trên cùng alignment (phần 5). Tổng độ dài cây là tổng mọi độ dài nhánh, đơn vị thay thế trên mỗi vị trí.
4. **Cây vẽ bằng ký tự.** Dòng ghi chú phía trên giải thích các con số trong ngoặc là SH-aLRT (%) / UFBoot (%). Kèm theo là ghi chú nhắc rằng cây không có gốc, dù một loài được vẽ ở vị trí trông như gốc.

Trong file `.treefile`, cùng thông tin ấy nằm trong định dạng Newick (phần 2). Ví dụ minh họa với số tự đặt:

```bash
((Loai_A:0.02,Loai_B:0.21)87.5/96:0.30,Loai_C:0.01,Loai_D:0.30);
```

Nhãn `87.5/96` đứng ngay sau dấu ngoặc đóng của nhóm (Loai_A, Loai_B): nhánh nối nhóm này với phần còn lại có SH-aLRT 87,5% và UFBoot 96%, tức split AB|CD đạt cả hai ngưỡng. Số `:0.30` sau nhãn là độ dài của nhánh đó.

### Xem và trình bày cây

FigTree (phần mềm trên máy) và iTOL (trang web) đọc được file `.treefile`, cho phép hiển thị nhãn nút (giá trị hỗ trợ), đặt gốc lại ở nhóm ngoài, tô màu clade và xuất hình cho báo cáo. Khi trình bày: ghi rõ cây đã được đặt gốc bằng cách nào, hiển thị thang độ dài nhánh, và chú thích ý nghĩa của các con số trên nút.

#### 🚩 Trạm dừng sau phần 10

**Giờ ta đã biết**

- Một lệnh IQ-TREE làm trọn chọn mô hình, tìm cây ML, UFBoot và SH-aLRT.
- Ba file chính: `.iqtree` (báo cáo), `.treefile` (cây), `.log` (nhật ký, cảnh báo).
- Nhãn `a/b` trên nút là SH-aLRT/UFBoot của split tương ứng.

**Vì sao cần bước tiếp**

Chạy ra một cây thì dễ. Biết khi nào cây ấy có thể sai, và báo cáo sao cho người khác kiểm tra lại được, mới là phần làm nên một đồ án tốt.

**Nguồn đã đối chiếu**

- Định dạng đầu vào, các file `.iqtree`, `.treefile`, `.log`, `.ckp.gz`, `.model`, `.contree`, `.splits.nex`; tùy chọn `-m MFP`, `-m MF`, `-B`, `-alrt`, `-T AUTO`, `-ntmax`, `--prefix`, `-redo`, cú pháp bản 1.x; ví dụ `TIM2+I+G4`; ghi chú cây không gốc: [IQ-TREE Beginner’s Tutorial](https://iqtree.github.io/doc/Tutorial).
- Tùy chọn `-o` (đặt gốc theo nhóm ngoài, mặc định là chuỗi đầu tiên): [IQ-TREE Command Reference](https://github.com/Cibiv/IQ-TREE/wiki/Command-Reference). Tên file `.model.gz`, `.mldist`, `.bionj`, `.uniqueseq.phy` trong một lần chạy thật: [hướng dẫn của M. Matschiner](https://github.com/mmatschiner/tutorials/blob/master/ml_species_tree_inference/README.md); [tóm tắt lệnh iqtree](https://linuxcommandlibrary.com/man/iqtree).
- Tên chương trình `iqtree3` khi biên dịch IQ-TREE 3; cài qua Bioconda: [Compilation Guide](https://github.com/thomaskf/iqtree3/wiki/Compilation-Guide), [Quickstart](https://github.com/thomaskf/iqtree3/wiki/Quickstart). Các web server: [trang tài liệu IQ-TREE](https://iqtree.github.io/doc/Tutorial).


---


## 11. Giới hạn và checklist cho đồ án

Mọi bước trong quy trình đều dựa trên giả định. Phần này gom lại những chỗ cây có thể sai, để bạn biết cần kiểm tra gì và cần viết gì trong phần thảo luận.

### Hai loại sai số

|   | Sai số lấy mẫu *(sampling error)* | Sai số hệ thống *(systematic error)* |
|---|---|---|
| Nguyên nhân | Có quá ít vị trí; kết quả bị ảnh hưởng bởi tình cờ | Phương pháp hoặc mô hình không khớp với quá trình tiến hóa thật; alignment sai |
| Thêm dữ liệu có giúp không? | Có: hỗ trợ cho câu trả lời đúng tăng dần | Không, thậm chí có hại: phương pháp có thể hội tụ ngày càng chắc chắn về một cây sai |
| Bootstrap, SH-aLRT phát hiện được? | Có, đó chính là thứ chúng đo | Thường là không |

### Hút nhánh dài: ví dụ điển hình của sai số hệ thống

**Hút nhánh dài** *(long-branch attraction, LBA)* là khi hai dòng dõi có quan hệ xa bị suy luận nhầm là gần nhau, chỉ vì cả hai đều thay đổi rất nhiều (có nhánh dài). Trên các nhánh dài, nhiều thay thế tình cờ trùng nhau (hai dòng dõi độc lập cùng đổi thành một chữ), tạo ra sự giống nhau giả. Felsenstein (1978) chỉ ra parsimony có thể sai một cách nhất quán trong tình huống này; phân tích maximum likelihood cũng có thể bị ảnh hưởng, đặc biệt khi mô hình quá đơn giản so với dữ liệu.

> 🖼️ **[Hình minh họa — chỉ xem được trong bản artifact HTML gốc]**
>
> *Mô tả hình:* Bên trái là cây thật AB|CD với nhánh A và C rất dài; bên phải là cây suy luận sai AC|BD trong đó hai nhánh dài bị gom vào nhau
>
> A và C là hai nhánh dài nhưng không họ hàng gần. Những thay đổi trùng hợp trên hai nhánh dài có thể khiến phương pháp gom chúng lại với nhau. Nhóm ngoài thường có nhánh dài, nên các loài có nhánh dài trong nhóm nghiên cứu hay bị “hút” về phía nhóm ngoài.

Bài tổng quan của Bergsten (2005) nêu các cách hay dùng để giảm nguy cơ LBA:

- Thêm loài để “chia nhỏ” các nhánh dài.
- Dùng phương pháp dựa trên mô hình (như ML) với mô hình phù hợp hơn, thay vì parsimony.
- Loại bớt các vị trí tiến hóa rất nhanh, ví dụ vị trí thứ ba của codon trong gen mã hóa protein.
- Chạy phân tích cả khi có và khi không có nhóm ngoài, để xem nhóm ngoài chỉ đặt gốc hay còn làm thay đổi cấu trúc bên trong nhóm nghiên cứu.

### Những giới hạn khác cần nhớ

- **Alignment sai thì cây sai** (phần 3). Không có bước nào phía sau sửa được lỗi tương đồng.
- **Mô hình chỉ là xấp xỉ** (phần 6). ModelFinder chọn mô hình tốt nhất *trong danh sách được thử*, không đảm bảo mô hình đó mô tả đúng dữ liệu. Ở đầu mỗi lần chạy, IQ-TREE kiểm định thành phần nucleotide (hoặc amino acid) của từng chuỗi và đánh dấu những chuỗi lệch hẳn so với trung bình; hãy đọc kết quả này trong file `.log`.
- **Tìm kiếm là heuristic** (phần 8). Chạy nhiều lần; nếu các lần cho cây khác nhau với LogL gần nhau, dữ liệu có lẽ không đủ để phân định.
- **Cây gen không nhất thiết là cây loài** (phần 2). Kết luận từ một gen là kết luận về lịch sử của gen đó.
- **Độ dài nhánh không phải thời gian** (phần 2). Muốn nói “tách nhau bao nhiêu năm” cần phân tích định tuổi riêng.

### Checklist thực hành

| Giai đoạn | Việc cần làm |
|---|---|
| Dữ liệu | Cùng một gen (ortholog) từ mọi loài; ghi mã truy cập (accession) của từng chuỗi; chọn nhóm ngoài có cơ sở từ tài liệu; đặt tên chuỗi chỉ gồm chữ, số, `_`, `-`, `.` để tránh bị IQ-TREE tự đổi tên. |
| Căn chỉnh | Chạy MAFFT (hoặc công cụ khác), **mở ra xem bằng mắt**; nếu cắt tỉa thì dựng cây cả trước và sau khi cắt, so sánh và ghi lại. |
| Mô hình và cây | Dùng `-m MFP`; ghi mô hình được chọn; đọc cảnh báo trong `.log`; chạy vài lần độc lập và so sánh LogL. |
| Độ tin cậy | `-B 1000 -alrt 1000`; diễn giải theo ngưỡng SH-aLRT ≥ 80% và UFBoot ≥ 95%; bàn về từng clade quan trọng, kể cả clade hỗ trợ yếu. |
| Hình | Đặt gốc bằng nhóm ngoài và nói rõ điều đó; có thang độ dài nhánh; chú thích con số trên nút. |
| Tái lập | Ghi phiên bản của mọi phần mềm, dòng lệnh đầy đủ; lưu file `.log` và `.iqtree` vào phụ lục hoặc kho mã. |
| Trích dẫn | IQ-TREE (bài báo của phiên bản bạn dùng), ModelFinder (Kalyaanamoorthy et al. 2017), UFBoot2 (Hoang et al. 2018), SH-aLRT (Guindon et al. 2010), công cụ căn chỉnh và cắt tỉa. |

#### 📎 Kiến thức bổ sung (khung đoạn “Phương pháp” cho báo cáo)

Đây là khung gợi ý; thay phần trong ngoặc vuông bằng thông tin thật của bạn và chỉ giữ những câu đúng với việc bạn đã làm.

*“Trình tự gen [tên gen] của [số] loài được tải từ [cơ sở dữ liệu] (mã truy cập ở Bảng S1). Các trình tự được căn chỉnh bằng MAFFT [phiên bản] với tùy chọn [--auto]. [Alignment được cắt tỉa bằng trimAl [phiên bản] với -automated1.] Cây phát sinh loài được dựng bằng maximum likelihood trong IQ-TREE [phiên bản]. Mô hình thay thế được chọn bằng ModelFinder theo tiêu chí BIC; mô hình được chọn là [tên mô hình]. Độ tin cậy của các nhánh được đánh giá bằng 1000 bản lặp ultrafast bootstrap và 1000 bản lặp SH-aLRT. Cây được đặt gốc bằng [nhóm ngoài] và hiển thị bằng [FigTree/iTOL].”*

#### 🚩 Trạm dừng sau phần 11

**Giờ ta đã biết**

- Hỗ trợ cao chỉ loại trừ sai số lấy mẫu, không loại trừ sai số hệ thống.
- Hút nhánh dài là sai số hệ thống kinh điển; có các cách giảm nguy cơ.
- Checklist từ dữ liệu đến trích dẫn giúp đồ án kiểm tra và tái lập được.

**Vì sao cần bước tiếp**

Phần cuối là bảng thuật ngữ Việt–Anh để tra lại nhanh khi đọc tài liệu tiếng Anh, và danh sách tài liệu gốc nên đọc tiếp.

**Nguồn đã đối chiếu**

- Định nghĩa LBA, việc ML cũng có thể bị ảnh hưởng, xu hướng bị hút về nhóm ngoài: [Long branch attraction](https://en.wikipedia.org/wiki/Long_branch_attraction) (theo Bergsten 2005); cơ chế thay thế trùng hợp trên nhánh dài: [bioRxiv 2024.12.06.627281](https://www.biorxiv.org/content/10.1101/2024.12.06.627281.full.pdf).
- Các cách tránh và phát hiện LBA, khuyến nghị chạy có và không có nhóm ngoài: Bergsten J. (2005) A review of long-branch attraction. *Cladistics* 21:163–193, [bản PDF](https://evo.dbio.uevora.pt/A_review_of_long-branch_attraction.pdf).
- Kiểm định thành phần ký tự của từng chuỗi ở đầu mỗi lần chạy: [IQ-TREE FAQ](https://iqtree.github.io/doc/Frequently-Asked-Questions). Ký tự được phép trong tên chuỗi: [IQ-TREE Beginner’s Tutorial](https://iqtree.github.io/doc/Tutorial).
- Khoảng dữ liệu tăng mà phương pháp vẫn hội tụ về cây sai (tính không nhất quán) khi mô hình không khớp: Felsenstein (1978), xem [bài tổng quan về di sản của Felsenstein](https://pmc.ncbi.nlm.nih.gov/articles/PMC7803665); về giới hạn của các phép kiểm định hỗ trợ khi mô hình bị vi phạm: xem nguồn ở phần 9.


---


## 12. Thuật ngữ và tài liệu

Bảng dưới đây xếp theo thứ tự xuất hiện trong bài. Cột cuối cho biết phần giải thích chi tiết, để bạn quay lại khi cần.

| Tiếng Việt | Tiếng Anh | Nghĩa ngắn gọn | Phần |
|---|---|---|---|
| DNA, nucleotide | *DNA, nucleotide* | Phân tử mang thông tin di truyền; chuỗi bốn loại đơn vị A, C, G, T | 1 |
| Gen | *Gene* | Đoạn DNA đảm nhận một chức năng cụ thể | 1 |
| Đột biến | *Mutation* | Thay đổi trong DNA khi sao chép | 1 |
| Thay thế | *Substitution* | Một chữ đổi thành chữ khác; trong mô hình tiến hóa là thay đổi đã phổ biến trong quần thể | 1, 6 |
| Indel | *Insertion/deletion* | Chèn hoặc mất chữ; lý do cần gap trong MSA | 1, 3 |
| Tương đồng | *Homologous* | Cùng bắt nguồn từ một vị trí hoặc gen ở tổ tiên chung | 1 |
| Ortholog, paralog | *Ortholog, paralog* | Gen tương đồng tách ra do tách loài, hoặc do nhân đôi gen | 1 |
| Cây phát sinh loài | *Phylogenetic tree* | Giả thuyết về lịch sử tách nhánh của các loài hoặc chuỗi | 2 |
| Lá, nút trong, nhánh | *Leaf (tip), internal node, branch* | Dữ liệu quan sát; tổ tiên suy luận; dòng dõi nối chúng | 2 |
| Độ dài nhánh | *Branch length* | Số thay thế kỳ vọng trên mỗi vị trí dọc theo nhánh | 2 |
| Gốc, cây không gốc | *Root, unrooted tree* | Tổ tiên chung của mọi lá; cây chỉ có cấu trúc phân nhóm, không có chiều thời gian | 2, 7 |
| Nhóm ngoài | *Outgroup* | Loài chắc chắn nằm ngoài nhóm nghiên cứu, dùng để đặt gốc | 2, 10 |
| Clade | *Clade* | Một tổ tiên và toàn bộ hậu duệ của nó | 2 |
| Topology | *Topology* | Cách phân nhóm của cây, bỏ qua độ dài nhánh | 2 |
| Split | *Split, bipartition* | Cách chia tập lá thành hai nhóm khi cắt một nhánh | 2, 9 |
| Định dạng Newick | *Newick format* | Cách viết cây thành một dòng văn bản bằng dấu ngoặc | 2, 10 |
| Căn chỉnh đa trình tự | *Multiple sequence alignment (MSA)* | Bảng xếp chuỗi sao cho mỗi cột là một vị trí tương đồng | 3 |
| Gap | *Gap* | Ký hiệu `-` biểu diễn giả thuyết có indel | 3, 7 |
| Căn chỉnh lũy tiến, cây dẫn đường | *Progressive alignment, guide tree* | Căn các chuỗi gần nhau trước theo một cây thô | 3 |
| Heuristic | *Heuristic* | Phương pháp cho đáp án tốt trong thời gian hợp lý, không đảm bảo tối ưu | 3, 4, 8 |
| NP-complete, NP-hard | *NP-complete, NP-hard* | Lớp bài toán không có thuật toán nhanh nào được biết để giải chính xác | 3, 4 |
| Cắt tỉa alignment | *Alignment trimming* | Bỏ các cột căn chỉnh kém tin cậy; còn tranh luận | 3 |
| Likelihood | *Likelihood* | Xác suất mà một giá trị tham số gán cho dữ liệu đã quan sát | 5 |
| Hợp lý cực đại | *Maximum likelihood (ML)* | Chọn tham số làm dữ liệu có xác suất cao nhất | 5 |
| Log-likelihood | *Log-likelihood (LogL)* | Logarit của likelihood; số âm, càng gần 0 càng tốt | 5, 10 |
| Mô hình thay thế | *Substitution model* | Quy tắc xác suất cho việc một chữ biến thành chữ khác dọc một nhánh | 6 |
| JC69, HKY, GTR | *JC69, HKY, GTR* | Các mô hình từ đơn giản đến phức tạp | 6 |
| Bão hòa | *Saturation* | Thay đổi chồng lên nhau đến mức mất thông tin lịch sử | 6 |
| Transition, transversion | *Transition, transversion* | Thay thế trong cùng nhóm (A↔G, C↔T), hoặc giữa hai nhóm | 6 |
| +I, +G, +R, +F | *Invariable sites, gamma, FreeRate, empirical frequencies* | Các phần gắn xử lý tốc độ khác nhau giữa vị trí và tần suất chữ | 6 |
| AIC, BIC | *Akaike / Bayesian information criterion* | Tiêu chí chọn mô hình, phạt số tham số; nhỏ hơn là tốt hơn | 6 |
| ModelFinder | *ModelFinder* | Thành phần chọn mô hình của IQ-TREE; mặc định dùng BIC | 6, 10 |
| Nguyên lý ròng rọc | *Pulley principle* | Với mô hình khả nghịch, vị trí gốc không đổi likelihood | 7 |
| Thuật toán cắt tỉa | *Pruning algorithm* | Tính likelihood từ lá lên gốc, mỗi nút một lần (Felsenstein 1981) | 7 |
| Phép biến đổi cây | *Tree rearrangement* | Thao tác nhỏ biến cây này thành cây hàng xóm | 8 |
| NNI, SPR, TBR | *Nearest neighbor interchange, subtree pruning and regrafting, tree bisection and reconnection* | Ba loại phép biến đổi cây, từ nhỏ đến lớn | 8 |
| Leo đồi | *Hill climbing* | Luôn chuyển sang hàng xóm tốt hơn cho đến khi không còn | 8 |
| Cực đại địa phương | *Local optimum* | Tốt hơn mọi hàng xóm nhưng chưa chắc tốt nhất toàn cục | 8 |
| Xáo trộn ngẫu nhiên | *Stochastic perturbation* | Thay đổi ngẫu nhiên để thoát khỏi cực đại địa phương | 8 |
| Tập cây ứng viên | *Candidate tree set* | Nhóm nhỏ cây tốt nhất IQ-TREE giữ trong quá trình tìm | 8 |
| Giá trị hỗ trợ | *Branch support* | Con số đo độ ổn định của một split | 9 |
| Bootstrap | *Bootstrap* | Lấy mẫu lại cột có hoàn lại, dựng lại cây, đếm tỷ lệ | 9 |
| Lấy mẫu có hoàn lại | *Sampling with replacement* | Rút xong thì trả lại, nên một phần tử có thể được rút nhiều lần | 9 |
| Vị trí thông tin | *Parsimony-informative site* | Cột phân biệt được các cây (ít nhất hai chữ, mỗi chữ ở ít nhất hai chuỗi) | 9 |
| UFBoot | *Ultrafast bootstrap* | Xấp xỉ nhanh của bootstrap trong IQ-TREE; tin khi ≥ 95% | 9, 10 |
| SH-aLRT | *SH-like approximate likelihood-ratio test* | Kiểm định nhanh cho từng nhánh; tin khi ≥ 80% | 9, 10 |
| Hút nhánh dài | *Long-branch attraction (LBA)* | Nhánh dài bị gom nhầm vào nhau do thay đổi trùng hợp | 11 |
| Sai số lấy mẫu, sai số hệ thống | *Sampling error, systematic error* | Sai do ít dữ liệu, hoặc do phương pháp và mô hình không khớp | 11 |

### Tài liệu gốc nên đọc tiếp

| Chủ đề | Tài liệu |
|---|---|
| Hướng dẫn chính thức | [IQ-TREE Beginner’s Tutorial](https://iqtree.github.io/doc/Tutorial), [FAQ](https://iqtree.github.io/doc/Frequently-Asked-Questions), [Substitution Models](https://iqtree.github.io/doc/Substitution-Models), [Command Reference](https://iqtree.github.io/doc/Command-Reference). Nên đọc theo đúng thứ tự này. |
| Phần mềm IQ-TREE | Nguyen L.-T. et al. (2015) *Mol. Biol. Evol.* 32:268–274. Minh B.Q. et al. (2020) IQ-TREE 2. *Mol. Biol. Evol.* 37:1530–1534. Wong T.K.F. et al. (2026) IQ-TREE 3. *Mol. Biol. Evol.* 43(5):msag117. |
| Chọn mô hình | Kalyaanamoorthy S. et al. (2017) ModelFinder. *Nat. Methods* 14:587–589. |
| Độ tin cậy | Felsenstein J. (1985) *Evolution* 39:783–791. Minh B.Q. et al. (2013) *Mol. Biol. Evol.* 30:1188–1195. Hoang D.T. et al. (2018) UFBoot2. *Mol. Biol. Evol.* 35:518–522. Anisimova M., Gascuel O. (2006) *Syst. Biol.* 55:539–552. Guindon S. et al. (2010) *Syst. Biol.* 59:307–321. |
| Likelihood trên cây | Felsenstein J. (1981) Evolutionary trees from DNA sequences: a maximum likelihood approach. *J. Mol. Evol.* 17:368–376. |
| MSA | Katoh K., Standley D.M. (2013) MAFFT. *Mol. Biol. Evol.* 30:772–780. Wang L., Jiang T. (1994) *J. Comput. Biol.* 1:337–348. Capella-Gutiérrez S. et al. (2009) trimAl. *Bioinformatics* 25:1972–1973. Tan G. et al. (2015) *Syst. Biol.* 64:778–791. |
| Giới hạn | Bergsten J. (2005) A review of long-branch attraction. *Cladistics* 21:163–193. |

#### 🚩 Trạm dừng cuối: nhìn lại cả hành trình

**Bốn chủ đề thầy giao**

- **Cây phát sinh loài**: giả thuyết về lịch sử tách nhánh, đọc qua tổ tiên chung, lưu dạng Newick.
- **MSA**: bảng các vị trí tương đồng, đầu vào của mọi thứ phía sau.
- **Maximum likelihood**: chọn cây làm dữ liệu có xác suất cao nhất dưới một mô hình thay thế.
- **IQ-TREE**: công cụ gói việc chọn mô hình, tìm cây và đo độ tin cậy vào một lệnh.

**Nếu chỉ nhớ một câu**

Cây là một giả thuyết thống kê: chất lượng của nó phụ thuộc vào alignment, mô hình và chiến lược tìm kiếm, và mỗi nhánh cần được báo cáo kèm con số hỗ trợ của nó.

**Nguồn đã đối chiếu**

- Bài báo IQ-TREE 3: [Mol. Biol. Evol. 43(5):msag117](https://academic.oup.com/mbe/article/43/5/msag117/8669857). IQ-TREE 2: [README IQ-TREE 2](https://github.com/iqtree/iqtree2). Các tài liệu còn lại đã được dẫn nguồn ở các phần tương ứng.


---


*Bài học soạn ngày 18/09/2026. Bốn chuỗi ở đầu trang, năm chuỗi trong phần 3 và mọi ví dụ số là dữ liệu tự tạo để học; các con số đã được tính lại bằng máy. Tùy chọn và tên file của IQ-TREE có thể thay đổi giữa các phiên bản: khi làm đồ án, hãy đối chiếu với tài liệu của đúng phiên bản bạn cài và gõ `iqtree3 -h` để kiểm tra.*
