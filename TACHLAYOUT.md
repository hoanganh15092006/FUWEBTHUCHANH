# Hướng dẫn kết nối DB

## Bước 1: Lấy connection string

```
Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=QLGiaiBongDa;Integrated Security=True;Connect Timeout=30;Encrypt=True;Trust Server Certificate=False;Application Intent=ReadWrite;Multi Subnet Failover=False;Command Timeout=30
```

## Bước 2: Điền connection string vào ""

```
Scaffold-DbContext "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=QLGiaiBongDa;Integrated Security=True;Connect Timeout=30;Encrypt=True;Trust Server Certificate=False;Application Intent=ReadWrite;Multi Subnet Failover=False;Command Timeout=30" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

vào manage package console và chạy lệnh đó

# HƯỚNG DẪN TÁCH HEADER VÀ FOOTER THÀNH PARTIAL VIEW TRONG ASP.NET CORE MVC

Hướng dẫn này giúp bạn cắt phần **Header** và **Footer** từ file Layout chung (`_Layout.cshtml`), đưa vào các file Partial View riêng biệt trong thư mục `Views/Shared` để mã nguồn gọn gàng và dễ bảo trì.

---

## BƯỚC 1: TẠO CÁC FILE PARTIAL VIEW TRONG SOLUTION EXPLORER

1. Trong **Solution Explorer**, tìm và mở thư mục **`Views`** -> chuột phải vào **`Shared`**.
2. Chọn **Add** -> **New Item...** (hoặc **View...**).
3. Chọn **Razor View - Empty** (hoặc **Razor View**):
   - Đặt tên file Header: `_Header.cshtml` -> Nhấn **Add**.
4. Lặp lại thao tác trên cho file Footer:
   - Đặt tên file Footer: `_Footer.cshtml` -> Nhấn **Add**.

> **Lưu ý:** Tên Partial View thường bắt đầu bằng dấu gạch dưới `_` để phân biệt với View thông thường.

---

## BƯỚC 2: CẮT MÃ HTML CỦA HEADER VÀ FOOTER

### 1. Cắt Header

- Mở file layout chính (ví dụ: `_Layout.cshtml`).
- Tìm toàn bộ khối mã HTML của Header (thường bắt đầu từ thẻ `<header>` đến `</header>`).
- **Cut (Ctrl + X)** đoạn mã đó và **Paste (Ctrl + V)** vào file `_Header.cshtml` vừa tạo.
- Lưu file `_Header.cshtml` (`Ctrl + S`).

### 2. Cắt Footer

- Quay lại file `_Layout.cshtml`, tìm toàn bộ khối mã HTML của Footer (thường bắt đầu từ thẻ `<footer>` đến `</footer>`).
- **Cut (Ctrl + X)** đoạn mã đó và **Paste (Ctrl + V)** vào file `_Footer.cshtml` vừa tạo.
- Lưu file `_Footer.cshtml` (`Ctrl + S`).

---

## BƯỚC 3: NHÚNG PARTIAL VIEW VÀO FILE LAYOUT CHÍNH (`_Layout.cshtml`)

Tại những vị trí bạn vừa cắt mã HTML trong `_Layout.cshtml`, hãy thay thế bằng câu lệnh gọi Partial View tương ứng:
Giữ lại @{ViewData}

```html
@{
    ViewData["Title"] = "Home Page"; 
}

 <!-- Single Product Area -->
 <div class="col-12 col-sm-6 col-lg-4">
```

```
Cách gọi khác bằng Tag Helper: Bạn cũng có thể dùng cú pháp 
<partial name="_Header" /> và <partial name="_Footer" />.
```

# Tạo components (hiển thị ... cho phần )

## Bước 1: Tạo folder ViewComponents

Trong thư mục dự án tạo ViewComponents->add class vào : ViewComponents/ten_filecomponents

```
using Microsoft.AspNetCore.Mvc;
using NguyenHoangAnh_242624495.Models;
namespace NguyenHoangAnh_242624495.ViewCoponents
{
    public class CLBMenu : ViewComponent
    {
        private readonly QlgiaiBongDaContext _context;

        public CLBMenu(QlgiaiBongDaContext context)
        {
            this._context = context;
        }
        public IViewComponentResult Invoke()
        {
            var CLB = _context.Caulacbos.ToList(); //Lấy toàn bộ clb
            return View(CLB);
        }
    }
}
```

Trong program.cs

```

builder.Services.AddDbContext<QlgiaiBongDaContext>(x => x.UseSqlServer(builder.Configuration.GetConnectionString("QlgiaiBongDaContext")));
```

Sửa QlgiaiBongDaContext thành DB mình sử dụng

## Bước 2: Add vào Shared 1 folder tên Components

Trong Share/Components add folder tên giống với file đã tạo ở bước 1 ten_filecomponents

trong Share/Components/ten_filecomponents add Razor View - Empty file Default.cshtml

Lấy trong _Layout phần mình muốn để thành menu, xóa ở trong _Layout đi và chuyển thành: dán vào Defalut.cshtml

trong _Layout:

```
@await Component.InvokeAsync("ten_filecomponents");
```

trong Defalt.cs

```
@using tenduan.Models
@model IEnumerable<Ten_bang_trongModels>
//Lấy 10
@foreach(var item in Model.Take(10)){
    <li><a href="#">@item.TenClb</a></li>
}

Lấy hết
@foreach(var item in Model){
    <li><a href="#">@item.TenClb</a></li> //TenClb có thể thay bằng thứ mà bạn muốn đặt vào menu
}
```

# Add sản phẩm

## Bước 1: Lấy phần ghi sản phẩm trong _Layout, Xóa phần đó và cho vào index

```
Hầu như làm hết từ bước tách header và footer
```

## Bước 2: Vào HomeController

khai báo private readonly Tencsdlcontext _context;

```
using HAluyentap.Models;
using Microsoft.AspNetCore.Mvc;
using System.Diagnostics;

namespace HAluyentap.Controllers
{
    public class HomeController : Controller
    {
        private readonly QlgiaiBongDaContext _context; //phan them

        public HomeController(QlgiaiBongDaContext context) { this._context = context; }  //phan them
        public IActionResult Index()
        {
            var Cauthu = _context.Cauthus.OrderBy(x => x.CauLacBo).ToList(); //phan them
            // có thể (x => x.CauLacBo == "")
            return View(Cauthu); //phan them
        }
        public IActionResult Privacy()
        {
            return View();
        }

        [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
        public IActionResult Error()
        {
            return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
        }
    }
}
```

Trong Index.cshtml thêm

```
@using HAluyentap.Models;  //HAluyentap là tên dự án có thể thay
@model IEnumerable<Cauthu>;//Cauthu là tên trong bảng BảngModel
```

```
@using HAluyentap.Models;
@model IEnumerable<Cauthu>;
@{
    ViewData["Title"] = "Home Page";
}
@foreach(var item in Model){
     <div class="col-12 col-sm-6 col-lg-4">
     <div class="single-product-area mb-50">
         <!-- Product Image -->
         <div class="product-img">
             <a href="shop-details.html"><img src="~/Images/@item.Anh" alt=""></a> 
             <!-- Product Tag -->
             <div class="product-tag">
                 <a href="#">Hot</a>
             </div>
             <div class="product-meta d-flex">
                 <a href="#" class="wishlist-btn"><i class="icon_heart_alt"></i></a>
                 <a href="cart.html" class="add-to-cart-btn">Add to cart</a>
                 <a href="#" class="compare-btn"><i class="arrow_left-right_alt"></i></a>
             </div>
         </div>
         <!-- Product Info -->
         <div class="product-info mt-15 text-center">
             <a href="shop-details.html">
                 <p>@item.HoVaTen</p>
             </a>
             <h6>@item.CauLacBo</h6>
         </div>
     </div>
 </div>
}

 //<a href="shop-details.html"><img  src="@Url.Content("~/Images/" + item.Anh)" alt=""></a>
```

## Phân trang:Trong HomeController

```
using HAluyentap.Models;
using Microsoft.AspNetCore.Mvc;
using System.Diagnostics;
using X.PagedList;

namespace HAluyentap.Controllers
{
    public class HomeController : Controller
    {
        private readonly QlgiaiBongDaContext _context;

        public HomeController(QlgiaiBongDaContext context) { this._context = context; } 
        public IActionResult Index(int? page )//
        {
            int pageSize = 4;//
            int pageNumber = page == null || page < 1 ? 1 : (int)page;//
            var CauThu = _context.Cauthus.OrderBy(x => x.CauLacBo).ToList();//

            PagedList<Cauthu> pageCauthu = new PagedList<Cauthu>(CauThu, pageNumber, pageSize);//

            return View(pageCauthu);//
        }
        public IActionResult Privacy() 
        {
            return View();
        }

        [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
        public IActionResult Error()
        {
            return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
        }
    }
}

}
```

Thêm xuống dưới cùng file index

```
<div class="text-center mt-4">
    @if (Model.HasPreviousPage)
    {
        <a href="@Url.Action("Index", new { page = Model.PageNumber - 1 })" class="btn btn-primary me-2"> Trang trước</a>
    }

    <span>Trang @Model.PageNumber / @Model.PageCount</span>

    @if (Model.HasNextPage)
    {
        <a href="@Url.Action("Index", new { page = Model.PageNumber + 1 })" class="btn btn-primary ms-2">Trang sau </a>
    }
</div>//Nút phân trang không có số
```

Nút phân trang có số

```
<!-- Nút chuyển trang có bấm chọn số trang -->
<div class="text-center mt-4">
    <!-- Nút Trang trước -->
    @if (Model.HasPreviousPage)
    {
        <a href="@Url.Action("Index", new { page = Model.PageNumber - 1 })" class="btn btn-outline-primary me-1">‹ Prev</a>
    }

    <!-- Danh sách nút bấm theo số trang -->
    @for (int i = 1; i <= Model.PageCount; i++)
    {
        if (i == Model.PageNumber)
        {
            <span class="btn btn-primary active me-1">@i</span>
        }
        else
        {
            <a href="@Url.Action("Index", new { page = i })" class="btn btn-outline-primary me-1">@i</a>
        }
    }

    <!-- Nút Trang sau -->
    @if (Model.HasNextPage)
    {
        <a href="@Url.Action("Index", new { page = Model.PageNumber + 1 })" class="btn btn-outline-primary ms-1">Next ›</a>
    }
</div>
```

sửa đầu

```
@using HAluyentap.Models;
@model IEnumerable<Cauthu>;
```

Thành

```
@using HAluyentap.Models;
@using X.PagedList;
@model IPagedList<Cauthu>
```

# AJAX

## Bước 1:Vào controller add chọn API controller-empty

đặt tên tùy thích

thêm những thứ này vào file

```
using HAluyentap.Models;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using HAluyentap.Models;
```

```
using HAluyentap.Models;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;
using HAluyentap.Models;

namespace HAluyentap.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class CauThuTheoCLB : ControllerBase
    {

        private readonly QlgiaiBongDaContext _context;

        public CauThuTheoCLB(QlgiaiBongDaContext context)
        {
            this._context = context;
        }

        [HttpGet]
        public async Task<ActionResult<IEnumerable<Cauthu>>> laytatca()
        {
            var CauThu = await _context.Cauthus.ToListAsync();

            return CauThu;
        }

        [HttpGet("{IdCauLacBo}")]
        public async Task<ActionResult<IEnumerable<Cauthu>>> laytatca(string IdCauLacBo)
        {
            var CauThu = await _context.Cauthus
                                       .Where(x => x.CauLacBoId == IdCauLacBo)
                                       .ToListAsync();

            return CauThu;
        }
    }
}
```

<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>

<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>

ở giữa body và head

<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>

thêm

```
<script src="https://code.jquery.com/jquery-3.6.0.min.js"></script>
```

## Bước 2 trong default

```
@using De04.Models
@model IEnumerable<Huanluyenvien>

@foreach(var item in Model.Take(6)){
    string getURL = "https://localhost:7143/api/Values/" + item.HuanLuyenVienId;
    <li><a href="#" style="cursor:pointer" onclick="showCauThuTheoHLV('@getURL')">@item.TenHlv</a></li>
  
}

<script>
       function showCauThuTheoHLV(getURL){
           var str = "";
           $.ajax({
               type: 'GET',
               url: getURL,
               dataType: 'json',
               success:
                   function(data){
                       $.each(
                           data,
                           function(key, val){
                               str += `
                                    <div class="col-12 col-sm-6 col-lg-3">
        <div class="single-product-area mb-50">
            <!-- Product Image -->
            <div class="product-img">
                <a href="shop-details.html"><img src="ImagesBaitap/${val.anh}" alt="${val.hoVaTen || ''}"></a  
                <div class="product-meta d-flex">
                    <a href="#" class="wishlist-btn"><i class="icon_heart_alt"></i></a>
                    <a href="cart.html" class="add-to-cart-btn">Add to cart</a>
                    <a href="#" class="compare-btn"><i class="arrow_left-right_alt"></i></a>
                </div>
            </div>
            <!-- Product Info -->
            <div class="product-info mt-15 text-center">
                <a href="#">
                    <p>${val.hoVaTen || ''}</p>
                </a>
                <h6>${val.cauThuId || val.cauThuID || ''}</h6>
            </div>
        </div>
    </div>
                               `;
                           }
                       );
                       $('#displayCauThu').html(str);
                   },

               error:
                   function(err){
                       alert(err.responseText)
                   }
           });
       }
</script>
```

để ý đoạn này:

```
                       $('#displayCauThu').html(str);
```

dùng đoạn đó để id vào class ví dụ:

```
 <div class="center_content" id="displayCauThu">
   <div class="center_title_bar">Latest Products</div>
   @RenderBody();
 </div>
```




# Xoa,them,sua

## Xóa SP

trong homecontroller

```
[HttpGet]

public IActionResult XoaCauThu(string idcauthu)
{
    var CauThu = _context.Cauthus.FirstOrDefault(x => x.CauThuId == idcauthu);

    bool daThamGiatrandau = _context.TrandauCauthus.Any(x => x.CauThuId == idcauthu);

        if (daThamGiatrandau)
    {
        TempData["XoaThatBai"] = "Khong the xoa vi da tham gia tran dau";
    }
    else
    {
        _context.Cauthus.Remove(CauThu);
        _context.SaveChanges();
        TempData["XoaThanhCong"] = "Xóa cầu thủ thành công!";

    }
    return RedirectToAction("Index");
}
```

trong index

```
@if (TempData["XoaThatBai"] != null)
{
    <script>
        alert('@Html.Raw(TempData["XoaThatBai"])');
    </script>
}


@if (TempData["XoaThanhCong"] != null)
{
    <script>
        alert('@Html.Raw(TempData["XoaThanhCong"])');
    </script>
}
```

button trong index

```
<button> <a asp-action="XoaCauThu" asp-route-idcauthu="@item.CauThuId">Xoa</a></button>
```

## Sửa

Tải 2 package này trong developer powershell

```
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
```


cd vào đúng thư mục dự án

dùng lệnh này trong developer powershell

```
dotnet aspnet-codegenerator view SuaCauThu Edit -m Cauthu -dc QlgiaiBongDaContext -outDir Views/Home
```


| Phần                       | Ý nghĩa                                                                                                               |
| --------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `dotnet`                  | Chạy công cụ .NET CLI                                                                                                |
| `aspnet-codegenerator`    | Công cụ**Scaffolding**của ASP.NET Core, giúp tự sinh code                                                    |
| `view`                    | Yêu cầu tạo**View**                                                                                            |
| `SuaCauThu`               | Tên View muốn tạo → thường sẽ tạo`SuaCauThu.cshtml` và phải giống tên phương thức trong homecontroller |
| `Edit`                    | Chọn**template Edit**→ View dùng để sửa dữ liệu                                                           |
| `-m Cauthus`              | `-m`= **Model** . View sẽ sử dụng model`Cauthus`                                                           |
| `-dc QlgiaiBongDaContext` | `-dc`= **Data Context** . Sử dụng`QlgiaiBongDaContext`để kết nối database                               |
| `-outDir Views/Home`      | Nơi lưu View được tạo                                                                                             |

trong homecontroller

```
[HttpGet]
public IActionResult SuaCauThu(string idcauthu) 
{
    var cauThu = _context.Cauthus.FirstOrDefault(x => x.CauThuId == idcauthu);
    ViewBag.CauLacBoId = new SelectList(_context.Caulacbos, "CauLacBoId", "TenClb"); 


    return View(cauThu);
}

[HttpPost]

public IActionResult SuaCauThu(Cauthu idcauthu)
{
    if (ModelState.IsValid)
    {
        _context.Cauthus.Update(idcauthu);
        _context.SaveChanges();

        return RedirectToAction("Index");
    }
    return View(idcauthu);
}
```

selectlist theo viewbag trong

```
<select asp-for="CauLacBoId" class ="form-control" asp-items="ViewBag.CauLacBoId"></select>
```

## Thêm

cd vào đúng thư mục dự án

dùng lệnh này trong developer powershell

```
dotnet aspnet-codegenerator view ThemCauThu Create -m Cauthu -dc QlgiaiBongDaContext -outDir Views/Home
```

trong homecontroller 

```
[HttpGet]
public IActionResult ThemMoi()
{
  
    ViewBag.CauLacBoId = new SelectList(_context.Caulacbos, "CauLacBoId", "TenClb");
    return View();
}

[HttpPost]
public IActionResult ThemMoi(Cauthu cauthu)
{
    if (ModelState.IsValid)
    {
        _context.Cauthus.Add(cauthu);
        _context.SaveChanges();

        return RedirectToAction("Index");
    }

  
    ViewBag.CauLacBoId = new SelectList(_context.Caulacbos, "CauLacBoId", "TenClb", cauthu.CauLacBoId);
    return View(cauthu);
}
```



selectlist theo viewbag trong

```
<select asp-for="CauLacBoId" class ="form-control" asp-items="ViewBag.CauLacBoId"></select>
```

# Các kiểu validate

### chỉ đc nhập chữ cái

```
[RegularExpression(@"^[a-zA-ZàáảãạâầấẩẫậăằắẳẵặèéẻẽẹêềếểễệđìíỉĩịòóỏõọôồốổỗộơờớởỡợùúủũụưừứửữựỳýỷỹỵÀÁẢÃẠÂẦẤẨẪẬĂẰẮẲẴẶÈÉẺẼẸÊỀẾỂỄỆĐÌÍỈĨỊÒÓỎÕỌÔỒỐỔỖỘƠỜỚỞỠỢÙÚỦŨỤƯỪỨỬỮỰỲÝỶỸỴ\s]+$", ErrorMessage = "Quốc tịch chỉ được nhập chữ cái!")]
```

### ảnh phải có đuôi

```
[RegularExpression(@"^.+\.(jpg|png|jpeg)$", ErrorMessage = "Tên file ảnh phải có đuôi .jpg, .png hoặc .jpeg!")]
```

### không để trống

```
[Required(ErrorMessage = "Họ tên không được để trống")]
```

### Giới hạn độ dài

```
[StringLength(50, MinimumLength = 3,
    ErrorMessage = "Họ tên phải từ 3 đến 50 ký tự")]
```

### Chỉ cho số nguyên

```
[Range(1, 100, ErrorMessage = "Tuổi phải từ 1 đến 100")]
```

### Số tiền / điểm trong khoảng

```
[Range(0, 100000000,
    ErrorMessage = "Giá phải từ 0 đến 100 triệu")]
```

### Email

```
[RegularExpression(
    @"^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$",
    ErrorMessage = "Email không hợp lệ"
)]
```

### Chỉ nhập chữ cái

```
[RegularExpression(@"^[a-zA-ZÀ-ỹ\s]+$",
    ErrorMessage = "Chỉ được nhập chữ cái")]
public string HoTen { get; set; }
```
