# 🧁 Cakesman Bakery - E-Commerce Platform

A modern, full-stack e-commerce platform for an online bakery built with React, Node.js, Express, and MongoDB.

## 🌐 Live Demo

- **Frontend**: [https://cakesman-bakery.vercel.app](https://cakesman-bakery.vercel.app)
- **Admin Panel**: [https://cakesman-bakery.vercel.app/admin](https://cakesman-bakery.vercel.app/admin)

### Demo Credentials
- **Admin Email**: `admin@cakesman.com`
- **Admin Password**: `admin123`

## 🎯 Features

### Customer Features
- 🏠 **Beautiful Homepage** with hero section, categories, and bestsellers
- 🛍️ **Product Catalog** with filtering by categories and subcategories
- 🛒 **Shopping Cart** with persistent storage
- 💳 **Checkout Process** with delivery options
- 👤 **Customer Authentication** (Login/Register)
- ⭐ **Product Reviews and Ratings**
- 📱 **Fully Responsive Design** - Mobile, tablet, and desktop

### Admin Features
- 📊 **Admin Dashboard** with statistics and analytics
- 📦 **Product Management** (Create, Read, Update, Delete)
- 📋 **Order Management** - View and track orders
- 👥 **Customer Management**
- 📈 **Sales Analytics** and insights

## 🛠️ Tech Stack

### Frontend
- **React 18.3.1** - UI library
- **React Router v6** - Client-side routing
- **Tailwind CSS** - Styling
- **Axios** - HTTP client
- **Sonner** - Toast notifications
- **Lucide React** - Icons
- **Context API** - State management

### Backend
- **Node.js** - Runtime
- **Express.js** - Web framework
- **MongoDB** - Database
- **Mongoose** - ODM
- **JWT** - Authentication
- **bcryptjs** - Password hashing

## 📁 Project Structure

```
Cakesman-Bakery/
├── frontend/                 # React application
│   ├── src/
│   │   ├── components/      # Reusable components
│   │   ├── pages/           # Page components
│   │   ├── context/         # Context API
│   │   ├── data/            # Constants and data
│   │   ├── utils/           # Utility functions
│   │   └── App.jsx          # Main app component
│   └── package.json
│
└── backend/                  # Node.js server
    ├── controllers/         # Route handlers
    ├── models/             # MongoDB schemas
    ├── routes/             # API routes
    ├── middleware/         # Custom middleware
    ├── config/             # Configuration files
    ├── server.js           # Entry point
    └── package.json
```

## 🚀 Getting Started

### Prerequisites
- Node.js v16+
- npm or yarn
- MongoDB (local or Atlas)
- Git

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/cakesman-bakery.git
cd Cakesman-Bakery
```

2. **Setup Backend**
```bash
cd backend
npm install

# Create .env file
echo "MONGODB_URI=your_mongodb_uri
PORT=5001
JWT_SECRET=your_jwt_secret" > .env

npm start
```

Backend will run on `http://localhost:5001`

3. **Setup Frontend**
```bash
cd frontend
npm install

npm start
```

Frontend will run on `http://localhost:3000`

## 📊 Database Schema

### Users Collection
```javascript
{
  _id: ObjectId,
  email: String (unique),
  password: String (hashed),
  fullName: String,
  phone: String,
  address: String,
  role: String ('customer' or 'admin'),
  createdAt: Date
}
```

### Products Collection
```javascript
{
  _id: ObjectId,
  name: String,
  description: String,
  basePrice: Number,
  categoryId: String,
  subcategoryId: String,
  image: String,
  inStock: Boolean,
  featured: Boolean,
  discount: Number,
  rating: Number,
  reviews: Number,
  deliveryTime: String,
  tags: [String],
  createdAt: Date,
  updatedAt: Date
}
```

### Orders Collection
```javascript
{
  _id: ObjectId,
  customerId: ObjectId (ref: User),
  totalAmount: Number,
  status: String ('pending', 'confirmed', 'preparing', 'delivered'),
  paymentStatus: String ('pending', 'completed', 'failed'),
  items: [
    {
      productId: ObjectId,
      name: String,
      flavor: String,
      size: String,
      quantity: Number,
      price: Number
    }
  ],
  deliveryAddress: String,
  deliveryDate: Date,
  specialInstructions: String,
  createdAt: Date,
  updatedAt: Date
}
```

## 🔐 Authentication

The project uses JWT (JSON Web Tokens) for authentication:

- **Customer Login**: Email + Password
- **Admin Login**: Demo credentials (admin@cakesman.com / admin123)
- **Protected Routes**: Admin dashboard requires admin role
- **Token Storage**: Stored in localStorage

### Demo Credentials
```
Admin Email: admin@cakesman.com
Admin Password: admin123
```

## 📱 Available Pages

### Public Pages
- `/` - Homepage
- `/category/:categoryId` - Category products
- `/login` - Login/Register page

### Protected Pages (Requires Authentication)
- `/admin` - Admin dashboard (Admin only)
- `/checkout` - Checkout page (Customers)

## 🎨 Design System

### Colors
- **Primary Pink**: #EC4899
- **Dark Amber**: #92400E
- **Cream**: #FEF3C7
- **White**: #FFFFFF

### Typography
- **Headings**: 48px (desktop), 36px (mobile)
- **Body**: 16px
- **Small**: 14px

## 🔗 API Endpoints

### Authentication
- `POST /api/auth/register` - Register new customer
- `POST /api/auth/login` - Login user
- `POST /api/auth/logout` - Logout user

### Products
- `GET /api/products` - Get all products
- `GET /api/products/:id` - Get product by ID
- `POST /api/products` - Create product (Admin only)
- `PUT /api/products/:id` - Update product (Admin only)
- `DELETE /api/products/:id` - Delete product (Admin only)

### Orders
- `GET /api/orders` - Get user's orders
- `POST /api/orders` - Create order
- `GET /api/orders/:id` - Get order details
- `PUT /api/orders/:id` - Update order status (Admin only)

### Cart
- `GET /api/cart` - Get user's cart
- `POST /api/cart` - Add to cart
- `PUT /api/cart/:itemId` - Update cart item
- `DELETE /api/cart/:itemId` - Remove from cart

## 🧪 Testing

```bash
# Frontend
cd frontend
npm test

# Backend
cd backend
npm test
```

## 📦 Deployment

### Frontend (Vercel/Netlify)
```bash
cd frontend
npm run build
```

### Backend (Heroku/Railway)
```bash
cd backend
npm start
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 👨‍💻 Author

**Your Name**
- GitHub: [@yourusername](https://github.com/yourusername)
- Email: your.email@example.com

## 🙏 Acknowledgments

- [React](https://react.dev)
- [Express.js](https://expressjs.com)
- [MongoDB](https://www.mongodb.com)
- [Tailwind CSS](https://tailwindcss.com)
- [Lucide Icons](https://lucide.dev)

## 📧 Support

For support, email support@cakesman.com or open an issue on GitHub.

---

**Made with ❤️ for cake lovers everywhere** 🧁
