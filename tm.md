npm run dev
npm run lint
task-manager/
# 📚 HƯỚNG DẪN CHI TIẾT DỰ ÁN TASK MANAGER
## Dành cho người mới bắt đầu

---

## 📖 MỤC LỤC

1. [Tổng Quan Dự Án](#1-tổng-quan-dự-án)
2. [Cài Đặt & Chạy](#2-cài-đặt--chạy)
3. [Cấu Trúc Thư Mục](#3-cấu-trúc-thư-mục)
4. [Giải Thích Tech Stack](#4-giải-thích-tech-stack)
5. [Giải Thích Code Chi Tiết](#5-giải-thích-code-chi-tiết)
6. [Testing - Kiểm Thử](#6-testing---kiểm-thử)
7. [Drag & Drop Workflow](#7-drag--drop-workflow)
8. [Empty State & UX](#8-empty-state--ux)
9. [Câu Hỏi Thường Gặp](#9-câu-hỏi-thường-gặp)
10. [Tips Cho Phỏng Vấn](#10-tips-cho-phỏng-vấn)

---

## 1. TỔNG QUAN DỰ ÁN

### 1.1 Dự án này là gì?

Đây là **ứng dụng quản lý công việc** dạng to-do list hiện đại. User có thể:
- ✅ Tạo công việc mới với feedback tức thì
- ✏️ Đánh dấu hoàn thành hoặc đang làm
- 🔄 Kéo thả để sắp xếp thứ tự ưu tiên
- 🔍 Lọc theo trạng thái (tất cả / đang làm / đã xong)
- 🌗 Chuyển đổi dark mode và lưu lựa chọn
- 💾 Lưu toàn bộ dữ liệu vào localStorage, không cần backend

### 1.2 Tại sao làm dự án này?

**Mục đích cho CV**:
- ✅ Chứng minh biết quản lý state với React hooks
- ✅ Chứng minh hiểu browser APIs (localStorage, drag & drop)
- ✅ Thể hiện khả năng thiết kế UI responsive + dark mode
- ✅ Cho thấy biết tách logic thành custom hooks để tái sử dụng
- ✅ Minh họa cách tạo UX thân thiện với empty state, counter

### 1.3 Demo & Repo

- Live demo: https://task-manager-ebon-eight-60.vercel.app
- Repository: https://github.com/nbv9704/task-manager

---

## 2. CÀI ĐẶT & CHẠY

### 2.1 Yêu cầu

Cần chuẩn bị trước:
- **Node.js** (phiên bản 20 trở lên) - [Tải tại đây](https://nodejs.org)
- **Git** - [Tải tại đây](https://git-scm.com)
- **VS Code** + các extension gợi ý: Tailwind CSS IntelliSense, ESLint

### 2.2 Các bước cài đặt

#### Bước 1: Clone project
```bash
git clone https://github.com/nbv9704/task-manager.git
cd task-manager
```

**Giải thích**:
- `git clone`: Lấy toàn bộ source về máy
- `cd task-manager`: Di chuyển vào thư mục dự án

#### Bước 2: Cài đặt dependencies
```bash
npm install
```

**Giải thích**:
- `npm install` đọc `package.json` và tải các thư viện cần thiết
- Thư viện sẽ được đặt trong thư mục `node_modules`

#### Bước 3: Chạy ứng dụng
```bash
npm run dev
```

**Giải thích**:
- Khởi chạy Vite dev server trên `http://localhost:5173`
- Hỗ trợ hot reload khi bạn chỉnh sửa code

### 2.3 Các lệnh quan trọng

```bash
# Build production + kiểm tra type
npm run build

# Preview build production
npm run preview

# Chạy ESLint để lint code
npm run lint
```

---

## 3. CẤU TRÚC THƯ MỤC

### 3.1 Tổng quan

```

├── src/
│   ├── components/
│   │   ├── FilterButtons.tsx
│   │   ├── TaskInput.tsx
│   │   ├── TaskItem.tsx
│   │   ├── TaskList.tsx
│   │   └── ThemeToggle.tsx
│   ├── hooks/
│   │   ├── useLocalStorage.ts
│   │   └── useTheme.ts
│   ├── types/
│   │   └── task.ts
│   ├── App.tsx
│   ├── main.tsx
│   ├── index.css
│   └── App.css
├── public/
├── package.json
├── tailwind.config.js
├── postcss.config.js
└── vite.config.ts
```

### 3.2 Giải thích từng thư mục

#### 📁 `src/components/`
Chứa các UI components:
- `TaskInput`: Form tạo công việc mới với validation
- `FilterButtons`: Bộ lọc trạng thái + bộ đếm số lượng
- `TaskList`: Bao bọc drag & drop, render danh sách
- `TaskItem`: Item đơn lẻ (checkbox, nút xoá)
- `ThemeToggle`: Nút bật/tắt dark mode

#### 📁 `src/hooks/`
- `useLocalStorage`: Custom hook đồng bộ state với localStorage
- `useTheme`: Quản lý theme (ưu tiên localStorage, fallback system)

#### 📁 `src/types/`
- `task.ts`: Khai báo interface `Task` và union `FilterType`

#### 📁 `src`
- `App.tsx`: Component gốc, nối các phần lại
- `main.tsx`: Entry point, mount React vào DOM
- `index.css`: Khai báo Tailwind + style global
- `App.css`: Custom style bổ sung (gradient, animation nhẹ)

---

## 4. GIẢI THÍCH TECH STACK

### 4.1 React - UI Library

- **React** giúp xây UI theo component. Mọi logic (thêm/xoá/kéo thả) đều đặt trong hooks + component nhỏ.
- Sử dụng React 19 → sẵn sàng cho concurrent features, useActionState... (dù dự án chưa cần ngay).

### 4.2 TypeScript - Type Safety

- TypeScript đảm bảo mọi task luôn có `id`, `title`, `completed`, `createdAt`.
- Lỗi type (ví dụ cố gắng gán boolean cho `title`) bị bắt ngay khi build.
- Tạo trải nghiệm autocomplete tuyệt vời trong VS Code.

### 4.3 Tailwind CSS - Styling

- Viết style nhanh bằng utility class (`bg-gray-900`, `rounded-xl`...).
- Dark mode chỉ cần thêm lớp `dark:` và toggle class `dark` ở `<html>`.
- Đảm bảo responsive nhanh với `sm:`, `md:`, `lg:`.

### 4.4 @hello-pangea/dnd - Drag & Drop

- Thư viện fork từ react-beautiful-dnd, tối ưu cho React 18+.
- Có hỗ trợ keyboard (Space + Arrow), tự động thêm aria attributes.
- Giảm thiểu việc tự xử lý DOM events phức tạp.

### 4.5 Lucide React - Icon

- Bộ icon nhẹ, hiện đại → thêm cảm giác polished (ví dụ icon mặt trời/trăng cho theme).

### 4.6 Vite - Build Tool

- Khởi động <1s, HMR mượt.
- Config đơn giản: chỉ cần plugin React + alias cơ bản.

### 4.7 ESLint + TypeScript ESLint

- Giữ code sạch, tránh bug do quên dependencies trong hook, quên handle promise.

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

**Giải thích:**
- Khởi tạo React root và mount `<App />` vào thẻ `div#root` trong `index.html`.
- `React.StrictMode` giúp phát hiện side effect không an toàn (chỉ tác động ở dev).

### 5.2 Component Chính: `App.tsx`

```tsx
function App() {
  const [tasks, setTasks] = useLocalStorage<Task[]>('tasks', []);
  const [filter, setFilter] = useState<FilterType>('all');
  const { isDark, toggleTheme } = useTheme();

  const addTask = (title: string) => {
    const newTask: Task = {
      id: crypto.randomUUID(),
      title,
      completed: false,
      createdAt: Date.now(),
    };
    setTasks([newTask, ...tasks]);
  };

  // ...toggleTask, deleteTask, reorderTasks

  const filteredTasks = useMemo(() => {
    switch (filter) {
      case 'active':
        return tasks.filter((task) => !task.completed);
      case 'completed':
        return tasks.filter((task) => task.completed);
      default:
        return tasks;
    }
  }, [tasks, filter]);

  return (
    <div className="min-h-screen bg-gray-50 dark:bg-gray-900">
      <ThemeToggle isDark={isDark} onToggle={toggleTheme} />
      {/* ... */}
    </div>
  );
}
```

**Điểm nổi bật:**
- `useLocalStorage` giúp dữ liệu tồn tại qua mỗi lần reload.
- `crypto.randomUUID()` bảo đảm ID duy nhất, tránh bug khi kéo thả.
- `useMemo` tránh filter lại danh sách khi không cần.
- Phần render chia layout rõ: tiêu đề → input → filter → list → counter.

### 5.3 Component `TaskList.tsx`

```tsx
const handleDragEnd = (result: DropResult) => {
  if (!result.destination) return;
  onReorder(result.source.index, result.destination.index);
};

return (
  <DragDropContext onDragEnd={handleDragEnd}>
    <Droppable droppableId="tasks">
      {(provided, snapshot) => (
        <div
          {...provided.droppableProps}
          ref={provided.innerRef}
          className={`space-y-2 transition-colors ${
            snapshot.isDraggingOver ? 'bg-blue-50 dark:bg-blue-900/20 rounded-lg p-2' : ''
          }`}
        >
          {tasks.map((task, index) => (
            <Draggable key={task.id} draggableId={task.id} index={index}>
              {(provided, snapshot) => (
                <div
                  ref={provided.innerRef}
                  {...provided.draggableProps}
                  {...provided.dragHandleProps}
                  style={{
                    ...provided.draggableProps.style,
                    cursor: snapshot.isDragging ? 'grabbing' : 'grab',
                  }}
                >
                  <TaskItem task={task} onToggle={onToggle} onDelete={onDelete} />
                </div>
              )}
            </Draggable>
          ))}
          {provided.placeholder}
        </div>
      )}
    </Droppable>
  </DragDropContext>
);
```

**Giải thích:**
- `handleDragEnd` chỉ update state khi có `destination` hợp lệ → tránh crash khi kéo ra ngoài.
- `snapshot.isDraggingOver` đổi background, giúp user biết đang thả vào đâu.
- `cursor: grab/grabbing` tăng trải nghiệm kéo thả.
- `provided.placeholder` giữ khoảng trống để layout không bị nhảy.

### 5.4 Custom Hook `useLocalStorage.ts`

```typescript
const [storedValue, setStoredValue] = useState<T>(() => {
  try {
    const item = window.localStorage.getItem(key);
    return item ? JSON.parse(item) : initialValue;
  } catch (error) {
    console.error(error);
    return initialValue;
  }
});

const setValue = (value: T | ((val: T) => T)) => {
  try {
    const valueToStore = value instanceof Function ? value(storedValue) : value;
    setStoredValue(valueToStore);
    window.localStorage.setItem(key, JSON.stringify(valueToStore));
  } catch (error) {
    console.error(error);
  }
};
```

- Dùng lazy initializer để chỉ đọc localStorage 1 lần.
- Hỗ trợ cả set trực tiếp và set bằng callback (giống `useState`).
- Có try/catch để tránh app crash nếu localStorage bị khoá.

### 5.5 Hook `useTheme.ts`

- Kiểm tra localStorage → nếu chưa có thì nhìn `prefers-color-scheme` của hệ điều hành.
- Thêm/bớt class `dark` ở `<html>` → Tailwind tự động apply màu.
- Lưu lại lựa chọn của user để lần sau mở vẫn giữ theme.

### 5.6 Type `Task`

```typescript
export interface Task {
  id: string;
  title: string;
  completed: boolean;
  createdAt: number;
}

export type FilterType = 'all' | 'active' | 'completed';
```

- Interface rõ ràng để mọi component dùng chung một chuẩn.
- Khi switch-case với `FilterType`, TypeScript sẽ cảnh báo nếu quên handle một nhánh.

---

## 6. TESTING - KIỂM THỬ

### 6.1 Tình hình hiện tại

- Dự án *chưa có test tự động*. Nhưng bạn nên bổ sung để portfolio “xịn” hơn.
- Gợi ý: sử dụng **Vitest** + **React Testing Library** (giống dự án Movie Search).

### 6.2 Nên test gì?

1. **Utils**: Tách hàm reorder ra `utils/reorder.ts` rồi test input/output.
2. **TaskInput**: Đảm bảo không thêm task rỗng, sau khi submit thì input clear.
3. **TaskList**: Mock `onReorder` để chắc chắn callback chạy đúng index.
4. **ThemeToggle**: Kiểm tra khi click thì `localStorage` lưu `theme` mới.

### 6.3 Ví dụ test utils (gợi ý)

```typescript
import { describe, it, expect } from 'vitest';
import { reorderTasks } from '../utils/reorder';

describe('reorderTasks', () => {
  it('should move item to new index', () => {
    const list = ['a', 'b', 'c'];
    expect(reorderTasks(list, 0, 2)).toEqual(['b', 'c', 'a']);
  });
});
```

### 6.4 Checklist trước khi viết test

- [ ] Tách logic khỏi component nếu có thể (ví dụ filter, reorder).
- [ ] Chuẩn bị mock data nhỏ gọn.
- [ ] Dọn `console.log` để output test sạch sẽ.

---

## 7. DRAG & DROP WORKFLOW

### 7.1 Library hoạt động thế nào?

- `DragDropContext` lắng nghe toàn bộ drag event.
- `Droppable` định nghĩa vùng drop (ở đây là container tasks).
- `Draggable` wrap từng task, cung cấp `dragHandleProps` để kéo.

### 7.2 Luồng sự kiện

1. User click/giữ vào task → thư viện thêm style `position: fixed` khi kéo.
2. Khi di chuyển, `snapshot.isDragging` = true → đổi background, cursor.
3. Khi thả, `onDragEnd` trả về `source` và `destination`.
4. Dựa vào hai index này, ta cập nhật lại mảng `tasks` và lưu vào localStorage.

### 7.3 Edge case cần nhớ

- Khi kéo ra ngoài vùng droppable → `destination` = null → *không* update state.
- Khi danh sách rỗng → hiển thị empty state, tránh render DragDropContext rỗng.
- ID phải unique → dùng `crypto.randomUUID()` thay vì index.

---

## 8. EMPTY STATE & UX

- Khi chưa có task, xuất hiện message “No tasks yet. Add one above! 👆” → hướng dẫn user.
- Filter buttons hiển thị badge số lượng → user biết đang có bao nhiêu task.
- Counter cuối trang: “X of Y tasks completed” → tạo cảm giác tiến bộ.
- ThemeToggle floating ở góc phải → dễ truy cập mà không chặn nội dung.
- Animation nhẹ (hover, shadow) tạo cảm giác sản phẩm polished.

---

## 9. CÂU HỎI THƯỜNG GẶP

### Q1: Tại sao không dùng backend?

**A:** Mục tiêu là chứng minh kỹ năng frontend. LocalStorage đủ cho cá nhân, phản hồi nhanh. Nếu mở rộng cho nhiều user, có thể add Supabase / Firebase / NestJS API.

### Q2: Drag & drop có hỗ trợ bàn phím không?

**A:** `@hello-pangea/dnd` hỗ trợ phím Space + mũi tên. Hãy đề cập rằng bạn đã kiểm tra và sẵn sàng thêm aria-label nếu cần.

### Q3: Làm sao tránh duplicate task?

**A:** Hiện tại cho phép trùng tên (phù hợp với to-do cá nhân). Nếu cần, có thể trim string, so sánh case-insensitive trước khi push vào mảng.

### Q4: Có thể thêm deadline/reminder không?

**A:** Hoàn toàn được. Có thể dùng `date-fns` để format ngày, hoặc tích hợp calendar API. Ghi vào roadmap để thể hiện tầm nhìn.

### Q5: Làm sao backup dữ liệu?

**A:** Có thể thêm nút “Export JSON” (convert tasks sang file .json) và “Import JSON” khi mở rộng.

---

## 10. TIPS CHO PHỎNG VẤN

### 10.1 Câu hỏi thường gặp & gợi ý trả lời

**Q: “Giới thiệu dự án Task Manager này đi.”**
```
"Đây là ứng dụng quản lý công việc cá nhân xây bằng React + TypeScript.

Feature chính:
- CRUD task với localStorage
- Drag & drop reorder bằng @hello-pangea/dnd
- Dark mode và filter theo trạng thái

Em tách logic vào custom hooks như useLocalStorage, useTheme
để code sạch và dễ tái sử dụng."
```

**Q: “Vì sao chọn drag & drop thư viện này?”**
```
"@hello-pangea/dnd là fork maintained của react-beautiful-dnd,
đã hỗ trợ React 18, có keyboard accessibility sẵn và API quen thuộc.
Em ưu tiên tốc độ deliver và trải nghiệm user ổn định."
```

**Q: “Nếu thêm backend, em sẽ làm gì?”**
```
"Em sẽ tạo REST API (ví dụ NestJS) hoặc dùng Supabase.
Task sẽ sync theo userId, thêm auth, optimistic update để không phá UX."
```

### 10.2 Demo dự án hiệu quả

**Chuẩn bị:**
1. Chạy `npm run dev`, mở sẵn tab demo.
2. Dọn `console.log`, kiểm tra dark mode hoạt động.
3. Chuẩn bị vài task mẫu để show drag & drop.

**Trong buổi demo:**
- 30s giới thiệu tổng quan.
- 2 phút thao tác: tạo task, kéo thả, filter, đổi theme.
- 1 phút mở VS Code, show `useLocalStorage` + `TaskList`.
- Nếu được hỏi về test → chia sẻ kế hoạch ở trên.

**Tránh:**
- Không xin lỗi vì chưa có backend, hãy nhấn mạnh phạm vi FE.
- Không tự so sánh với Notion/Trello, tập trung vào điều bạn làm tốt.

### 10.3 Body Language & Communication

- Ngồi thẳng, nhìn camera, nở nụ cười nhẹ.
- Nói mạch lạc, tránh “à… ờ…”.
- Khi không biết câu trả lời, thành thật và đề xuất cách tìm hiểu.

### 10.4 Follow-up sau phỏng vấn

```
Subject: Thank you - [Your Name] - Frontend Intern Interview

Dear [Interviewer Name],

Thank you for the insightful conversation about the Frontend Intern position.
I enjoyed discussing the Task Manager project and would love to contribute to [Company].

Please let me know if you need any additional information.

Best regards,
[Your Name]
[Phone] | [Email] | [GitHub]
```

---

## 📚 TÀI LIỆU THAM KHẢO

### Official Docs:
- [React Docs](https://react.dev)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Tailwind CSS Docs](https://tailwindcss.com/docs)
- [@hello-pangea/dnd](https://github.com/hello-pangea/dnd)
- [Vite Guide](https://vitejs.dev/guide/)

### Learning Resources:
- [TodoMVC](https://todomvc.com) – Tham khảo nhiều cách build to-do
- [Total TypeScript](https://www.totaltypescript.com) – Học TS thực chiến
- [CSS Tricks](https://css-tricks.com) – Ý tưởng hover, animation

---

## 🎯 CHECKLIST TRƯỚC KHI NỘP CV

### Code:
- [ ] Không còn `console.log`
- [ ] Không để code comment vô nghĩa
- [ ] `npm run lint` không báo lỗi
- [ ] `npm run build` pass, không lỗi TypeScript

### Documentation:
- [ ] README cập nhật, có screenshot light/dark mode
- [ ] Live demo (Vercel) hoạt động ổn định
- [ ] Mô tả ngắn gọn trong README + portfolio

### GitHub:
- [ ] Commit message rõ ràng (“Add drag drop support”)
- [ ] Đã ignore `node_modules`, `.env`
- [ ] Có phần “Future Work” để thể hiện tầm nhìn

### Testing:
- [ ] (Tuỳ chọn) Thêm ít nhất 1-2 unit test để nâng giá trị
- [ ] Ghi chú manual test: mobile, desktop, dark mode

---

## ✨ KẾT LUẬN

Bạn đã có:
- ✅ Khả năng xây một ứng dụng CRUD hoàn chỉnh
- ✅ Hiểu rõ cách quản lý state, side-effect và persistence
- ✅ UX thân thiện: drag & drop, counter, dark mode
- ✅ Kế hoạch mở rộng rõ ràng, thể hiện tư duy sản phẩm

**Remember:**
- 📚 Trung thực về phạm vi, nhấn mạnh điều mình đã làm tốt
- 💪 Chủ động đề xuất hướng phát triển tiếp theo
- 🎯 Khi demo, tập trung vào trải nghiệm người dùng và logic chính

**Chúc bạn tự tin khoe dự án Task Manager trong CV! 🚀**

---

*Guide được biên soạn riêng cho bạn. Đọc kỹ, luyện tập, và shine trong buổi phỏng vấn nhé!* 💙
