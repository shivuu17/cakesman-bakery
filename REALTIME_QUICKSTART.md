# Real-Time Sync Quick Start Guide

## What's Been Implemented

✅ **Backend Socket.io Server** - Configured in `server.js`  
✅ **Socket Service Layer** - Helper functions in `services/socketService.js`  
✅ **Product Controller Updates** - Emits `product_created`, `product_updated`, `product_deleted`  
✅ **Order Controller Updates** - Emits `order_created`, `order_status_updated`  
✅ **React Socket Hook** - `useSocket.js` for connection management  
✅ **Socket Context** - `SocketContext.jsx` for global state management  
✅ **App Integration** - Wrapped with `SocketProvider` in `App.jsx`  

## Installation & Setup

### Step 1: Install Dependencies
```bash
# Backend
cd backend
npm install socket.io

# Frontend  
cd frontend
npm install socket.io-client
```

### Step 2: Ensure Backend Port is 5001
Check your `.env` file:
```
MONGO_URI=mongodb://localhost:27017/cakesman
PORT=5001
```

### Step 3: Start Servers
```bash
# Terminal 1 - Backend
cd backend
npm start

# Terminal 2 - Frontend
cd frontend
npm start
```

### Step 4: Verify Connection
Open Admin Dashboard at `http://localhost:3000/admin`  
You should see **🟢 Connected** status badge in top-right corner.

## Socket Events Flow

### When Admin Creates a Product:
```
Admin submits form
  ↓
API POST /api/products
  ↓
Controller saves to MongoDB
  ↓
Emits socket event: product_created
  ↓
SocketContext receives event
  ↓
Updates products state
  ↓
All customers see new product instantly ⚡
```

### When Admin Updates Order Status:
```
Admin selects new status
  ↓
API PATCH /api/orders/:id/status
  ↓
Controller updates in MongoDB
  ↓
Emits socket event: order_status_updated
  ↓
SocketContext receives event
  ↓
Updates orders state
  ↓
Customer sees status change instantly ⚡
Admin sees order updated in real-time ⚡
```

### When Customer Places Order:
```
Customer completes checkout
  ↓
API POST /api/orders
  ↓
Controller saves to MongoDB
  ↓
Emits socket event: order_created (to admins)
  ↓
Admin sees new order notification instantly ⚡
Admin SocketContext updates orders list
  ↓
Emits socket event: customer_notification
  ↓
Customer sees "Order received" message ⚡
```

## Key Files Reference

| File | Purpose | Key Code |
|------|---------|----------|
| `backend/server.js` | Socket.io setup | `io.on("connection", ...)` |
| `backend/services/socketService.js` | Event broadcasting | `broadcastProductUpdate()` |
| `backend/controllers/productController.js` | Product operations | `broadcastProductUpdate(io, "product_created", ...)` |
| `backend/controllers/orderController.js` | Order operations | `broadcastOrderCreated(io, order)` |
| `frontend/src/hooks/useSocket.js` | Connection management | `useSocket(userId, userRole)` |
| `frontend/src/context/SocketContext.jsx` | Global socket state | Event listeners & state updates |
| `frontend/src/App.jsx` | App setup | `<SocketProvider>` wrapper |

## Using Socket Events in Components

### Example: Product List Component
```javascript
import { useContext } from "react";
import { SocketContext } from "../context/SocketContext";

const ProductGallery = () => {
  const { products, connected } = useContext(SocketContext);
  
  return (
    <div>
      {!connected && <div className="warning">⚠️ Connection lost</div>}
      {products.map((p) => (
        <ProductCard key={p._id} product={p} />
      ))}
    </div>
  );
};
```

### Example: Order Status Component
```javascript
const OrderTracker = ({ orderId }) => {
  const { orders } = useContext(SocketContext);
  
  const order = orders.find((o) => o._id === orderId);
  
  return (
    <div>
      <h2>Order {orderId}</h2>
      <p>Status: <strong>{order?.status}</strong></p>
      {/* Status updates automatically when admin changes it */}
    </div>
  );
};
```

## Testing Real-Time Functionality

### Test 1: Product Real-Time Sync
1. Open Admin Dashboard: http://localhost:3000/admin
2. Open Customer Page: http://localhost:3000 (in another tab)
3. In Admin Dashboard, click "+ Add Product"
4. Fill in product details and submit
5. **Expected:** Product appears in customer page instantly (no refresh needed)

### Test 2: Order Real-Time Notifications
1. Stay on customer page, add items to cart
2. Proceed to checkout and place order
3. Switch to Admin Dashboard
4. **Expected:** New order appears instantly with notification "New order received"

### Test 3: Order Status Real-Time Update
1. Keep customer page open with order history
2. In Admin Dashboard, go to Orders tab
3. Change order status to "Processing"
4. **Expected:** Customer page updates instantly showing new status

### Browser Console Verification
Open browser DevTools Console (F12) and look for:
```
[Socket] Connected to server
User [id] (admin) joined room
```

## Troubleshooting

### Issue: "Connected" shows as disconnected
**Solution:**
- Verify backend is running on port 5001: `netstat -ano | findstr :5001`
- Check CORS origin in `server.js` matches your frontend URL
- Restart both servers

### Issue: Events not firing
**Solution:**
- Check browser console for socket connection messages
- Verify `SocketContext` is wrapping your app in `App.jsx`
- Ensure controllers are calling socket service functions

### Issue: Products not syncing
**Solution:**
- Verify `socket.io` is installed in backend: `npm ls socket.io`
- Check `productController.js` is importing `broadcastProductUpdate`
- Look for errors in backend terminal logs

### Issue: Orders not visible in admin
**Solution:**
- Verify user is logged in with admin role
- Check SocketContext is properly initialized
- Ensure order controller is populating user details

## Production Checklist

Before deploying to production:

- [ ] Update CORS origin to production domain
- [ ] Set NODE_ENV=production
- [ ] Configure environment variables properly
- [ ] Implement SSL/TLS (wss:// instead of ws://)
- [ ] Set up Redis adapter for Socket.io (for multiple servers)
- [ ] Add error logging and monitoring
- [ ] Test with production database
- [ ] Set up rate limiting for socket events
- [ ] Implement socket authentication properly
- [ ] Configure firewall rules for socket port

## Performance Tips

1. **Limit Socket Rooms:** Use namespace separation for admin/customer
2. **Event Throttling:** Avoid rapid successive updates
3. **Memory Management:** Clean up event listeners properly
4. **Compression:** Enable gzip compression for socket payloads
5. **Redis Adapter:** Scale across multiple server instances

## Next Steps

1. ✅ Implement basic real-time sync (DONE)
2. ⏳ Add email notifications for order updates
3. ⏳ Implement socket.io-redis for scaling
4. ⏳ Add comprehensive error recovery
5. ⏳ Create admin dashboard analytics with real-time updates
6. ⏳ Add inventory management with stock sync
7. ⏳ Implement user activity logging via sockets

## Support & Debug

To enable Socket.io debug logs:
```javascript
// In browser console
localStorage.debug = '*';
// Then reload the page
```

To monitor socket events in backend:
```bash
# In backend terminal
DEBUG=socket.io npm start
```

## Architecture Overview

```
User connects to frontend
        ↓
Frontend emits "user_role" event
        ↓
Backend adds user to room (admin/customer/user_id)
        ↓
When data changes:
        ├─ Admin changes product → Server broadcasts to all clients
        ├─ Customer places order → Server notifies admin room
        ├─ Admin updates status → Server notifies customer room
        └─ All listeners update state → UI re-renders
```

---

**Happy Real-Time Syncing! 🎉**
