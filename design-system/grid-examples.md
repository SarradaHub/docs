# Grid System Examples

**Purpose**: Common layout patterns using the 12-column grid system

**Related Documents**:
- [Grid System Guide](./grid-system-guide.md) - Grid specifications
- [Component Structure Template](./component-structure-template.md) - Component standards

---

## Two-Column Layout (Sidebar + Content)

### React Example

```tsx
export function DashboardPage() {
  return (
    <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
      <div className="grid grid-cols-1 md:grid-cols-12 gap-6">
        {/* Sidebar - 3 columns */}
        <aside className="md:col-span-3">
          <Navigation />
        </aside>
        
        {/* Main content - 9 columns */}
        <main className="md:col-span-9">
          <h1 className="text-3xl font-bold mb-6">Dashboard</h1>
          {/* Content */}
        </main>
      </div>
    </div>
  );
}
```

### Rails Example

```erb
<div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
  <div class="grid grid-cols-1 md:grid-cols-12 gap-6">
    <!-- Sidebar - 3 columns -->
    <aside class="md:col-span-3">
      <%= render 'shared/navigation' %>
    </aside>
    
    <!-- Main content - 9 columns -->
    <main class="md:col-span-9">
      <h1 class="text-3xl font-bold mb-6">Dashboard</h1>
      <%= yield %>
    </main>
  </div>
</div>
```

---

## Three-Column Layout

### React Example

```tsx
export function AdminPage() {
  return (
    <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
      <div className="grid grid-cols-1 md:grid-cols-12 gap-6">
        {/* Left sidebar - 2 columns */}
        <aside className="md:col-span-2">
          <AdminNavigation />
        </aside>
        
        {/* Main content - 8 columns */}
        <main className="md:col-span-8">
          <h1 className="text-3xl font-bold mb-6">Admin Panel</h1>
          {/* Content */}
        </main>
        
        {/* Right sidebar - 2 columns */}
        <aside className="md:col-span-2">
          <QuickActions />
        </aside>
      </div>
    </div>
  );
}
```

### Rails Example

```erb
<div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
  <div class="grid grid-cols-1 md:grid-cols-12 gap-6">
    <!-- Left sidebar - 2 columns -->
    <aside class="md:col-span-2">
      <%= render 'admin/navigation' %>
    </aside>
    
    <!-- Main content - 8 columns -->
    <main class="md:col-span-8">
      <h1 class="text-3xl font-bold mb-6">Admin Panel</h1>
      <%= yield %>
    </main>
    
    <!-- Right sidebar - 2 columns -->
    <aside class="md:col-span-2">
      <%= render 'admin/quick_actions' %>
    </aside>
  </div>
</div>
```

---

## Card Grid Layout

### React Example

```tsx
export function ProductsPage({ products }) {
  return (
    <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
      <h1 className="text-3xl font-bold mb-8">Products</h1>
      
      {/* Card grid - responsive */}
      <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
        {products.map((product) => (
          <ProductCard key={product.id} product={product} />
        ))}
      </div>
    </div>
  );
}
```

### Rails Example

```erb
<div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
  <h1 class="text-3xl font-bold mb-8">Products</h1>
  
  <!-- Card grid - responsive -->
  <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
    <% @products.each do |product| %>
      <%= render 'products/card', product: product %>
    <% end %>
  </div>
</div>
```

---

## Form Layout

### React Example

```tsx
export function CreateFormPage() {
  return (
    <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
      <h1 className="text-3xl font-bold mb-8">Create Item</h1>
      
      <form className="grid grid-cols-1 md:grid-cols-12 gap-6">
        {/* Full width field */}
        <div className="md:col-span-12">
          <Input label="Title" name="title" required />
        </div>
        
        {/* Two column fields */}
        <div className="md:col-span-6">
          <Input label="First Name" name="firstName" required />
        </div>
        <div className="md:col-span-6">
          <Input label="Last Name" name="lastName" required />
        </div>
        
        {/* Three column fields */}
        <div className="md:col-span-4">
          <Input label="City" name="city" />
        </div>
        <div className="md:col-span-4">
          <Input label="State" name="state" />
        </div>
        <div className="md:col-span-4">
          <Input label="Zip" name="zip" />
        </div>
        
        {/* Full width textarea */}
        <div className="md:col-span-12">
          <Textarea label="Description" name="description" rows={4} />
        </div>
        
        {/* Actions */}
        <div className="md:col-span-12 flex gap-4">
          <Button variant="primary" type="submit">Save</Button>
          <Button variant="outline" type="button">Cancel</Button>
        </div>
      </form>
    </div>
  );
}
```

### Rails Example

```erb
<div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
  <h1 class="text-3xl font-bold mb-8">Create Item</h1>
  
  <%= form_with model: @item, local: true, class: "grid grid-cols-1 md:grid-cols-12 gap-6" do |f| %>
    <!-- Full width field -->
    <div class="md:col-span-12">
      <%= f.label :title, class: "block text-sm font-medium text-neutral-700 mb-1" %>
      <%= f.text_field :title, class: "w-full px-3 py-2 border border-neutral-300 rounded-md", required: true %>
    </div>
    
    <!-- Two column fields -->
    <div class="md:col-span-6">
      <%= f.label :first_name, class: "block text-sm font-medium text-neutral-700 mb-1" %>
      <%= f.text_field :first_name, class: "w-full px-3 py-2 border border-neutral-300 rounded-md", required: true %>
    </div>
    <div class="md:col-span-6">
      <%= f.label :last_name, class: "block text-sm font-medium text-neutral-700 mb-1" %>
      <%= f.text_field :last_name, class: "w-full px-3 py-2 border border-neutral-300 rounded-md", required: true %>
    </div>
    
    <!-- Actions -->
    <div class="md:col-span-12 flex gap-4">
      <%= f.submit "Save", class: "px-4 py-2 bg-primary-600 text-white rounded-md hover:bg-primary-700" %>
      <%= link_to "Cancel", items_path, class: "px-4 py-2 bg-neutral-200 text-neutral-900 rounded-md hover:bg-neutral-300" %>
    </div>
  <% end %>
</div>
```

---

## Dashboard Layout

### React Example

```tsx
export function DashboardPage() {
  return (
    <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
      {/* Header */}
      <div className="mb-8">
        <h1 className="text-3xl font-bold">Dashboard</h1>
        <p className="text-neutral-600 mt-2">Welcome back!</p>
      </div>
      
      {/* Stats grid - 4 columns */}
      <div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
        <StatCard title="Users" value="1,234" />
        <StatCard title="Revenue" value="$12,345" />
        <StatCard title="Growth" value="+12%" />
        <StatCard title="Active" value="567" />
      </div>
      
      {/* Main content grid */}
      <div className="grid grid-cols-1 md:grid-cols-12 gap-6">
        {/* Chart - 8 columns */}
        <div className="md:col-span-8">
          <Card>
            <CardHeader>
              <CardTitle>Revenue Chart</CardTitle>
            </CardHeader>
            <CardContent>
              {/* Chart component */}
            </CardContent>
          </Card>
        </div>
        
        {/* Recent activity - 4 columns */}
        <div className="md:col-span-4">
          <Card>
            <CardHeader>
              <CardTitle>Recent Activity</CardTitle>
            </CardHeader>
            <CardContent>
              {/* Activity list */}
            </CardContent>
          </Card>
        </div>
      </div>
    </div>
  );
}
```

---

## Responsive Breakpoints

### Breakpoint Strategy

```tsx
// Mobile-first approach
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4 sm:gap-6">
  {/* 
    - Mobile (< 640px): 1 column
    - Small (≥ 640px): 2 columns
    - Large (≥ 1024px): 3 columns
    - XL (≥ 1280px): 4 columns
  */}
</div>
```

### Common Responsive Patterns

**Cards**:
```tsx
// 1 col mobile → 2 col tablet → 3 col desktop → 4 col xl
grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4
```

**Sidebar Layout**:
```tsx
// Full width mobile → Sidebar + content desktop
grid-cols-1 md:grid-cols-12
// Sidebar: md:col-span-3
// Content: md:col-span-9
```

**Form Fields**:
```tsx
// Full width mobile → 2 columns desktop
md:col-span-6
// Full width mobile → 3 columns desktop
md:col-span-4
```

---

## Nested Grids

### React Example

```tsx
export function ComplexLayout() {
  return (
    <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">
      {/* Outer grid */}
      <div className="grid grid-cols-1 md:grid-cols-12 gap-6">
        <aside className="md:col-span-3">
          Sidebar
        </aside>
        
        <main className="md:col-span-9">
          {/* Nested grid */}
          <div className="grid grid-cols-1 lg:grid-cols-2 gap-6">
            <Card>Card 1</Card>
            <Card>Card 2</Card>
            <Card>Card 3</Card>
            <Card>Card 4</Card>
          </div>
        </main>
      </div>
    </div>
  );
}
```

---

## Resources

- [Grid System Guide](./grid-system-guide.md) - Grid specifications
- [Component Structure Template](./component-structure-template.md) - Component standards
- [Phase 3 Standardization](./phase-3-standardization.md) - Overall standards

---

**Last Updated**: 2025-01-XX  
**Maintained By**: Design System Team

