# 📸 ASSETS IMAGE GUIDE - Hướng dẫn sử dụng ảnh từ Assets

## 🎯 **Tổng quan**

Hệ thống event management hỗ trợ cả ảnh local (từ assets folder) và ảnh external (URLs). Các giải pháp được tích hợp để xử lý lỗi và fallback một cách tự động.

## 📁 **Cấu trúc Ảnh Assets**

```
src/assets/
├── events/
│   ├── event-1.png    # Ảnh sự kiện ID 1
│   ├── event-2.png    # Ảnh sự kiện ID 2
│   ├── event-3.png    # Ảnh sự kiện ID 3
│   ├── event-4.png    # Ảnh sự kiện ID 4
│   ├── event-5.png    # Ảnh sự kiện ID 5
│   ├── event-6.png    # Ảnh sự kiện ID 6
│   ├── event-7.png    # Ảnh sự kiện ID 7
│   └── event-8.png    # Ảnh sự kiện ID 8
├── banner.png
├── logo-school-white.png
├── logo UEL.png
└── eventImages.js     # File tập trung import/export ảnh
```

## 🔧 **Cách sử dụng**

### **1. Sử dụng ảnh local trong eventData.js**

```javascript
// eventData.js
import { getEventImage } from "../assets/eventImages.js";

export const eventData = [
  {
    id: 1,
    title: "Sự kiện ABC",
    // Sử dụng ảnh local theo ID
    image: getEventImage(1),
    // ...
  },
  {
    id: 2,
    title: "Sự kiện XYZ",
    // Sử dụng external URL
    image: "https://example.com/image.jpg",
    // ...
  },
];
```

### **2. Sử dụng EventImage Component**

```javascript
import EventImage from "../components/EventImage";

// Trong component
<EventImage
  event={event}
  alt="Event description"
  fallbackText="Custom fallback text"
  className="custom-class"
  style={{ width: "100%" }}
/>;
```

### **3. Thêm ảnh mới vào assets**

1. **Thêm file ảnh** vào `src/assets/events/event-X.png`
2. **Cập nhật eventImages.js**:

```javascript
// Import ảnh mới
import event9 from './events/event-9.png';

// Thêm vào object eventImages
export const eventImages = {
  1: event1,
  2: event2,
  // ...
  9: event9,  // Ảnh mới
  default: event1,
};

// Export individual
export { event1, event2, ..., event9 };
```

## ✅ **Tính năng xử lý lỗi**

### **Auto Fallback**

- ✅ **External URL fail** → Fallback to SVG placeholder
- ✅ **Local image missing** → Use default local image
- ✅ **No image provided** → Use default image
- ✅ **Network error** → Show fallback với message tùy chỉnh

### **Error Handling Flow**

```
1. Check if event.image exists
   ├─ No → Use getEventImage('default')
   └─ Yes → Continue

2. Check image type
   ├─ External URL (starts with 'http') → Use directly
   └─ Local/imported → Use as-is

3. On image load error
   └─ Show SVG fallback with custom text
```

## 🎨 **Styling và Responsive**

EventImage component tự động áp dụng:

- ✅ **Responsive sizing** (200px → 160px → 140px)
- ✅ **Hover effects** (scale 1.02)
- ✅ **Smooth transitions**
- ✅ **Error state styling**

## 📝 **Best Practices**

### **✅ Nên làm:**

1. **Đặt tên ảnh theo ID**: `event-{id}.png`
2. **Sử dụng EventImage component** thay vì `<img>` tag
3. **Tối ưu kích thước ảnh**: ≤ 500KB
4. **Format phù hợp**: PNG/JPG/WebP
5. **Test cả local và external images**

### **❌ Không nên:**

1. Hardcode đường dẫn ảnh trong components
2. Quên thêm fallback handling
3. Sử dụng ảnh quá lớn (>1MB)
4. Bỏ qua responsive design
5. Quên import CSS cho EventImage

## 🔄 **Migration từ External URLs**

Để chuyển từ external URLs sang local assets:

```javascript
// Trước
image: "https://external-site.com/very-long-url...",

// Sau
image: getEventImage(1),
```

## 🧪 **Testing**

Để test image handling:

```javascript
// Browser console
console.log("Testing image sources:");
eventData.slice(0, 5).forEach((event) => {
  console.log(`Event ${event.id}:`, event.image);
});
```

## 🚀 **Performance Tips**

1. **Lazy loading**: EventImage tự động optimize
2. **Caching**: Browser cache local images
3. **Fallback speed**: SVG fallback load instant
4. **Bundle size**: Local images được optimize bởi Vite

## 📊 **Current Status**

- ✅ **Events 1-3**: Sử dụng local assets
- ✅ **Events 4+**: External URLs (cho demo)
- ✅ **All components**: Có error handling
- ✅ **EventImage**: Component tập trung
- ✅ **CSS**: Responsive styling

---

**🎉 Kết quả**: Hệ thống image robust, responsive và user-friendly với fallback handling hoàn chỉnh!
