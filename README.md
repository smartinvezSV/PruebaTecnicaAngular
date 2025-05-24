# Prueba Técnica Frontend - Sistema de Gestión de Empleados TalentHub

## Contexto del Negocio

Eres el desarrollador frontend de **TalentHub**, una empresa de gestión de recursos humanos. Tu tarea es crear la interfaz web que permitirá a las empresas cliente gestionar sus empleados, departamentos y reportes de manera intuitiva y eficiente.

## Objetivo General

Desarrollar una aplicación web SPA (Single Page Application) con Angular 19 que consuma la API REST del backend de TalentHub, proporcionando una experiencia de usuario moderna, responsive y eficiente.

## Stack Tecnológico Requerido

- **Framework**: Angular 19 (última versión estable)
- **UI Library**: PrimeNG (última versión compatible)
- **Icons**: PrimeIcons
- **HTTP Client**: Angular HttpClient
- **State Management**: Angular Signals + RxJS
- **Routing**: Angular Router
- **Forms**: Reactive Forms
- **Styling**: SCSS + CSS Grid/Flexbox
- **Responsive**: Mobile-first approach

## Funcionalidades Requeridas

### 1. Sistema de Autenticación

- **Página de Login**: Form reactivo con validaciones
- **Página de Registro**: Para nuevas empresas
- **Guards**: Protección de rutas privadas
- **Token Management**: Almacenamiento y renovación de JWT
- **Auto-logout**: Por expiración de token

### 2. Dashboard Principal

- **Cards de resumen**: Total empleados, departamentos activos
- **Gráficos básicos**: Empleados por departamento (Chart.js via PrimeNG)
- **Accesos rápidos**: Links a funcionalidades principales
- **Responsive design**: Adaptable a mobile/tablet/desktop

### 3. Gestión de Departamentos

- **Lista de departamentos**: Tabla con PrimeNG DataTable
- **CRUD completo**: Crear, editar, eliminar departamentos
- **Modal forms**: Para crear/editar
- **Confirmaciones**: Dialog de confirmación para eliminar
- **Validaciones**: Campos requeridos, longitud

### 4. Gestión de Empleados

- **Lista de empleados**: DataTable con paginación, sorting, filtros
- **CRUD completo**: Crear, editar, eliminar empleados
- **Filtros avanzados**: Por departamento, estado, fecha de contratación
- **Búsqueda global**: Input de búsqueda en tiempo real
- **Carga masiva**: Upload de archivo Excel con progress bar
- **Vista detalle**: Modal o página separada con información completa

### 5. Navegación y UX

- **Sidebar responsive**: Menú lateral colapsable
- **Breadcrumbs**: Navegación contextual
- **Loading states**: Spinners y skeletons
- **Error handling**: Toast notifications para errores
- **Success feedback**: Confirmaciones de acciones exitosas

## Características Técnicas Específicas

### Angular 19 - Nueva Sintaxis de Control Flow

Usar obligatoriamente la nueva sintaxis:

```typescript
// Condicionales
@if (employees().length > 0) {
  <p-dataTable [value]="employees()">
    <!-- tabla content -->
  </p-dataTable>
} @else {
  <div class="empty-state">
    <p>No hay empleados registrados</p>
  </div>
}

// Switch
@switch (currentView()) {
  @case ('list') {
    <app-employee-list />
  }
  @case ('grid') {
    <app-employee-grid />
  }
  @default {
    <app-employee-list />
  }
}

// Loops
@for (employee of employees(); track employee.id) {
  <app-employee-card [employee]="employee" />
} @empty {
  <p>No se encontraron empleados</p>
}
```

### Signals y State Management

- **Signals** para estado local de componentes
- **Computed signals** para valores derivados
- **Effect()** para side effects
- **Signal inputs** y **outputs** en componentes

```typescript
export class EmployeeService {
  private employeesSignal = signal<Employee[]>([]);
  employees = this.employeesSignal.asReadonly();

  employeeCount = computed(() => this.employees().length);
  activeEmployees = computed(() =>
    this.employees().filter((emp) => emp.active),
  );
}
```

### RxJS y Observables

- **HttpClient** para llamadas a API
- **Operators**: map, filter, debounceTime, switchMap, catchError
- **BehaviorSubject** para estados globales cuando sea necesario
- **Async pipe** en templates
- **takeUntilDestroyed()** para auto-unsubscribe

## Páginas/Componentes Principales

### Estructura de Rutas

```
/auth/login
/auth/register
/dashboard
/departments
/departments/new
/departments/:id/edit
/employees
/employees/new
/employees/:id/edit
/employees/:id/detail
/employees/upload
```

### Componentes Requeridos

1. **AuthComponent** - Login/Register forms
2. **DashboardComponent** - Página principal con estadísticas
3. **DepartmentListComponent** - Lista y gestión de departamentos
4. **DepartmentFormComponent** - Formulario crear/editar departamento
5. **EmployeeListComponent** - Lista con filtros y búsqueda
6. **EmployeeFormComponent** - Formulario crear/editar empleado
7. **EmployeeDetailComponent** - Vista detallada de empleado
8. **FileUploadComponent** - Carga masiva de Excel
9. **LayoutComponent** - Layout principal con sidebar
10. **HeaderComponent** - Header con usuario y logout

## Diseño UI/UX Requirements

### PrimeNG Components a Utilizar

- **Button**: Botones con iconos y loading states
- **DataTable**: Tablas con sorting, filtering, pagination
- **Dialog**: Modales para forms y confirmaciones
- **Toast**: Notificaciones de éxito/error
- **ProgressBar**: Para uploads y loading
- **Calendar**: Date pickers
- **Dropdown**: Selects para departamentos
- **InputText**: Campos de texto
- **FileUpload**: Carga de archivos
- **Chart**: Gráficos en dashboard
- **Sidebar**: Menú lateral
- **MenuBar**: Navegación top

### Responsive Design

- **Mobile First**: Diseño pensado desde mobile
- **Breakpoints**:
  - Mobile: < 768px
  - Tablet: 768px - 1024px
  - Desktop: > 1024px
- **Sidebar colapsable** en mobile
- **DataTable responsive** con scroll horizontal
- **Grid layouts** adaptativos

## Validaciones y Manejo de Errores

### Validaciones de Forms

```typescript
employeeForm = this.fb.group({
  name: ['', [Validators.required, Validators.minLength(2)]],
  email: ['', [Validators.required, Validators.email]],
  department: ['', Validators.required],
  hireDate: ['', Validators.required],
  salary: ['', [Validators.required, Validators.min(0)]],
});
```

### Error Handling

- **HTTP Interceptor** para manejo global de errores
- **Toast notifications** para feedback de errores
- **Retry mechanism** para requests fallidos
- **Offline detection** y mensajes apropiados

## Criterios de Evaluación

### Código y Arquitectura (30%)

- Estructura limpia y modular
- Uso correcto de Angular 19 features
- Separación de responsabilidades
- Servicios bien definidos
- Lazy loading de módulos

### UI/UX y Responsive (25%)

- Diseño atractivo y profesional
- Uso apropiado de PrimeNG components
- Responsive design funcional
- Accesibilidad básica
- Loading states y feedback

### State Management (20%)

- Uso correcto de Signals
- Implementación apropiada de RxJS
- Manejo eficiente del estado
- Performance optimizations

### Funcionalidades (25%)

- Todas las funcionalidades implementadas
- Integración correcta con backend
- Validaciones funcionando
- Error handling robusto

## Entregables

1. **Código fuente** en repositorio Git
2. **README.md** con:
   - Instrucciones de instalación
   - Comandos para ejecutar el proyecto
   - Estructura del proyecto
   - Screenshots de la aplicación
3. **Configuración de environment** para diferentes entornos
4. **Build de producción** funcionando
5. **Responsive demo** - evidencia de funcionamiento en diferentes dispositivos

## Instrucciones Adicionales

1. **Tiempo estimado**: 4-6 horas
2. **Configuración**: Angular CLI con latest version
3. **Linting**: ESLint + Prettier configurado
4. **Styles**: SCSS con variables organizadas
5. **Git**: Commits descriptivos y frecuentes
6. **Comments**: Documentar solo lógica compleja

## Desafíos Adicionales (Bonus)

Si terminas antes del tiempo estimado, considera implementar:

### Funcionalidades Avanzadas

- **Tema dark/light**: Toggle de temas
- **Internacionalización**: Soporte para múltiples idiomas (es/en)
- **PWA features**: Service workers básicos
- **Offline mode**: Funcionalidad básica sin conexión

### Optimizaciones

- **OnPush change detection**: Para mejor performance
- **TrackBy functions**: En listas grandes
- **Lazy loading images**: Para avatars de empleados
- **Virtual scrolling**: Para listas muy grandes
- **Standalone components**: Migrar a componentes standalone

### Integración Avanzada

- **WebSockets**: Notificaciones en tiempo real
- **File preview**: Vista previa de archivos Excel antes de subir
- **Export features**: Exportar datos a Excel/PDF
- **Advanced charts**: Gráficos más complejos con filtros

## Preguntas para Reflexión

Al finalizar, prepárate para discutir:

1. ¿Por qué elegiste esta estructura de estado management?
2. ¿Cómo optimizarías la performance con listas de miles de empleados?
3. ¿Qué estrategias usaste para hacer la app accessible?
4. ¿Cómo manejarías la sincronización de datos en tiempo real?
5. ¿Qué consideraciones tuviste para el responsive design?

---

**¡Demuestra tus habilidades frontend y crea una experiencia de usuario excepcional!** 🎨✨
