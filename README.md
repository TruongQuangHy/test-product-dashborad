# Product Dashboard - Nuxt 3

A dashboard application displaying product lists built with Nuxt 3.

## ✨ Features

- 📱 **Responsive Design**: Works smoothly on all devices (mobile, tablet, desktop)
- 🖱️ **Interactive Cards**: Hover and click effects for product selection
- 📊 **Sales Summary**: Automatic statistics of quantity and total price of available products
- 🎯 **TypeScript**: Type-safe with Composition API

## 🛠️ Technologies Used

- **Nuxt 3**: Next-generation Vue.js framework
- **Vue 3**: Composition API with `<script setup lang="ts">`
- **TypeScript**: Type checking support

## 📋 System Requirements

- Node.js >= 18.x
- pnpm (recommended) or npm/yarn

## 🚀 Installation

1. **Clone or download the project**

2. **Navigate to project directory**

```bash
cd product-dashboard
```

3. **Install dependencies**

```bash
pnpm install
# or
npm install
# or
yarn install
```

## 💻 Running the Application

### Development mode

```bash
pnpm dev
# or
npm run dev
# or
yarn dev
```

The application will run at: `http://localhost:3000`

### Build production

```bash
pnpm build
# or
npm run build
# or
yarn build
```

### Preview production

```bash
pnpm preview
# or
npm run preview
# or
yarn preview
```

## 📂 Project Structure

```
product-dashboard/
├── assets/
│   └── css/
│       └── main.css          # Tailwind CSS imports
├── components/
│   └── ProductDashboard.vue  # Main component displaying products
├── data/
│   └── products.json         # Product data (mock data)
├── pages/
│   └── index.vue             # Home page
├── nuxt.config.ts            # Nuxt configuration
└── README.md                 # This file
```

## 🎯 Detailed Features

### ProductDashboard Component

1. **Display product list**:

   - Responsive grid: 1 column (mobile), 2 columns (tablet), 3 columns (desktop)
   - Each card displays: image, name, price, status, "View Details" button
   - Hover effects: scale + shadow

2. **Product selection**:

   - Click on card to select
   - Display selected product name at the top
   - Can deselect using X button

3. **Sales Summary**:
   - Automatically calculates number of products with status = "available"
   - Automatically calculates total value of available products
   - Displays as card with beautiful icons

## 🎨 Customization

### Adding new products

Edit the `/data/products.json` file:

```json
{
  "id": 9,
  "name": "Product Name",
  "price": 999,
  "status": "available",
  "image": "Image URL"
}
```

## 🚀 Deployment

### Before deploying

Make sure your lockfile is up to date:

```bash
# Delete node_modules and lockfile
rm -rf node_modules pnpm-lock.yaml

# Reinstall dependencies
pnpm install
```

### Common deployment issues

**Lockfile mismatch error:**

```
ERR_PNPM_OUTDATED_LOCKFILE Cannot install with "frozen-lockfile"
```

**Solution:**

```bash
# Option 1: Update lockfile locally and commit
pnpm install
git add pnpm-lock.yaml
git commit -m "Update pnpm lockfile"

# Option 2: Use --no-frozen-lockfile flag (CI environments)
pnpm install --no-frozen-lockfile
```

### Deployment platforms

**Vercel/Netlify:**

- Ensure `pnpm-lock.yaml` is committed to your repository
- Set Node.js version to >= 18.x in platform settings

**Manual deployment:**

```bash
# Build for production
pnpm build

# Start production server
pnpm preview
```

## 🛠️ Troubleshooting

### Lockfile issues

- Always commit `pnpm-lock.yaml` to version control
- Run `pnpm install` after pulling changes that modify `package.json`
- Use `pnpm install --frozen-lockfile` in CI environments

### Node.js version

- Ensure you're using Node.js >= 18.x
- Check version: `node --version`

## 👨‍💻 Author

Created with ❤️ using Nuxt 3
