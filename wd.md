npm run dev
# 📚 HƯỚNG DẪN CHI TIẾT DỰ ÁN WEATHER DASHBOARD
## Dành cho người mới bắt đầu

---

## 📖 MỤC LỤC

1. [Tổng Quan Dự Án](#1-tổng-quan-dự-án)
2. [Cài Đặt & Chạy](#2-cài-đặt--chạy)
3. [Cấu Trúc Thư Mục](#3-cấu-trúc-thư-mục)
4. [Giải Thích Tech Stack](#4-giải-thích-tech-stack)
5. [Giải Thích Code Chi Tiết](#5-giải-thích-code-chi-tiết)
6. [Testing - Kiểm Thử](#6-testing---kiểm-thử)
7. [Xử Lý API & Geolocation](#7-xử-lý-api--geolocation)
8. [Data Visualization](#8-data-visualization)
9. [Câu Hỏi Thường Gặp](#9-câu-hỏi-thường-gặp)
10. [Tips Cho Phỏng Vấn](#10-tips-cho-phỏng-vấn)

---

## 1. TỔNG QUAN DỰ ÁN

### 1.1 Dự án này là gì?

Đây là **ứng dụng dashboard thời tiết** hiển thị dữ liệu real-time. User có thể:
- 🔍 Tìm kiếm thành phố bất kỳ trên thế giới
- 📍 Dùng vị trí hiện tại thông qua Geolocation API
- 🌡️ Xem thời tiết hiện tại (nhiệt độ, độ ẩm, gió, áp suất)
- 📊 Theo dõi biểu đồ nhiệt độ theo giờ (24 giờ tiếp theo)
- 📅 Xem dự báo 7 ngày với icon điều kiện thời tiết
- 🌓 Bật/tắt dark mode, lưu lại lựa chọn
- 💾 Nhớ thành phố cuối cùng trong localStorage

### 1.2 Tại sao làm dự án này?

**Mục đích cho CV**:
- ✅ Chứng minh khả năng tích hợp REST API phức tạp
- ✅ Thể hiện năng lực trực quan hóa dữ liệu với chart
- ✅ Sử dụng browser APIs (geolocation, localStorage)
- ✅ Trình bày UX polished: gradient, loading, error banner
- ✅ Thể hiện kỹ năng Tailwind + TypeScript nâng cao

### 1.3 Demo & Repo

- Live demo: https://weather-dashboard-blue-chi.vercel.app
- Repository: https://github.com/nbv9704/weather-dashboard
- API provider: https://www.weatherapi.com (tài khoản miễn phí)

---

## 2. CÀI ĐẶT & CHẠY

### 2.1 Yêu cầu

- **Node.js** 18+ – [Download](https://nodejs.org)
- **Git** – [Download](https://git-scm.com)
- Tài khoản WeatherAPI để lấy API key
- Gợi ý tool: VS Code + Tailwind CSS IntelliSense

### 2.2 Các bước cài đặt

#### Bước 1: Clone project
```bash
git clone https://github.com/nbv9704/weather-dashboard.git
cd weather-dashboard
```

#### Bước 2: Cài dependencies
```bash
npm install
```

#### Bước 3: Cấu hình `.env`
```bash
copy .env.example .env       # Windows
# cp .env.example .env       # macOS / Linux
```

Mở file `.env`, thay `your_api_key_here` bằng API key từ WeatherAPI.

#### Bước 4: Chạy app
```bash
npm run dev
```

- Server chạy tại `http://localhost:5173`
- Mọi thay đổi sẽ được hot reload

### 2.3 Các lệnh quan trọng

```bash
npm run build    # Kiểm tra type + build production
npm run preview  # Preview build production
npm run lint     # Chạy ESLint
```

**Lưu ý:** `.env` đã được ignore, không đẩy API key lên GitHub.

---

## 3. CẤU TRÚC THƯ MỤC

### 3.1 Tổng quan

```
weather-dashboard/
├── public/
├── src/
│   ├── assets/
│   ├── components/
│   │   ├── CurrentWeather.tsx
│   │   ├── DailyForecast.tsx
│   │   ├── Header.tsx
│   │   ├── HourlyChart.tsx
│   │   ├── LoadingSpinner.tsx
│   │   ├── SearchBar.tsx
│   │   └── WeatherStats.tsx
│   ├── hooks/
│   │   ├── useLocalStorage.ts
│   │   ├── useTheme.ts
│   │   └── useWeather.ts
│   ├── services/
│   │   └── weatherApi.ts
│   ├── types/
│   │   └── weather.ts
│   ├── utils/
│   │   └── formatters.ts
│   ├── App.tsx
│   ├── main.tsx
│   ├── App.css
│   └── index.css
├── package.json
├── tailwind.config.js
├── postcss.config.js
└── vite.config.ts
```

### 3.2 Giải thích

- **components/**: Chia theo từng block UI của dashboard (header, search, stats, chart...).
- **hooks/**: Gói gọn logic phức tạp (fetch dữ liệu, lưu theme, localStorage).
- **services/**: Chứa toàn bộ call đến WeatherAPI, dễ dàng maintain.
- **types/**: Định nghĩa dạng dữ liệu trả về để tránh sai sót.
- **utils/**: Hàm format giờ, nhiệt độ, gió giúp code gọn.

---

## 4. GIẢI THÍCH TECH STACK

### 4.1 React 19

- Component-based, dễ tách logic giữa các phần (SearchBar, Chart, Stats).
- Hooks (`useState`, `useEffect`, custom hooks) xử lý bất đồng bộ và side effect.

### 4.2 TypeScript

- WeatherAPI trả về JSON nhiều tầng → cần type để tránh sai key.
- IDE autocomplete chuẩn, giúp dev nhanh và ít bug runtime.

### 4.3 Tailwind CSS 3

- Layout gradient, card, blur… chỉ với utility classes.
- Dark mode implement đơn giản thông qua class `dark` trên `<html>`.

### 4.4 Axios

- Tạo instance chung, auto gắn `key` và `baseURL`.
- Dễ cấu hình interceptors nếu cần logging hoặc retry.

### 4.5 Recharts

- Biểu đồ responsive, declarative, tích hợp tốt với React.
- Hỗ trợ tooltip custom, gradient fill, responsive container.

### 4.6 date-fns

- Format thời gian, chuyển múi giờ, tính chênh lệch đơn giản.

### 4.7 Vite + ESLint

- Build siêu nhanh, HMR mượt, config ngắn gọn.
- ESLint giữ code style thống nhất, tránh quên dependency trong hooks.

---

## 5. GIẢI THÍCH CODE CHI TIẾT

### 5.1 Entry Point: `main.tsx`

```typescript
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.tsx'
import './index.css'

ReactDOM.createRoot(document.getElementById('root')!).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
)
```

- Mount `<App />` vào DOM, bật `StrictMode` để cảnh báo side effect.

### 5.2 Component Chính: `App.tsx`

```tsx
const { isDark, toggleTheme } = useTheme();
const { weather, loading, error, fetchWeather } = useWeather();

const handleUseLocation = () => {
  if (!navigator.geolocation) return;
  navigator.geolocation.getCurrentPosition(
    ({ coords }) => fetchWeather(`${coords.latitude},${coords.longitude}`),
    () => console.error('Unable to get location')
  );
};
```

- `useTheme`: đồng bộ theme giữa localStorage và class `dark`.
- `useWeather`: giữ state `weather`, `loading`, `error`, và expose `fetchWeather`.
- `handleUseLocation`: Gọi geolocation → fetch bằng toạ độ `lat,long`.

Render chính:

```tsx
{loading && <LoadingSpinner />}

{error && (
  <div className="bg-red-500/80 ...">
    <p className="font-semibold">❌ {error}</p>
  </div>
)}

{!loading && weather && (
  <div className="space-y-6">
    <CurrentWeather weather={weather} />
    <WeatherStats weather={weather} />
    <HourlyChart weather={weather} />
    <DailyForecast weather={weather} />
  </div>
)}
```

- Hiển thị spinner khi đang load, banner khi lỗi, và dashboard khi có dữ liệu.

### 5.3 Hook `useWeather.ts`

```typescript
useEffect(() => {
  const lastCity = localStorage.getItem('lastCity') || 'Hanoi';
  fetchWeather(lastCity);
}, []);

const fetchWeather = async (query: string) => {
  setLoading(true);
  setError(null);
  try {
    const data = await getWeather(query);
    setWeather(data);
    localStorage.setItem('lastCity', data.location.name);
  } catch (err: any) {
    setError(err.message || 'Không lấy được dữ liệu');
  } finally {
    setLoading(false);
  }
};
```

- Lần đầu load → lấy city cuối cùng hoặc default “Hanoi”.
- Khi fetch thành công → lưu tên city chuẩn hoá (viết hoa đúng chuẩn).
- Khi lỗi → set message để show ở banner.

### 5.4 Service `weatherApi.ts`

- Tạo axios instance với `baseURL` + `key` chung.
- Endpoint sử dụng: `forecast.json?days=7&aqi=no&alerts=no`.
- Map data trả về thành `WeatherData` (location, current, forecast).

### 5.5 Components chính

- `SearchBar`: Gồm input + nút “Use my location”, debounce nhẹ.
- `CurrentWeather`: Hiển thị nhiệt độ hiện tại, cảm giác như, điều kiện.
- `WeatherStats`: Cards nhỏ (UV, gió, áp suất, tầm nhìn…)
- `HourlyChart`: Biểu đồ AreaChart cho 24 giờ tới.
- `DailyForecast`: Grid 7 ngày, icon điều kiện, min/max.

### 5.6 Utils formatters

```typescript
export const formatTemperature = (value: number) => `${Math.round(value)}°C`;
export const formatLocalTime = (date: string, tz: string) =>
  formatInTimeZone(new Date(date), tz, 'HH:mm');
```

- Tách logic format giúp component gọn, dễ test.

### 5.7 Types `weather.ts`

- `WeatherData`, `Location`, `Current`, `ForecastDay`, `Hour`.
- Khi truy cập `weather.forecast.forecastday[0]`, TypeScript bảo vệ khỏi `undefined`.

---

## 6. TESTING - KIỂM THỬ

### 6.1 Hiện trạng

- Chưa có test tự động ⇒ cơ hội tốt để nâng portfolio.

### 6.2 Gợi ý thêm test

1. **Utils**: Test `formatTemperature`, `formatLocalTime`, `formatWind` với dữ liệu edge case (âm, >100 km/h).
2. **Hook `useWeather`**: Dùng `vi.spyOn` mock `getWeather`, kiểm tra `loading`, `error`.
3. **HourlyChart**: Tạo mock data, render bằng Testing Library để đảm bảo 24 điểm dữ liệu.
4. **SearchBar**: Test input change, submit, và gọi `onSearch` đúng giá trị.

### 6.3 Thiết lập Vitest (tham khảo Movie Search)

```bash
npm install -D vitest @testing-library/react @testing-library/jest-dom
```

- Tạo `src/test/setup.ts` để import `@testing-library/jest-dom`.
- Cấu hình `vitest.config.ts` hoặc dùng cùng file với `vite.config.ts`.

---

## 7. XỬ LÝ API & GEOLOCATION

### 7.1 Quy trình gọi API

1. User nhập city / dùng geolocation → nhận `query`.
2. `fetchWeather(query)` set `loading = true`, clear `error`.
3. Gọi `getWeather(query)` → Axios request đến WeatherAPI.
4. Nếu thành công: cập nhật `weather`, lưu `lastCity`.
5. Nếu lỗi: set `error` (ví dụ “API key invalid”, “City not found”).
6. Cuối cùng: `loading = false` để ẩn spinner.

### 7.2 Geolocation

- Check `navigator.geolocation` trước khi gọi.
- Khi user deny → show `console.error`, có thể nâng cấp thành toast.
- Nếu muốn UX tốt hơn: show modal giải thích lý do cần location.

### 7.3 Rate limit & fallback

- WeatherAPI free tier giới hạn 1,000,000 calls/tháng.
- Có thể thêm debounce input hoặc caching (sessionStorage) nếu traffic cao.
- Khi không có internet → banner error vẫn hiển thị gọn gàng, không crash.

---

## 8. DATA VISUALIZATION

### 8.1 Hourly Chart

- Dùng `AreaChart` với gradient xanh tím, khớp theme tổng.
- Tooltips custom hiển thị nhiệt độ + icon.
- `ResponsiveContainer` giúp chart fill toàn bộ width.

### 8.2 Daily Forecast

- Grid 7 cột, từng card chứa: ngày, icon, min & max.
- Dữ liệu lấy từ `forecast.forecastday` → mapping sang component.

### 8.3 WeatherStats

- Cards hiển thị UV, Wind, Humidity, Pressure.
- Format số liệu bằng utils để thống nhất.

### 8.4 UX Considerations

- Gradient background tạo cảm giác trời chuyển màu.
- Dark mode đổi gradient sang tông tím đậm → vẫn giữ độ tương phản.
- Loading spinner dùng Tailwind `animate-spin` + blur background.

---

## 9. CÂU HỎI THƯỜNG GẶP

### Q1: Làm sao xử lý API key bảo mật?

**A:** Frontend bắt buộc expose key, nhưng có thể hạn chế domain trong WeatherAPI dashboard. Với bản production lớn, nên tạo backend proxy để che key.

### Q2: Nếu API trả về sai định dạng?

**A:** TypeScript sẽ cảnh báo ngay khi bạn truy cập field không tồn tại. Có thể thêm fallback (ví dụ `weather?.current ?? defaultValue`).

### Q3: Làm gì khi geolocation bị từ chối?

**A:** Hiển thị toast “Không truy cập được vị trí, vui lòng nhập tên thành phố”. Đồng thời disable button “Use my location” tạm thời.

### Q4: Có thể hỗ trợ nhiều ngôn ngữ không?

**A:** WeatherAPI có param `lang`. Bạn có thể thêm dropdown chọn ngôn ngữ và truyền vào request.

### Q5: Làm sao đo hiệu năng?

**A:** Dùng Lighthouse hoặc React Profiler. Hiện tại render chỉ phụ thuộc dữ liệu mới nên khá nhẹ. Nếu muốn tối ưu thêm có thể memo chart data.

---

## 10. TIPS CHO PHỎNG VẤN

### 10.1 Câu hỏi & gợi ý trả lời

**Q: “Mô tả nhanh dự án Weather Dashboard.”**
```
"Ứng dụng cung cấp thời tiết real-time, có tìm kiếm, geolocation,
biểu đồ 24h và dự báo 7 ngày. Em dùng React + TypeScript,
Recharts để vẽ biểu đồ và Tailwind cho UI."
```

**Q: “Làm sao đảm bảo dữ liệu hiển thị chính xác?”**
```
"Em map trực tiếp từ API, dùng TypeScript để chắc chắn mỗi field
được truy cập đúng kiểu. Ngoài ra có formatters xử lý số và thời gian."
```

**Q: “Nếu API chậm?”**
```
"Em đã show spinner và giữ giao diện ổn định. Nếu cần có thể
thêm caching, debounce hoặc hiển thị dữ liệu lần fetch trước khi API trả về."
```

### 10.2 Demo hiệu quả

**Trước buổi phỏng vấn:**
- Chạy `npm run dev`, mở sẵn tab demo.
- Test thử geolocation (có thể fake location trong Chrome DevTools).
- Chuẩn bị vài screenshot light/dark mode.

**Trong demo:**
- 1 phút giới thiệu mục tiêu ứng dụng.
- 2 phút thao tác: tìm thành phố → bật dark mode → xem chart.
- 1 phút giải thích code: mở `useWeather.ts`, `HourlyChart.tsx`.
- Nếu có thời gian: show `.env` template và cách bảo vệ API key.

### 10.3 Communication & Follow-up

- Giữ eye-contact, nói rõ ràng, hứng thú với dữ liệu real-time.
- Sau buổi phỏng vấn, gửi email cảm ơn tương tự template ở dự án Movie Search.

---

## 📚 TÀI LIỆU THAM KHẢO

### Official Docs:
- [WeatherAPI Docs](https://www.weatherapi.com/docs/)
- [React Docs](https://react.dev)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Tailwind CSS Docs](https://tailwindcss.com/docs)
- [Recharts Docs](https://recharts.org/en-US)

### Learning Resources:
- [Total TypeScript](https://www.totaltypescript.com)
- [Josh W Comeau Blog](https://www.joshwcomeau.com) – UI/UX tips
- [Smashing Magazine](https://www.smashingmagazine.com) – Inspiration cho dữ liệu

---

## 🎯 CHECKLIST TRƯỚC KHI NỘP CV

### Code & Build:
- [ ] `npm run lint` ✅
- [ ] `npm run build` ✅ (không lỗi TypeScript)
- [ ] Không còn `console.log` hoặc comment thừa

### Documentation:
- [ ] README có hướng dẫn lấy API key + screenshot light/dark
- [ ] Link demo hoạt động ổn định
- [ ] Nêu rõ các feature chính trong portfolio

### API & ENV:
- [ ] `.env` không bị commit
- [ ] API key cấu hình hạn chế domain (nếu có thể)

### Testing & QA:
- [ ] (Khuyến nghị) Thêm ít nhất 1-2 unit test cho utils
- [ ] Kiểm tra responsive: mobile 375px, tablet 768px, desktop 1440px
- [ ] Thử chế độ offline hoặc cố tình nhập city sai để đảm bảo error banner hoạt động

---

## ✨ KẾT LUẬN

Bạn đã có:
- ✅ Kỹ năng kết nối API bên thứ ba và xử lý dữ liệu phức tạp
- ✅ Kinh nghiệm trực quan hóa dữ liệu thời gian thực
- ✅ Khả năng xây UX mềm mại: gradient, dark mode, error state
- ✅ Kế hoạch mở rộng rõ ràng (favorites, alerts, PWA…)

**Remember:**
- 📚 Luôn trung thực về phạm vi (frontend focus, API public)
- 💡 Nhấn mạnh tư duy dữ liệu và trải nghiệm người dùng
- 🎯 Trong buổi demo, highlight flow tìm kiếm → biểu đồ → dự báo 7 ngày

**Sẵn sàng đem Weather Dashboard vào CV và chinh phục nhà tuyển dụng! 🚀**

---

*Guide được viết riêng cho bạn. Ôn kỹ, luyện demo, và tự tin tỏa sáng!* 💙
