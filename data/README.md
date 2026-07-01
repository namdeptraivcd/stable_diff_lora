# Dataset Itay cho LoRA

Thư mục `data/Itay/` chứa tập ảnh nguồn dùng để fine-tune Stable Diffusion bằng
LoRA. Mục tiêu của thí nghiệm là tạo checkpoint có khả năng tái hiện chủ thể
giống Itay nhất có thể, đồng thời vẫn giữ được khả năng làm theo prompt và tạo
ảnh trong bối cảnh mới.

> Chỉ sử dụng dữ liệu khi đã có quyền sử dụng và sự đồng ý của người trong ảnh.
> Không commit, upload hoặc chia sẻ ảnh cá nhân, embedding khuôn mặt và checkpoint
> LoRA nếu chưa được cho phép rõ ràng.

## Chuẩn bị dữ liệu

`data/Itay/` hiện là tập ảnh nguồn. Theo yêu cầu Assignment 2.5, mỗi lần training
chỉ được sử dụng **5–10 ảnh tự thu thập**. Vì vậy không train trực tiếp trên toàn
bộ ảnh nếu thư mục có nhiều hơn 10 ảnh.

Hãy chọn một subset 5–10 ảnh đáp ứng các tiêu chí sau:

- Khuôn mặt rõ, đúng nét và không bị che quá nhiều.
- Đa dạng góc chụp, biểu cảm, ánh sáng, trang phục và phông nền.
- Không chứa ảnh trùng hoặc các frame gần như giống nhau.
- Hạn chế ảnh có người khác xuất hiện cùng chủ thể.
- Không dùng ảnh của người nổi tiếng hoặc nhân vật công chúng.

Các ảnh không dùng để train có thể làm tập reference/hold-out cho CLIP-I. Không
dùng chính ảnh train làm toàn bộ tập đánh giá vì kết quả sẽ thiên lệch.

Trước khi training, tạo một thư mục subset cục bộ, ví dụ:

```text
data/
├── Itay/                 # tập ảnh nguồn
├── Itay_train/           # 5–10 ảnh được chọn để train
└── Itay_reference/       # ảnh hold-out dùng cho CLIP-I
```

Sau đó cập nhật danh sách `TRAIN_FILES` và `REFERENCE_FILES` trong cell cấu hình
của `notebooks/assignment2_5.ipynb`. Các thư mục ảnh cá nhân phải được giữ ngoài
Git.

## Caption và subject token

Mỗi ảnh có thể có một file caption cùng tên:

```text
data/Itay_train/itay_01.jpg
data/Itay_train/itay_01.txt
```

Sử dụng một subject token riêng, không chứa tên thật, chẳng hạn `sks_subject`.
Caption nên mô tả đúng nội dung ảnh và luôn chứa subject token cùng class word:

```text
a photo of sks_subject man wearing a blue shirt, outdoors, natural light
```

Không đưa thông tin riêng tư vào caption. Nên xóa EXIF không cần thiết, đặc biệt
là vị trí GPS, khỏi bản sao dùng để training.

## Kế hoạch thí nghiệm

Không đánh giá checkpoint chỉ bằng training loss. Giữ cố định prompt validation
và seed, lưu checkpoint trung gian, sau đó so sánh các cấu hình LoRA có kiểm soát:

- Rank và alpha, ví dụ `4/4`, `8/8`, `16/16`.
- Learning rate và số training steps.
- LoRA strength khi inference.
- Chính sách crop/resize và caption.

Mỗi lần chỉ thay đổi một nhóm tham số. Ghi lại base model, seed, resolution,
precision, optimizer, learning rate, rank, alpha, batch size, gradient
accumulation và checkpoint được chọn.

Kết quả tham khảo về chất lượng tái hiện khuôn mặt:

- [Comparison: Astria, Kohya, Headshot, PhotoAI](https://www.notion.so/Comparison-Astria-Kohya-Headshot-PhotoAI-1851151613a64ccbb1f6843c11f515ee?pvs=21)

Liên kết trên chỉ là tài liệu tham khảo định tính; checkpoint cuối cùng phải được
chọn từ kết quả thực nghiệm trên dataset Itay.

## Tiêu chí chọn checkpoint

Đánh giá base model và từng checkpoint LoRA bằng cùng prompt, seed, sampler,
guidance scale, số steps và resolution.

- **CLIP-I:** đo độ tương đồng giữa ảnh sinh ra và ảnh reference của Itay.
- **CLIP-T:** đo mức độ phù hợp giữa ảnh sinh ra và prompt.
- **Đánh giá trực quan:** khuôn mặt, đặc điểm nhận dạng, màu da, tóc, hình học,
  artifact, độ đa dạng và khả năng thay đổi bối cảnh/trang phục.

Checkpoint tốt không chỉ có CLIP-I cao. Nếu ảnh giống chủ thể nhưng bỏ qua prompt,
lặp lại ảnh train hoặc mất tính đa dạng thì checkpoint đó đã bị overfit.

## Inference checkpoint

Sau khi chọn checkpoint tốt nhất, thực hiện inference theo hai cách:

### Diffusers

Sử dụng code inference thủ công của project, không dùng high-level pipeline.
Chương trình phải load base model và LoRA, encode prompt/null prompt, thực hiện
hai U-Net forward pass cho mỗi timestep, áp dụng CFG, chạy DDIM scheduler và
decode latent bằng VAE.

Để tạo bộ ảnh base/LoRA có cùng prompt và seed, chạy phần **Generate matched base
and LoRA images** trong `notebooks/assignment2_5.ipynb`. Nhớ cập nhật checkpoint,
LoRA strength và guidance scale tốt nhất trước khi chạy.

### ComfyUI

Copy checkpoint LoRA đã train vào `ComfyUI/models/loras/`, sau đó import workflow
trong `workflows/`. Workflow cần có checkpoint loader, LoRA loader, positive và
negative prompt, sampler, VAE decoder và image output.

So sánh `workflows/baseline.json` với `workflows/optimized.json` bằng cùng prompt
và seed. Ghi lại thời gian chạy, phần cứng, memory nếu có, resolution và nhận xét
chất lượng. Không kết luận workflow nhanh hơn hoặc đẹp hơn nếu chưa đo thực tế.
