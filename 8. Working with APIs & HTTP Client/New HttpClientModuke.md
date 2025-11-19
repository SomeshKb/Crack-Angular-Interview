# Modern Way to Add HttpClient in Angular (v15+)

Angular now provides a **module-less, modern API** to use HttpClient without importing `HttpClientModule`.
The new recommended method uses **`provideHttpClient()`**.

---

## ✅ 1. Add HttpClient in `main.ts`

```ts
import { bootstrapApplication } from '@angular/platform-browser';
import { AppComponent } from './app/app.component';
import { provideHttpClient } from '@angular/common/http';

bootstrapApplication(AppComponent, {
  providers: [
    provideHttpClient()
  ]
});
```

---

## 🟦 Optional: Register Interceptors (New Functional API)

```ts
import { provideHttpClient, withInterceptors } from '@angular/common/http';
import { myInterceptor } from './app/interceptor/my.interceptor';

bootstrapApplication(AppComponent, {
  providers: [
    provideHttpClient(
      withInterceptors([
        myInterceptor
      ])
    )
  ]
});
```

### Functional Interceptor Example

```ts
import { HttpInterceptorFn } from '@angular/common/http';

export const myInterceptor: HttpInterceptorFn = (req, next) => {
  const modified = req.clone({
    setHeaders: {
      Authorization: 'Bearer token'
    }
  });
  return next(modified);
};
```

---

## 🟩 Using HttpClient Inside a Standalone Component

```ts
@Component({
  standalone: true,
  selector: 'app-example',
  template: `<p>Example Works!</p>`,
  providers: [provideHttpClient()]
})
export class ExampleComponent {}
```

---

## ❌ Old Way (Deprecated Style)

```ts
import { HttpClientModule } from '@angular/common/http';

@NgModule({
  imports: [HttpClientModule]
})
export class AppModule {}
```

---

## ✔ Summary

| Feature           | Old Way            | New Recommended Way               |
| ----------------- | ------------------ | --------------------------------- |
| Import HttpClient | `HttpClientModule` | `provideHttpClient()`             |
| Add Interceptors  | Class-based        | Functional (`withInterceptors()`) |
| Requires Module?  | Yes                | No (Standalone Angular)           |

