# 🟨 TUẦN 11: DASHBOARD & CHARTS

## 🎯 Mục tiêu
- Viết API thống kê (Tổng doanh thu, số đơn hàng).
- Vẽ biểu đồ cột/tròn bằng Chart.js trên Vue.

---

## 📈 1. Backend: Statistics API

Ta cần API trả về dữ liệu dạng số liệu để vẽ biểu đồ.

```csharp
[HttpGet("stats")]
public async Task<IActionResult> GetStats()
{
    var totalRevenue = await _context.Orders.SumAsync(o => o.TotalAmount);
    var totalOrders = await _context.Orders.CountAsync();
    var topProducts = await _context.OrderItems
        .GroupBy(x => x.ProductName)
        .Select(g => new { Name = g.Key, Count = g.Sum(x => x.Quantity) })
        .Take(5)
        .ToListAsync();

    return Ok(new { totalRevenue, totalOrders, topProducts });
}
```

---

## 📊 2. Frontend: Chart.js

### 2.1. Cài đặt
```powershell
npm install chart.js vue-chartjs
```

### 2.2. Tạo Component Biểu đồ (`components/RevenueChart.vue`)

```html
<script setup>
import { Bar } from 'vue-chartjs'
import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)

const chartData = {
  labels: ['Tháng 1', 'Tháng 2', 'Tháng 3'],
  datasets: [
    { label: 'Doanh thu', backgroundColor: '#f87979', data: [40, 20, 12] }
  ]
}
</script>

<template>
  <Bar :data="chartData" />
</template>
```

---

## 🧪 3. Thực hành

1. Viết API thống kê đơn giản.
2. Tạo trang `DashboardPage.vue`.
3. Nhúng biểu đồ doanh thu vào Dashboard.
4. Hiển thị các thẻ số liệu (Cards) ở trên cùng: "Tổng thu nhập: 100tr", "Đơn hàng: 50".
