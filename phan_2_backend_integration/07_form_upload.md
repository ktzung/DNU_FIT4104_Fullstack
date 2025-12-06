# 🟩 TUẦN 7: XỬ LÝ FORM & UPLOAD FILE

## 🎯 Mục tiêu
- Xây dựng Form thêm mới sản phẩm trên Vue.
- Xử lý upload ảnh (Multipart/form-data).
- Lưu ảnh vào thư mục server và trả về đường dẫn.

---

## 📤 1. Backend: Upload File

API nhận file phải dùng `IFormFile`.

### 1.1. DTO
```csharp
public class CreateProductWithImageDto
{
    public string Name { get; set; }
    public decimal Price { get; set; }
    public IFormFile? ImageFile { get; set; } // File ảnh gửi lên
}
```

### 1.2. Controller
```csharp
[HttpPost]
public async Task<IActionResult> Create([FromForm] CreateProductWithImageDto request)
{
    string imagePath = null;
    
    if (request.ImageFile != null)
    {
        // 1. Tạo tên file unique
        var fileName = Guid.NewGuid() + Path.GetExtension(request.ImageFile.FileName);
        // 2. Đường dẫn lưu (wwwroot/images)
        var filePath = Path.Combine(Directory.GetCurrentDirectory(), "wwwroot", "images", fileName);
        
        // 3. Lưu file
        using (var stream = new FileStream(filePath, FileMode.Create))
        {
            await request.ImageFile.CopyToAsync(stream);
        }
        
        // 4. Lưu đường dẫn vào DB (VD: /images/abc.jpg)
        imagePath = "/images/" + fileName;
    }

    var product = new Product 
    { 
        Name = request.Name, 
        Price = request.Price, 
        ImageUrl = imagePath 
    };
    
    _context.Products.Add(product);
    await _context.SaveChangesAsync();
    
    return Ok(product);
}
```

### 1.3. Cấu hình Static Files
Để client xem được ảnh, phải bật Static Files trong `Program.cs`:
```csharp
app.UseStaticFiles(); // Cho phép truy cập thư mục wwwroot
```

---

## 📝 2. Frontend: Form Data

Khi gửi file, không thể gửi JSON thường (`application/json`). Phải dùng `FormData`.

```javascript
// productService.js
create(data) {
    // data là object { name: 'A', price: 100, image: File }
    const formData = new FormData();
    formData.append('name', data.name);
    formData.append('price', data.price);
    if (data.image) {
        formData.append('imageFile', data.image);
    }

    return axios.post('/products', formData, {
        headers: { 'Content-Type': 'multipart/form-data' }
    });
}
```

### 2.1. Vue Component (`v-file-input`)

```html
<script setup>
import { ref } from 'vue';

const form = ref({
  name: '',
  price: 0,
  image: null
});

async function submit() {
  await productService.create(form.value);
  alert("Thêm thành công!");
}
</script>

<template>
  <v-form @submit.prevent="submit">
    <v-text-field v-model="form.name" label="Tên SP"></v-text-field>
    <v-text-field v-model="form.price" type="number" label="Giá"></v-text-field>
    <v-file-input v-model="form.image" label="Ảnh đại diện"></v-file-input>
    
    <v-btn type="submit" color="primary">Lưu</v-btn>
  </v-form>
</template>
```

---

## 🧪 3. Thực hành

1. Tạo thư mục `wwwroot/images` trong Backend.
2. Viết API Upload.
3. Tạo Dialog "Thêm mới" trong trang ProductPage của Vue.
4. Test upload ảnh -> Kiểm tra ảnh có xuất hiện trong thư mục server không.
