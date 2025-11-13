# 📚 HƯỚNG DẪN CHI TIẾT DỰ ÁN MOVIE SEARCH
## Dành cho người mới bắt đầu

---

## 📖 MỤC LỤC

1. [Tổng Quan Dự Án](#1-tổng-quan-dự-án)
2. [Cài Đặt & Chạy](#2-cài-đặt--chạy)
3. [Cấu Trúc Thư Mục](#3-cấu-trúc-thư-mục)
4. [Giải Thích Tech Stack](#4-giải-thích-tech-stack)
5. [Giải Thích Code Chi Tiết](#5-giải-thích-code-chi-tiết)
6. [Testing - Kiểm Thử](#6-testing---kiểm-thử)
7. [Error Boundary](#7-error-boundary)
8. [Loading Skeletons](#8-loading-skeletons)
9. [Câu Hỏi Thường Gặp](#9-câu-hỏi-thường-gặp)
10. [Tips Cho Phỏng Vấn](#10-tips-cho-phỏng-vấn)

---

## 1. TỔNG QUAN DỰ ÁN

### 1.1 Dự án này là gì?

Đây là **ứng dụng web tìm kiếm phim** giống như Netflix mini. User có thể:
- 🔍 Tìm kiếm phim
- 📋 Xem danh sách phim phổ biến
- 📝 Xem chi tiết phim (rating, mô tả, trailer)
- 🎬 Xem danh sách diễn viên

### 1.2 Tại sao làm dự án này?

**Mục đích cho CV**:
- ✅ Chứng minh biết làm việc với API
- ✅ Chứng minh biết React + TypeScript
- ✅ Chứng minh có testing (quan trọng!)
- ✅ Chứng minh code sạch, có tổ chức

### 1.3 Nguồn dữ liệu

Dự án sử dụng **TMDB API** (The Movie Database) - một API miễn phí cung cấp thông tin về phim.

**Ví dụ**: Khi bạn search "Avengers", app sẽ gọi API của TMDB để lấy data về phim Avengers.

---

## 2. CÀI ĐẶT & CHẠY

### 2.1 Yêu cầu

Cần cài đặt trước:
- **Node.js** (version 18 trở lên) - [Download tại đây](https://nodejs.org)
- **Git** - [Download tại đây](https://git-scm.com)
- **Code Editor** - Recommend VS Code

### 2.2 Các bước cài đặt

#### Bước 1: Clone project
```bash
git clone https://github.com/nbv9704/movie-search.git
cd movie-search
```

**Giải thích**: 
- `git clone`: Tải code về máy
- `cd movie-search`: Di chuyển vào folder dự án

#### Bước 2: Cài đặt dependencies
```bash
npm install
```

**Giải thích**: 
- `npm install` sẽ đọc file `package.json` và cài tất cả thư viện cần thiết
- Thư viện sẽ được cài vào folder `node_modules`

#### Bước 3: Tạo file .env
Tạo file `.env` ở root folder với nội dung:
```
VITE_TMDB_API_KEY=your_api_key_here
VITE_TMDB_BASE_URL=https://api.themoviedb.org/3
VITE_TMDB_IMAGE_BASE_URL=https://image.tmdb.org/t/p
```

**Cách lấy API key**:
1. Đăng ký tài khoản tại [TMDB](https://www.themoviedb.org/signup)
2. Vào Settings → API → Request API Key
3. Copy key và paste vào file `.env`

#### Bước 4: Chạy app
```bash
npm run dev
```

**Giải thích**:
- Lệnh này start development server
- Mở browser vào `http://localhost:5173`
- App sẽ tự reload khi bạn sửa code

### 2.3 Các lệnh quan trọng

```bash
# Chạy app (development mode)
npm run dev

# Chạy tests
npm run test:run

# Build cho production (deploy)
npm run build

# Preview production build
npm run preview
```

---

## 3. CẤU TRÚC THƯ MỤC

### 3.1 Tổng quan

```
movie-search/
├── src/                    # Code chính
│   ├── components/         # React components
│   ├── pages/             # Các trang
│   ├── services/          # API calls
│   ├── types/             # TypeScript types
│   ├── utils/             # Helper functions
│   ├── hooks/             # Custom hooks
│   └── test/              # Test setup
├── public/                # Static files
└── package.json           # Dependencies
```

### 3.2 Chi tiết từng folder

#### 📁 `src/components/`
Chứa các React components - **những khối xây dựng của UI**

```
components/
├── common/              # Components dùng chung
│   ├── MovieCard.tsx    # Card hiển thị 1 phim
│   ├── SearchBar.tsx    # Thanh tìm kiếm
│   ├── Pagination.tsx   # Phân trang
│   ├── ErrorBoundary.tsx # Xử lý lỗi
│   └── *Skeleton.tsx    # Loading states
├── layout/             # Layout components
│   ├── Header.tsx      # Header của app
│   └── Footer.tsx      # Footer của app
└── movie/              # Components về phim
    ├── HeroSection.tsx # Banner chính
    ├── MovieGrid.tsx   # Grid hiển thị phim
    └── CastList.tsx    # Danh sách diễn viên
```

**Tại sao chia như vậy?**
- `common/`: Components có thể dùng ở nhiều nơi
- `layout/`: Components về layout tổng thể
- `movie/`: Components specific cho phim

#### 📁 `src/pages/`
Chứa các **trang** của app

```
pages/
├── HomePage.tsx         # Trang chủ
├── SearchPage.tsx       # Trang tìm kiếm
└── MovieDetailPage.tsx  # Trang chi tiết phim
```

**Page vs Component?**
- **Page**: Trang hoàn chỉnh, có URL riêng
- **Component**: Một phần của page

#### 📁 `src/services/`
Chứa code gọi API

```
services/
└── tmdbApi.ts          # Các function gọi TMDB API
```

**Tại sao tách riêng?**
- Dễ quản lý API calls
- Dễ test
- Dễ thay đổi API sau này

#### 📁 `src/types/`
Chứa TypeScript types

```
types/
└── movie.ts            # Types cho Movie data
```

**TypeScript type là gì?**
Type giúp define cấu trúc data. Ví dụ:
```typescript
interface Movie {
  id: number;
  title: string;
  overview: string;
}
```

#### 📁 `src/utils/`
Chứa helper functions

```
utils/
├── formatters.ts       # Format data (rating, date, etc.)
└── image.ts           # Xử lý image URLs
```

**Helper function là gì?**
Những function nhỏ, tái sử dụng được. Ví dụ:
```typescript
// Chuyển rating 8.456 → "8.5"
formatRating(8.456) // "8.5"
```

#### 📁 `src/hooks/`
Chứa custom React hooks

```
hooks/
└── useMovies.ts       # Hook để fetch movies
```

**Hook là gì?**
Hook là function đặc biệt của React để tái sử dụng logic.

#### 📁 `src/test/`
Chứa test setup và mock data

```
test/
├── setup.ts           # Cấu hình testing
└── mockData/
    └── movies.ts      # Data giả để test
```

---

## 4. GIẢI THÍCH TECH STACK

### 4.1 React - UI Library

**React là gì?**
React là thư viện JavaScript để xây dựng giao diện người dùng (UI).

**Tại sao dùng React?**
- ✅ Component-based: Chia UI thành các mảnh nhỏ
- ✅ Reusable: Tái sử dụng components
- ✅ Popular: Nhiều công ty dùng

**Ví dụ đơn giản:**
```jsx
function Welcome() {
  return <h1>Hello, World!</h1>;
}
```

### 4.2 TypeScript - Type Safety

**TypeScript là gì?**
TypeScript = JavaScript + Types. Giúp bắt lỗi sớm khi code.

**So sánh:**

JavaScript (không type):
```javascript
function add(a, b) {
  return a + b;
}
add(1, "2") // "12" - Bug!
```

TypeScript (có type):
```typescript
function add(a: number, b: number): number {
  return a + b;
}
add(1, "2") // Error! TypeScript sẽ báo lỗi ngay
```

### 4.3 Vite - Build Tool

**Vite là gì?**
Tool để build và chạy app. Nhanh hơn Webpack/Create React App.

**Làm gì?**
- 🚀 Start dev server siêu nhanh
- 📦 Build app cho production
- 🔥 Hot reload (tự động reload khi sửa code)

### 4.4 Tailwind CSS - Styling

**Tailwind là gì?**
Framework CSS với utility classes sẵn.

**Ví dụ:**
```jsx
// Thay vì viết CSS riêng:
<div className="text-blue-500 font-bold text-xl">
  Hello
</div>

// Tương đương:
// color: blue
// font-weight: bold
// font-size: 1.25rem
```

**Ưu điểm:**
- ⚡ Viết CSS nhanh
- 🎨 Consistent design
- 📱 Responsive dễ dàng

### 4.5 React Router - Routing

**React Router là gì?**
Library để tạo các routes (URLs) trong app.

**Ví dụ:**
```typescript
<Routes>
  <Route path="/" element={<HomePage />} />
  <Route path="/search" element={<SearchPage />} />
  <Route path="/movie/:id" element={<MovieDetailPage />} />
</Routes>
```

- `/` → Hiển thị HomePage
- `/search` → Hiển thị SearchPage
- `/movie/123` → Hiển thị chi tiết phim có id=123

### 4.6 Axios - HTTP Client

**Axios là gì?**
Library để gọi API.

**Ví dụ:**
```typescript
// Gọi API để lấy danh sách phim
const response = await axios.get('https://api.themoviedb.org/3/movie/popular');
console.log(response.data); // Data phim
```

### 4.7 Vitest - Testing

**Vitest là gì?**
Tool để test code. Đảm bảo code hoạt động đúng.

**Ví dụ test:**
```typescript
test('formatRating should work', () => {
  expect(formatRating(8.456)).toBe('8.5');
});
```

---

## 5. GIẢI THÍCH CODE CHI TIẾT

### 5.1 File Entry Point: `main.tsx`

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

**Giải thích từng dòng:**

1. **Import statements**: Load các thư viện và files cần thiết
   ```typescript
   import React from 'react'        // React library
   import ReactDOM from 'react-dom/client'  // Để render React vào HTML
   import App from './App.tsx'      // Component chính
   import './index.css'             // CSS global
   ```

2. **ReactDOM.createRoot()**: Tạo root để render React app
   ```typescript
   document.getElementById('root')  // Lấy element có id="root" trong HTML
   ```

3. **render()**: Hiển thị App lên màn hình
   ```typescript
   <App />  // Component chính của app
   ```

4. **React.StrictMode**: Mode để phát hiện bugs sớm (chỉ trong development)

### 5.2 Component Chính: `App.tsx`

```typescript
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import ErrorBoundary from './components/common/ErrorBoundary';
import Header from './components/layout/Header';
import Footer from './components/layout/Footer';
import HomePage from './pages/HomePage';
import SearchPage from './pages/SearchPage';
import MovieDetailPage from './pages/MovieDetailPage';

function App() {
  return (
    <ErrorBoundary>
      <BrowserRouter>
        <div className="flex flex-col min-h-screen bg-white dark:bg-gray-900">
          <Header />
          <main className="flex-1">
            <Routes>
              <Route path="/" element={<HomePage />} />
              <Route path="/search" element={<SearchPage />} />
              <Route path="/movie/:id" element={<MovieDetailPage />} />
            </Routes>
          </main>
          <Footer />
        </div>
      </BrowserRouter>
    </ErrorBoundary>
  );
}

export default App;
```

**Giải thích cấu trúc:**

1. **ErrorBoundary**: Bọc toàn bộ app để catch errors
   - Nếu có lỗi → Show error page
   - Không có lỗi → App chạy bình thường

2. **BrowserRouter**: Enable routing (URL navigation)

3. **Layout**: 
   ```
   ┌─────────────┐
   │   Header    │ ← Luôn hiển thị
   ├─────────────┤
   │             │
   │   Content   │ ← Thay đổi theo route
   │             │
   ├─────────────┤
   │   Footer    │ ← Luôn hiển thị
   └─────────────┘
   ```

4. **Routes**: Define các đường dẫn
   - `/` → HomePage
   - `/search` → SearchPage
   - `/movie/123` → MovieDetailPage (chi tiết phim id=123)

### 5.3 Component: `MovieCard.tsx`

Đây là component hiển thị 1 card phim.

```typescript
import { Link } from 'react-router-dom';
import { Star } from 'lucide-react';
import type { Movie } from '../../types/movie';
import { getPosterUrl } from '../../utils/image';
import { formatRating, formatYear } from '../../utils/formatters';

interface MovieCardProps {
  movie: Movie;  // Data của 1 phim
}

export default function MovieCard({ movie }: MovieCardProps) {
  return (
    <Link
      to={`/movie/${movie.id}`}
      className="group relative overflow-hidden rounded-lg bg-gray-100 dark:bg-gray-800 shadow-md hover:shadow-xl transition-all duration-300 transform hover:scale-105"
    >
      {/* Poster Image */}
      <div className="aspect-[2/3] overflow-hidden">
        <img
          src={getPosterUrl(movie.poster_path)}
          alt={movie.title}
          className="w-full h-full object-cover transition-transform duration-300 group-hover:scale-110"
          loading="lazy"
        />
      </div>

      {/* Overlay with info */}
      <div className="absolute inset-0 bg-gradient-to-t from-black/90 via-black/50 to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300">
        <div className="absolute bottom-0 left-0 right-0 p-4">
          <h3 className="text-white font-bold text-lg mb-2 line-clamp-2">
            {movie.title}
          </h3>
          <div className="flex items-center justify-between text-sm text-gray-300">
            <span>{formatYear(movie.release_date)}</span>
            <div className="flex items-center gap-1">
              <Star className="w-4 h-4 fill-yellow-400 text-yellow-400" />
              <span className="font-semibold">{formatRating(movie.vote_average)}</span>
            </div>
          </div>
        </div>
      </div>

      {/* Rating badge (always visible) */}
      <div className="absolute top-2 right-2 bg-black/70 backdrop-blur-sm px-2 py-1 rounded-full flex items-center gap-1">
        <Star className="w-3 h-3 fill-yellow-400 text-yellow-400" />
        <span className="text-white text-xs font-bold">
          {formatRating(movie.vote_average)}
        </span>
      </div>
    </Link>
  );
}
```

**Phân tích chi tiết:**

1. **Props (Input):**
   ```typescript
   interface MovieCardProps {
     movie: Movie;  // Nhận vào data 1 phim
   }
   ```
   Props là cách truyền data vào component.

2. **Link Component:**
   ```typescript
   <Link to={`/movie/${movie.id}`}>
   ```
   - Click vào card → Navigate đến trang chi tiết
   - Ví dụ: Click phim có id=123 → `/movie/123`

3. **Image:**
   ```typescript
   <img
     src={getPosterUrl(movie.poster_path)}
     alt={movie.title}
     loading="lazy"
   />
   ```
   - `src`: URL của ảnh poster
   - `alt`: Text mô tả (cho accessibility)
   - `loading="lazy"`: Load ảnh khi user scroll đến (tối ưu performance)

4. **Overlay effect:**
   ```typescript
   className="opacity-0 group-hover:opacity-100"
   ```
   - Ban đầu: `opacity-0` (ẩn)
   - Khi hover: `opacity-100` (hiện)
   - `group-hover`: Hover vào parent `.group` → child effect

5. **Helper functions:**
   ```typescript
   getPosterUrl(movie.poster_path)      // Tạo full URL ảnh
   formatYear(movie.release_date)       // "1999-10-15" → "1999"
   formatRating(movie.vote_average)     // 8.456 → "8.5"
   ```

### 5.4 API Service: `tmdbApi.ts`

File này chứa các function gọi API.

```typescript
import axios from 'axios';
import type { MovieDetail, MovieResponse } from '../types/movie';

// Lấy API key và URL từ environment variables
const API_KEY = import.meta.env.VITE_TMDB_API_KEY;
const BASE_URL = import.meta.env.VITE_TMDB_BASE_URL;

// Tạo axios instance với config chung
const tmdbClient = axios.create({
  baseURL: BASE_URL,
  params: {
    api_key: API_KEY,  // Tự động thêm API key vào mọi request
  },
});

// Function tìm kiếm phim
export const searchMovies = async (
  query: string,
  page: number = 1
): Promise<MovieResponse> => {
  try {
    const response = await tmdbClient.get('/search/movie', {
      params: { query, page },
    });
    return response.data;
  } catch (error) {
    console.error('Error searching movies:', error);
    throw new Error('Failed to search movies');
  }
};

// Function lấy phim phổ biến
export const getPopularMovies = async (
  page: number = 1
): Promise<MovieResponse> => {
  try {
    const response = await tmdbClient.get('/movie/popular', {
      params: { page },
    });
    return response.data;
  } catch (error) {
    console.error('Error fetching popular movies:', error);
    throw new Error('Failed to fetch popular movies');
  }
};
```

**Phân tích:**

1. **axios.create()**: Tạo HTTP client có config chung
   ```typescript
   const tmdbClient = axios.create({
     baseURL: BASE_URL,           // URL gốc
     params: { api_key: API_KEY } // Params mặc định
   });
   ```
   
   Benefit: Không cần viết lại BASE_URL và API_KEY ở mọi request.

2. **async/await**: Xử lý bất đồng bộ (asynchronous)
   ```typescript
   async function example() {
     const response = await tmdbClient.get('/movie/popular');
     // Đợi API trả về rồi mới chạy tiếp
   }
   ```

3. **try/catch**: Xử lý lỗi
   ```typescript
   try {
     // Code có thể gây lỗi
     const response = await tmdbClient.get(...);
   } catch (error) {
     // Nếu có lỗi → Chạy code này
     console.error('Error:', error);
     throw new Error('Failed...');
   }
   ```

4. **TypeScript Promise<>**: Định nghĩa kiểu data trả về
   ```typescript
   Promise<MovieResponse>
   // Function này sẽ trả về MovieResponse khi hoàn thành
   ```

### 5.5 Custom Hook: `useMovies.ts`

Hook để fetch movies và quản lý state.

```typescript
import { useState, useEffect } from 'react';
import type { Movie, MovieResponse } from '../types/movie';

export function useMovies(
  fetchFunction: (page: number) => Promise<MovieResponse>,
  initialPage: number = 1
) {
  const [movies, setMovies] = useState<Movie[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);
  const [page, setPage] = useState(initialPage);
  const [totalPages, setTotalPages] = useState(0);

  useEffect(() => {
    const fetchMovies = async () => {
      try {
        setLoading(true);
        setError(null);
        const data = await fetchFunction(page);
        setMovies(data.results);
        setTotalPages(Math.min(data.total_pages, 500));
      } catch (err) {
        setError(err instanceof Error ? err.message : 'Failed to fetch movies');
      } finally {
        setLoading(false);
      }
    };

    fetchMovies();
  }, [page]);

  return { movies, loading, error, page, totalPages, setPage };
}
```

**Giải thích concepts:**

1. **useState**: Hook để lưu state (data thay đổi)
   ```typescript
   const [movies, setMovies] = useState<Movie[]>([]);
   // movies: Giá trị hiện tại
   // setMovies: Function để update giá trị
   // []: Giá trị ban đầu (empty array)
   ```

2. **useEffect**: Hook chạy side effects
   ```typescript
   useEffect(() => {
     // Code này chạy khi 'page' thay đổi
     fetchMovies();
   }, [page]);  // Dependencies
   ```
   
   **Khi nào chạy?**
   - Lần đầu component render
   - Mỗi khi `page` thay đổi

3. **Async function trong useEffect:**
   ```typescript
   useEffect(() => {
     const fetchMovies = async () => {
       // Fetch data
     };
     fetchMovies();  // Gọi function
   }, [page]);
   ```

4. **try/catch/finally:**
   ```typescript
   try {
     setLoading(true);      // Bắt đầu loading
     const data = await fetchFunction(page);
     setMovies(data.results);
   } catch (err) {
     setError(err.message); // Có lỗi → Set error
   } finally {
     setLoading(false);     // Luôn chạy, dù lỗi hay không
   }
   ```

5. **Return object:**
   ```typescript
   return { movies, loading, error, page, totalPages, setPage };
   ```
   Component khác có thể dùng:
   ```typescript
   const { movies, loading, error } = useMovies(getPopularMovies);
   ```

### 5.6 Utils: `formatters.ts`

Helper functions để format data.

```typescript
// Format rating: 8.456 → "8.5"
export const formatRating = (rating: number): string => {
  return rating.toFixed(1);
};

// Extract year: "1999-10-15" → "1999"
export const formatYear = (dateString: string): string => {
  return new Date(dateString).getFullYear().toString();
};

// Format runtime: 139 minutes → "2h 19m"
export const formatRuntime = (minutes: number): string => {
  const hours = Math.floor(minutes / 60);
  const mins = minutes % 60;
  return `${hours}h ${mins}m`;
};
```

**Giải thích từng function:**

1. **formatRating:**
   ```typescript
   rating.toFixed(1)
   // 8.456 → "8.5"
   // 7.0 → "7.0"
   ```
   `.toFixed(1)`: Làm tròn đến 1 số thập phân

2. **formatYear:**
   ```typescript
   new Date("1999-10-15")  // Tạo Date object
   .getFullYear()          // Lấy năm: 1999
   .toString()             // Convert thành string: "1999"
   ```

3. **formatRuntime:**
   ```typescript
   const hours = Math.floor(139 / 60)  // 2 (làm tròn xuống)
   const mins = 139 % 60               // 19 (phần dư)
   return `${hours}h ${mins}m`         // "2h 19m"
   ```
   - `Math.floor()`: Làm tròn xuống
   - `%`: Phép chia lấy dư

### 5.7 Utils: `image.ts`

Helper functions để xử lý image URLs.

```typescript
const IMAGE_BASE_URL = import.meta.env.VITE_TMDB_IMAGE_BASE_URL;

export const getImageUrl = (
  path: string | null,
  size: 'w200' | 'w300' | 'w500' | 'w780' | 'original' = 'w500'
): string => {
  if (!path) return '/placeholder-movie.jpg';
  return `${IMAGE_BASE_URL}/${size}${path}`;
};

export const getPosterUrl = (path: string | null): string => {
  return getImageUrl(path, 'w500');
};
```

**Giải thích:**

1. **getImageUrl:**
   ```typescript
   // Input: "/abc123.jpg", "w500"
   // Output: "https://image.tmdb.org/t/p/w500/abc123.jpg"
   ```
   
   TMDB API trả về path: `/abc123.jpg`
   Cần ghép với base URL và size: `https://...t/p/w500/abc123.jpg`

2. **Size options:**
   - `w200`: Width 200px (nhỏ, cho thumbnails)
   - `w500`: Width 500px (medium, cho cards)
   - `original`: Full resolution (lớn, cho detail page)

3. **Null check:**
   ```typescript
   if (!path) return '/placeholder-movie.jpg';
   ```
   Nếu không có ảnh → Dùng placeholder

---

## 6. TESTING - KIỂM THỬ

### 6.1 Testing là gì?

**Định nghĩa đơn giản:**
Testing = Viết code để kiểm tra code của bạn có hoạt động đúng không.

**Ví dụ thực tế:**
Bạn có function cộng 2 số:
```typescript
function add(a: number, b: number) {
  return a + b;
}
```

Test để đảm bảo nó hoạt động:
```typescript
test('add should work', () => {
  expect(add(1, 2)).toBe(3);      // ✅ Pass
  expect(add(5, 5)).toBe(10);     // ✅ Pass
});
```

### 6.2 Tại sao cần testing?

**Lợi ích:**
1. ✅ **Catch bugs sớm**: Phát hiện lỗi trước khi deploy
2. ✅ **Confidence**: Tự tin khi refactor code
3. ✅ **Documentation**: Tests = doc về cách code hoạt động
4. ✅ **CV value**: Công ty thích ứng viên biết testing

**Khi nào nên test?**
- ✅ Utils functions (formatters, helpers)
- ✅ Core components (MovieCard, SearchBar)
- ❌ KHÔNG test mọi thứ (overkill!)

### 6.3 Test Setup

File `src/test/setup.ts`:
```typescript
import { afterEach } from 'vitest';
import { cleanup } from '@testing-library/react';
import '@testing-library/jest-dom/vitest';

afterEach(() => {
  cleanup();
});
```

**Giải thích:**

1. **afterEach**: Chạy sau mỗi test
2. **cleanup()**: Dọn dẹp DOM sau test (tránh conflict)
3. **jest-dom/vitest**: Thêm matchers như `toBeInTheDocument()`

### 6.4 Test Utils: `formatters.test.ts`

```typescript
import { describe, it, expect } from 'vitest';
import { formatRating, formatYear, formatRuntime } from '../formatters';

describe('formatters', () => {
  describe('formatRating', () => {
    it('should format rating to 1 decimal place', () => {
      expect(formatRating(8.456)).toBe('8.5');
      expect(formatRating(7.0)).toBe('7.0');
    });
  });

  describe('formatYear', () => {
    it('should extract year from date string', () => {
      expect(formatYear('1999-10-15')).toBe('1999');
      expect(formatYear('2024-01-01')).toBe('2024');
    });
  });

  describe('formatRuntime', () => {
    it('should format runtime correctly', () => {
      expect(formatRuntime(139)).toBe('2h 19m');
      expect(formatRuntime(90)).toBe('1h 30m');
    });
  });
});
```

**Phân tích cấu trúc:**

1. **describe**: Nhóm các tests liên quan
   ```typescript
   describe('formatters', () => {
     // Tất cả tests về formatters
   });
   ```

2. **it**: Một test case
   ```typescript
   it('should format rating to 1 decimal place', () => {
     // Test logic
   });
   ```
   
   Đặt tên test rõ ràng: "should do X"

3. **expect**: Assertion (kiểm tra kết quả)
   ```typescript
   expect(formatRating(8.456)).toBe('8.5');
   //     ↑ Actual              ↑ Expected
   ```
   
   Nếu actual ≠ expected → Test fail ❌

**Chạy tests:**
```bash
npm run test:run
```

**Output:**
```
✓ formatters.test.ts (3 tests)
  ✓ formatRating (2 assertions)
  ✓ formatYear (2 assertions)
  ✓ formatRuntime (2 assertions)

Tests: 3 passed
```

### 6.5 Test Component: `MovieCard.test.tsx`

```typescript
import { describe, it, expect, vi } from 'vitest';
import { render, screen } from '@testing-library/react';
import { BrowserRouter } from 'react-router-dom';
import MovieCard from '../MovieCard';
import { mockMovie } from '../../../test/mockData/movies';

// Mock the image utils
vi.mock('../../../utils/image', () => ({
  getPosterUrl: (path: string | null) => 
    path ? `https://image.tmdb.org/t/p/w500${path}` : '/placeholder.jpg',
}));

const renderWithRouter = (component: React.ReactElement) => {
  return render(<BrowserRouter>{component}</BrowserRouter>);
};

describe('MovieCard', () => {
  it('should render movie title', () => {
    renderWithRouter(<MovieCard movie={mockMovie} />);
    expect(screen.getByText('Fight Club')).toBeInTheDocument();
  });

  it('should render movie poster image', () => {
    renderWithRouter(<MovieCard movie={mockMovie} />);
    const image = screen.getByAltText('Fight Club') as HTMLImageElement;
    expect(image).toBeInTheDocument();
  });

  it('should render movie rating', () => {
    renderWithRouter(<MovieCard movie={mockMovie} />);
    const ratings = screen.getAllByText('8.4');
    expect(ratings.length).toBeGreaterThan(0);
  });

  it('should have link to movie detail page', () => {
    renderWithRouter(<MovieCard movie={mockMovie} />);
    const link = screen.getByRole('link');
    expect(link).toHaveAttribute('href', '/movie/550');
  });
});
```

**Giải thích concepts:**

1. **Mock**: Giả lập function/module
   ```typescript
   vi.mock('../../../utils/image', () => ({
     getPosterUrl: () => 'fake-url.jpg'
   }));
   ```
   
   **Tại sao?** Không muốn gọi API thật khi test.

2. **render**: Render component để test
   ```typescript
   render(<MovieCard movie={mockMovie} />)
   ```

3. **screen**: Query elements trong rendered component
   ```typescript
   screen.getByText('Fight Club')     // Tìm text
   screen.getByAltText('Fight Club')  // Tìm img có alt
   screen.getByRole('link')           // Tìm link
   ```

4. **toBeInTheDocument**: Check element có tồn tại không
   ```typescript
   expect(element).toBeInTheDocument()  // ✅ Element tồn tại
   ```

5. **BrowserRouter wrapper**: 
   ```typescript
   <BrowserRouter>
     <MovieCard movie={mockMovie} />
   </BrowserRouter>
   ```
   MovieCard dùng `<Link>` nên cần Router wrapper.

### 6.6 Mock Data

File `src/test/mockData/movies.ts`:
```typescript
export const mockMovie: Movie = {
  id: 550,
  title: 'Fight Club',
  overview: 'A ticking-time-bomb insomniac...',
  poster_path: '/pB8BM7pdSp6B6Ih7QZ4DrQ3PmJK.jpg',
  backdrop_path: '/fCayJrkfRaCRCTh8GqN30f8oyQF.jpg',
  release_date: '1999-10-15',
  vote_average: 8.4,
  vote_count: 26280,
  popularity: 61.416,
  genre_ids: [18, 53, 35],
  adult: false,
  original_language: 'en',
  video: false,
};
```

**Tại sao cần mock data?**
- ✅ Không phụ thuộc vào API thật
- ✅ Tests chạy nhanh
- ✅ Consistent data (không thay đổi)

---

## 7. ERROR BOUNDARY

### 7.1 Error Boundary là gì?

**Định nghĩa đơn giản:**
Error Boundary = Component đặc biệt để catch errors trong React tree.

**Tại sao cần?**
Nếu không có Error Boundary:
```
App crashes → Blank white screen → User confused 😱
```

Có Error Boundary:
```
Error happens → Show friendly error page → User can retry 😊
```

### 7.2 Code: `ErrorBoundary.tsx`

```typescript
import React, { Component } from 'react';
import type { ReactNode } from 'react';
import { AlertTriangle } from 'lucide-react';

interface Props {
  children: ReactNode;
  fallback?: ReactNode;
}

interface State {
  hasError: boolean;
  error: Error | null;
}

class ErrorBoundary extends Component<Props, State> {
  constructor(props: Props) {
    super(props);
    this.state = {
      hasError: false,
      error: null,
    };
  }

  static getDerivedStateFromError(error: Error): State {
    return {
      hasError: true,
      error,
    };
  }

  componentDidCatch(error: Error, errorInfo: React.ErrorInfo): void {
    console.error('ErrorBoundary caught an error:', error, errorInfo);
  }

  handleReset = (): void => {
    this.setState({
      hasError: false,
      error: null,
    });
  };

  render(): ReactNode {
    if (this.state.hasError) {
      // Show error UI
      return (
        <div className="min-h-screen flex items-center justify-center">
          <div className="max-w-md w-full bg-white rounded-lg shadow-lg p-8 text-center">
            <AlertTriangle className="w-12 h-12 text-red-600 mx-auto mb-4" />
            <h1 className="text-2xl font-bold mb-2">
              Oops! Something went wrong
            </h1>
            <p className="text-gray-600 mb-6">
              We're sorry, but something unexpected happened.
            </p>
            <button
              onClick={this.handleReset}
              className="px-6 py-2 bg-blue-500 text-white rounded-lg"
            >
              Try Again
            </button>
          </div>
        </div>
      );
    }

    return this.props.children;
  }
}

export default ErrorBoundary;
```

**Giải thích từng phần:**

1. **Class Component**: Error Boundary phải là class (không thể là function)
   ```typescript
   class ErrorBoundary extends Component<Props, State>
   ```

2. **State**: Lưu trạng thái error
   ```typescript
   interface State {
     hasError: boolean;  // Có lỗi không?
     error: Error | null;  // Lỗi gì?
   }
   ```

3. **getDerivedStateFromError**: Lifecycle method bắt error
   ```typescript
   static getDerivedStateFromError(error: Error): State {
     return { hasError: true, error };
   }
   ```
   Khi có error → Update state

4. **componentDidCatch**: Log error để debug
   ```typescript
   componentDidCatch(error: Error, errorInfo: React.ErrorInfo): void {
     console.error('Error:', error, errorInfo);
   }
   ```

5. **render**: Hiển thị UI dựa trên state
   ```typescript
   if (this.state.hasError) {
     return <ErrorUI />;  // Error UI
   }
   return this.props.children;  // Normal UI
   ```

6. **handleReset**: Reset error state
   ```typescript
   handleReset = () => {
     this.setState({ hasError: false, error: null });
   };
   ```
   User click "Try Again" → Reset state → App chạy lại

### 7.3 Cách sử dụng

Wrap app trong ErrorBoundary:
```typescript
<ErrorBoundary>
  <App />
</ErrorBoundary>
```

**Flow:**
```
1. User đang dùng app
2. Error xảy ra (VD: API fail)
3. ErrorBoundary catch error
4. Show error page với button "Try Again"
5. User click "Try Again"
6. ErrorBoundary reset state
7. App chạy lại bình thường
```

---

## 8. LOADING SKELETONS

### 8.1 Skeleton là gì?

**Định nghĩa:**
Skeleton = Placeholder hiển thị khi đang load data.

**So sánh:**

**Spinner (cũ):**
```
[Loading spinner...] ← User không biết gì
```

**Skeleton (mới):**
```
┌─────────────┐
│ ▯▯▯▯▯▯▯▯▯▯ │ ← Placeholder giống layout thật
│ ▯▯▯▯▯       │
└─────────────┘
```

**Ưu điểm:**
- ✅ User thấy layout ngay
- ✅ Perceived performance tốt hơn
- ✅ Giảm layout shift
- ✅ Professional appearance

### 8.2 Code: `MovieCardSkeleton.tsx`

```typescript
export default function MovieCardSkeleton() {
  return (
    <div className="group relative overflow-hidden rounded-lg bg-gray-200 dark:bg-gray-800 shadow-md animate-pulse">
      {/* Poster placeholder */}
      <div className="aspect-[2/3] bg-gray-300 dark:bg-gray-700" />

      {/* Rating badge placeholder */}
      <div className="absolute top-2 right-2 bg-gray-300 dark:bg-gray-700 px-2 py-1 rounded-full w-12 h-6" />
    </div>
  );
}
```

**Giải thích:**

1. **animate-pulse**: Tailwind utility cho pulse effect
   ```css
   @keyframes pulse {
     0%, 100% { opacity: 1; }
     50% { opacity: 0.5; }
   }
   ```

2. **aspect-[2/3]**: Giữ tỉ lệ 2:3 (giống poster thật)

3. **bg-gray-300**: Màu xám cho placeholder

### 8.3 Code: `MovieGridSkeleton.tsx`

```typescript
import MovieCardSkeleton from './MovieCardSkeleton';

interface MovieGridSkeletonProps {
  count?: number;  // Số lượng skeletons
}

export default function MovieGridSkeleton({ count = 20 }: MovieGridSkeletonProps) {
  return (
    <div className="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 gap-6">
      {Array.from({ length: count }).map((_, index) => (
        <MovieCardSkeleton key={index} />
      ))}
    </div>
  );
}
```

**Giải thích:**

1. **Array.from({ length: count })**: Tạo array với count elements
   ```typescript
   Array.from({ length: 3 })  // [undefined, undefined, undefined]
   ```

2. **map**: Loop qua array và render skeleton
   ```typescript
   .map((_, index) => <MovieCardSkeleton key={index} />)
   ```

### 8.4 Cách sử dụng

```typescript
function HomePage() {
  const { movies, loading } = useMovies(getPopularMovies);

  if (loading) {
    return <MovieGridSkeleton count={20} />;  // Show skeletons
  }

  return <MovieGrid movies={movies} />;  // Show real movies
}
```

**Flow:**
```
1. User vào trang
2. loading = true → Show skeletons
3. API trả về data
4. loading = false → Show real movies
```

---

## 9. CÂU HỎI THƯỜNG GẶP

### Q1: Props là gì?

**A:** Props = Properties = Cách truyền data từ parent → child component.

**Ví dụ:**
```typescript
// Parent
<MovieCard movie={movieData} />

// Child
function MovieCard({ movie }) {
  return <div>{movie.title}</div>
}
```

### Q2: State là gì?

**A:** State = Data có thể thay đổi trong component.

**Ví dụ:**
```typescript
const [count, setCount] = useState(0);  // Initial: 0
setCount(1);  // Update → Component re-render
```

### Q3: useEffect dùng để làm gì?

**A:** useEffect = Run side effects (API calls, subscriptions, etc.)

**Ví dụ:**
```typescript
useEffect(() => {
  fetchMovies();  // Gọi API
}, [page]);  // Chạy lại khi 'page' thay đổi
```

### Q4: Tại sao cần TypeScript?

**A:** TypeScript = JavaScript + Types → Catch bugs sớm.

**Ví dụ:**
```typescript
// JavaScript: Bug không phát hiện
function add(a, b) {
  return a + b;
}
add(1, "2")  // "12" - Bug!

// TypeScript: Lỗi ngay lập tức
function add(a: number, b: number): number {
  return a + b;
}
add(1, "2")  // ❌ Error: Type 'string' is not assignable to type 'number'
```

### Q5: async/await là gì?

**A:** async/await = Cách viết asynchronous code dễ đọc hơn.

**So sánh:**

Promises (cũ):
```typescript
fetchMovies()
  .then(data => setMovies(data))
  .catch(error => console.error(error))
```

Async/await (mới):
```typescript
try {
  const data = await fetchMovies();
  setMovies(data);
} catch (error) {
  console.error(error);
}
```

### Q6: Tại sao cần keys trong list?

**A:** Keys giúp React identify elements khi re-render.

**Ví dụ:**
```typescript
// ❌ Bad: No key
movies.map(movie => <MovieCard movie={movie} />)

// ✅ Good: With key
movies.map(movie => <MovieCard key={movie.id} movie={movie} />)
```

**Tại sao quan trọng?**
- Performance: React biết element nào thay đổi
- Avoid bugs: Đảm bảo đúng element được update

### Q7: Khi nào dùng useEffect?

**A:** Dùng khi cần side effects:
- ✅ Fetch data từ API
- ✅ Subscribe to events
- ✅ Update DOM manually
- ✅ Set up timers

**Không dùng cho:**
- ❌ Transform data (dùng useMemo)
- ❌ Event handlers (dùng functions)
- ❌ Calculations (dùng variables)

### Q8: Component vs Page khác gì?

**A:**
- **Component**: Một phần của UI, reusable
  - VD: Button, Card, Modal
- **Page**: Một trang hoàn chỉnh, có route
  - VD: HomePage, SearchPage

### Q9: Tailwind classes khó nhớ không?

**A:** Ban đầu khó, nhưng:
- ✅ IDE có autocomplete
- ✅ Quen dần sẽ nhớ
- ✅ Có docs để tra cứu

**Tips:**
```
text-xl     = font-size: 1.25rem
text-blue-500 = color: #3b82f6
p-4         = padding: 1rem
m-2         = margin: 0.5rem
```

### Q10: Làm sao debug khi có lỗi?

**A:** Các bước debug:

1. **Đọc error message** - Thường nó chỉ rõ vấn đề
2. **Console.log** - Log data ra xem
3. **React DevTools** - Xem component state/props
4. **Network tab** - Check API calls
5. **Google error message** - Nhiều người gặp vấn đề giống bạn

---

## 10. TIPS CHO PHỎNG VẤN

### 10.1 Câu hỏi thường gặp & Cách trả lời

#### Q: "Giới thiệu về dự án này"

**Template câu trả lời:**
```
"Đây là ứng dụng tìm kiếm phim được build bằng React và TypeScript.

Features chính:
- Tìm kiếm phim với TMDB API
- Hiển thị danh sách phim popular
- Chi tiết phim với trailer và cast
- Responsive design cho mobile

Tech stack:
- React 19 với TypeScript
- Tailwind CSS cho styling
- React Router cho navigation
- Vitest cho testing

Em focus vào code quality với TypeScript types và unit tests
cho core functionality."
```

#### Q: "Tại sao chọn React?"

**Trả lời:**
```
"Em chọn React vì:
1. Component-based: Dễ tái sử dụng và maintain
2. Large ecosystem: Nhiều libraries và resources
3. Industry standard: Nhiều công ty dùng React
4. Em đã học và practice React nên tự tin nhất với nó"
```

#### Q: "Tại sao dùng TypeScript?"

**Trả lời:**
```
"TypeScript giúp:
1. Catch bugs sớm với type checking
2. Better IDE support với autocomplete
3. Code dễ đọc và maintain hơn
4. Document code tốt hơn với types

Ví dụ: Khi em định nghĩa Movie type, IDE sẽ suggest
properties và báo lỗi nếu em type sai."
```

#### Q: "Explain về testing trong dự án"

**Trả lời:**
```
"Em có 7 unit tests covering:
- Utils functions như formatRating, formatYear
- MovieCard component

Em dùng Vitest và React Testing Library.

Tests giúp em:
1. Ensure functions hoạt động đúng
2. Tự tin khi refactor code
3. Catch bugs sớm

Em focus vào test những functions quan trọng nhất
thay vì test mọi thứ."
```

#### Q: "Làm sao xử lý errors?"

**Trả lời:**
```
"Em có 2 layers error handling:

1. API level: try/catch trong API calls
   - Catch network errors
   - Show error messages

2. Component level: Error Boundary
   - Catch React errors
   - Show fallback UI thay vì crash app
   
Error Boundary là React pattern recommend cho production."
```

#### Q: "Performance optimization?"

**Trả lời (Honest approach):**
```
"Em có research về React.memo, useMemo, useCallback
nhưng trong project này em chưa implement vì:

1. App hiện tại chạy smooth
2. Em follow principle 'premature optimization'
3. Em muốn master basics trước

Tuy nhiên em biết khi nào nên dùng:
- React.memo: Component re-render nhiều không cần thiết
- useMemo: Expensive calculations
- useCallback: Pass callbacks to child components

Em đang học và sẽ apply khi thực sự cần."
```

### 10.2 Demo dự án hiệu quả

#### Chuẩn bị trước:

1. **App đang chạy sẵn**
   ```bash
   npm run dev
   ```

2. **Tests pass**
   ```bash
   npm run test:run
   ```

3. **Code sạch, không có console.logs**

4. **README.md cập nhật**

#### Khi demo:

**1. Overview (30s)**
```
"Đây là movie search app. Em sẽ demo các features chính..."
```

**2. Features walkthrough (2-3 phút)**
- Home page: Trending movies với hero section
- Search: Tìm kiếm và pagination
- Detail page: Thông tin chi tiết, cast, trailer

**3. Technical highlights (2 phút)**
- Mở VSCode, show folder structure
- Explain component organization
- Show một component đơn giản (MovieCard)
- Show tests và chạy tests

**4. Error handling demo (1 phút)**
- Explain Error Boundary
- Show ErrorBoundary.tsx code

**5. Q&A (còn lại)**
- Sẵn sàng trả lời questions

#### Don'ts:

- ❌ KHÔNG apologize về thiếu features
- ❌ KHÔNG compare với production apps
- ❌ KHÔNG nói "Em không biết"
  - Thay vào: "Em chưa có cơ hội học về X, nhưng em sẽ research"

### 10.3 Body language & Communication

#### Dos:

- ✅ **Confident posture**: Ngồi thẳng, nhìn camera
- ✅ **Speak clearly**: Nói rõ ràng, không quá nhanh
- ✅ **Enthusiasm**: Show passion về tech
- ✅ **Honest**: Thành thật về skills
- ✅ **Examples**: Đưa examples cụ thể

#### Don'ts:

- ❌ Fidgeting
- ❌ Monotone voice
- ❌ Looking down
- ❌ "Ummm..." quá nhiều

### 10.4 Câu hỏi nên hỏi lại interviewer

Cuối interview, khi được hỏi "Do you have questions?":

**Good questions:**

1. "Team hiện tại đang dùng tech stack gì?"
2. "Onboarding process cho intern như thế nào?"
3. "Một ngày typical của intern trông như thế nào?"
4. "Team có practice nào về code review không?"
5. "Cơ hội để học và grow như thế nào?"

**Avoid:**

- ❌ "Làm việc bao nhiêu giờ?"
- ❌ "Remote được không?"
- ❌ "Lương bao nhiêu?" (hỏi HR, không phỏng vấn kỹ thuật)

### 10.5 After interview

**Follow-up email template:**

```
Subject: Thank you - [Your Name] - Frontend Intern Interview

Dear [Interviewer Name],

Thank you for taking the time to speak with me today about 
the Frontend Intern position. 

I really enjoyed learning about [specific thing discussed] 
and I'm excited about the opportunity to contribute to 
[company name].

The discussion about [technical topic] was particularly 
interesting, and I'd love to be part of the team.

Please feel free to reach out if you need any additional 
information.

Thank you again for your time and consideration.

Best regards,
[Your Name]
[Phone]
[Email]
[GitHub]
```

---

## 📚 TÀI LIỆU THAM KHẢO

### Official Docs:
- [React Docs](https://react.dev)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Vite Guide](https://vitejs.dev/guide/)
- [Tailwind CSS Docs](https://tailwindcss.com/docs)
- [Vitest Docs](https://vitest.dev)

### Learning Resources:
- [JavaScript.info](https://javascript.info) - JS fundamentals
- [React Tutorial](https://react.dev/learn) - Step by step
- [TypeScript for Beginners](https://www.totaltypescript.com/tutorials/beginners-typescript)

### Practice:
- [Frontend Mentor](https://www.frontendmentor.io) - Projects
- [LeetCode](https://leetcode.com) - Algorithms (optional)

---

## 🎯 CHECKLIST TRƯỚC KHI NỘP CV

### Code:
- [ ] No console.logs
- [ ] No commented code
- [ ] Tests pass (7/7)
- [ ] App builds successfully
- [ ] No TypeScript errors

### Documentation:
- [ ] README.md updated
- [ ] Clear setup instructions
- [ ] Screenshots added
- [ ] Live demo link works

### GitHub:
- [ ] Clean commit history
- [ ] Descriptive commit messages
- [ ] No sensitive data (API keys)
- [ ] .gitignore proper

### Testing:
- [ ] Can explain every test
- [ ] Tests meaningful, not random
- [ ] Test names clear

---

## ✨ KẾT LUẬN

Bạn đã có:
- ✅ Clean, well-organized code
- ✅ Basic but solid testing
- ✅ Error handling
- ✅ Modern UX patterns
- ✅ Full understanding of your code

**Remember:**
- 📚 Honesty > Fake expertise
- 💪 Growth mindset > Perfect code
- 🎯 Explanation > Complex features

**You're ready! Good luck! 🚀**

---

*Guide được tạo dành riêng cho bạn. Đọc kỹ, practice, và tự tin!* 💙
