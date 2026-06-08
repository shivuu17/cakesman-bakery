# API Testing Guide

## 1. Database Connection
✅ Backend connects to MongoDB
✅ Collections created automatically
✅ Sample data seeded on startup

Test: `npm start` in backend folder, check console

## 2. Image Upload
- POST `/api/products/upload-images` (multipart/form-data)
  - Max 5 images
  - Max 5MB each
  - Allowed: JPG, PNG, WebP

## 3. Product Creation
- POST `/api/products/create`
  - Requires: name, description, category, basePrice, images
  - Returns: product with image URLs

## 4. Product Listing
- GET `/api/products`
  - Returns: all products with images

## 5. Order Creation (Security)
- POST `/api/orders`
  - Server recalculates total price
  - Verifies product prices match
  - Validates phone number
  - Prevents price manipulation

## 6. Security Features
✅ Helmet.js - Security headers
✅ Input sanitization - No NoSQL injection
✅ Rate limiting - Prevents brute force
✅ CORS - Restricted domains
✅ Image validation - Only images allowed
✅ Price verification - Server-side calculation
