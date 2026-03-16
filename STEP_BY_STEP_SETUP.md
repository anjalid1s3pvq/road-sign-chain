# Step-by-Step Setup Instructions

## Prerequisites
- Node.js 18+ installed
- PostgreSQL database (optional - will use in-memory storage if not available)
- Python 3.8+ (for ML service - optional)

---

## Step 1: Install Dependencies

```bash
npm install
```

This will install all required packages including:
- Backend: express, multer, sharp, exifr, axios, drizzle-orm, pg
- Frontend: React, React Query, etc.

---

## Step 2: Setup Environment Variables

Create a `.env` file in the root directory:

```env
# Database (optional - will use in-memory if not set)
DATABASE_URL=postgresql://user:password@localhost:5432/traffic_audit

# ML Service (optional - will use mock detection if not set)
ML_SERVICE_URL=http://localhost:5001

# Server Port
PORT=5000
```

**Note**: If you don't have PostgreSQL, the app will automatically use in-memory storage for development.

---

## Step 3: Setup Database (Optional but Recommended)

If you have PostgreSQL:

```bash
# Create database
createdb traffic_audit

# Run migrations
npm run db:push
```

This will create all necessary tables:
- `users`
- `audit_records`
- `detected_signs`
- `blockchain_transactions`

---

## Step 4: Setup ML Service (Optional)

### Option A: Use Mock Detection (Default)
The app includes a mock ML detection service that works out of the box. No setup needed.

### Option B: Use Real ML Service
1. Create a Python Flask service (see `ml-service-example/` directory)
2. Install Python dependencies:
   ```bash
   cd ml-service
   pip install flask ultralytics opencv-python pillow numpy
   ```
3. Run the ML service:
   ```bash
   python app.py
   ```
4. Set `ML_SERVICE_URL=http://localhost:5001` in `.env`

---

## Step 5: Start the Application

### Development Mode
```bash
# Terminal 1: Start backend
npm run dev

# Terminal 2: Start frontend (if needed)
npm run dev:client
```

The app will be available at `http://localhost:5000`

---

## Step 6: Test the Application

1. **Upload an Image**:
   - Go to "Upload & Analyze" page
   - Upload a traffic sign image
   - Click "Initiate Analysis"
   - Wait for detection results

2. **View Audit Records**:
   - Go to "Audit Logs" page
   - See all analyzed images
   - Click on any audit to see details

3. **View Blockchain**:
   - Go to "Network" page
   - See blockchain blocks and transactions

---

## Step 7: Verify Functionality

### ✅ Checklist:
- [ ] Image upload works
- [ ] ML detection returns results (mock or real)
- [ ] Metadata extraction works (GPS, timestamp)
- [ ] Audit records are saved
- [ ] Blockchain transactions are created
- [ ] Frontend displays real data
- [ ] Audit detail page shows complete information

---

## Troubleshooting

### Issue: "DATABASE_URL not set"
**Solution**: Either set DATABASE_URL in `.env` or the app will use in-memory storage (data will be lost on restart).

### Issue: "ML Service connection failed"
**Solution**: The app will automatically fall back to mock detection. Check ML_SERVICE_URL if using external service.

### Issue: "Image upload fails"
**Solution**: 
- Check file size (max 10MB)
- Ensure file is an image (jpg, png, etc.)
- Check `uploads/` directory exists and is writable

### Issue: "Blockchain not working"
**Solution**: The blockchain is simulated and works in-memory. For production, integrate with real blockchain (Ethereum, etc.).

---

## Next Steps for Production

1. **Replace Mock ML with Real Model**:
   - Train or use pre-trained YOLO model for traffic signs
   - Deploy ML service separately
   - Update ML_SERVICE_URL

2. **Integrate Real Blockchain**:
   - Deploy smart contracts to Ethereum/Polygon
   - Use web3.js or ethers.js for transactions
   - Update blockchain service

3. **Add Image Storage**:
   - Use cloud storage (AWS S3, Cloudinary, etc.)
   - Update image URL handling

4. **Add Authentication**:
   - Implement user authentication
   - Add role-based access control

5. **Add Real-time Updates**:
   - Use WebSockets for live feed
   - Add real-time notifications

---

## Architecture Overview

```
Frontend (React)
    ↓
Backend API (Express)
    ↓
├── Image Upload → Metadata Extraction → ML Detection
    ↓
├── Database (PostgreSQL) → Audit Records
    ↓
└── Blockchain Service → Transaction Creation
```

---

## API Endpoints

- `POST /api/analyze` - Upload and analyze image
- `GET /api/audits` - Get all audit records
- `GET /api/audits/:id` - Get specific audit record
- `GET /api/blockchain` - Get blockchain status
- `GET /api/blockchain/blocks` - Get blockchain blocks
- `GET /api/health` - Health check

---

## File Structure

```
├── server/
│   ├── routes.ts          # API endpoints
│   ├── storage.ts         # Database operations
│   ├── utils/
│   │   ├── metadata.ts   # EXIF extraction
│   │   ├── ml-service.ts # ML detection
│   │   └── blockchain.ts # Blockchain service
│   └── index.ts          # Server setup
├── client/
│   └── src/
│       ├── pages/        # Frontend pages
│       └── components/   # React components
├── shared/
│   └── schema.ts        # Database schema
└── uploads/             # Uploaded images
```

---

## Support

For issues or questions, check:
1. Console logs for errors
2. Network tab for API responses
3. Database connection status
4. ML service availability

Happy coding! 🚀



