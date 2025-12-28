# Local Setup Guide

## Prerequisites

1. **Node.js** (v14 or higher recommended)
   - Download from [nodejs.org](https://nodejs.org/)
   - Verify installation: `node --version`

2. **MongoDB Atlas Account** (or local MongoDB)
   - The project uses MongoDB Atlas (cloud database)
   - You'll need your MongoDB connection credentials

3. **Firebase Project**
   - You'll need a Firebase service account key (JSON file)
   - The key should be base64 encoded in the `.env` file

4. **Stripe Account** (for payment functionality)
   - You'll need a Stripe secret key

## Installation Steps

### 1. Install Dependencies

```bash
npm install
```

### 2. Set Up Environment Variables

Create a `.env` file in the root directory with the following variables:

```env
# Server Port (optional, defaults to 3000)
PORT=3000

# MongoDB Connection
DB_USER=your_mongodb_username
DB_PASS=your_mongodb_password

# Firebase Admin SDK
# This should be a base64 encoded JSON service account key
FB_SERVICE_KEY=your_base64_encoded_firebase_service_account_key

# Stripe Payment
STRIPE_SECRET=your_stripe_secret_key

# Site Domain (for Stripe redirect URLs)
SITE_DOMAIN=http://localhost:3000
```

### 3. Get Firebase Service Account Key

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Select your project
3. Go to Project Settings > Service Accounts
4. Click "Generate New Private Key"
5. Download the JSON file
6. Convert it to base64:
   ```bash
   # On Windows (PowerShell):
   [Convert]::ToBase64String([IO.File]::ReadAllBytes("path/to/serviceAccountKey.json"))
   
   # On Mac/Linux:
   base64 -i path/to/serviceAccountKey.json
   ```
7. Copy the base64 string to `FB_SERVICE_KEY` in your `.env` file

### 4. Run the Server

```bash
npm start
```

Or:

```bash
node index.js
```

The server will start on `http://localhost:3000` (or the port specified in your `.env` file).

## Testing the Server

Once running, you can test it by visiting:
- `http://localhost:3000/` - Should return "zap is shifting shifting!"

## API Endpoints

The server provides various endpoints for:
- User management (`/users/*`)
- Parcel management (`/parcels/*`)
- Rider management (`/riders/*`)
- Payment processing (`/payments/*`)
- Tracking (`/trackings/*`)

All endpoints (except the root) require Firebase authentication via the `Authorization` header.

## Troubleshooting

1. **MongoDB Connection Issues**
   - Verify your MongoDB Atlas credentials
   - Check if your IP is whitelisted in MongoDB Atlas
   - Ensure the connection string format is correct

2. **Firebase Authentication Errors**
   - Verify your `FB_SERVICE_KEY` is correctly base64 encoded
   - Ensure the service account has proper permissions

3. **Port Already in Use**
   - Change the `PORT` in your `.env` file
   - Or kill the process using that port

4. **Module Not Found Errors**
   - Run `npm install` again
   - Delete `node_modules` and `package-lock.json`, then run `npm install`

## Notes

- The `.env` file is gitignored and should not be committed
- Make sure you have the Firebase Admin SDK JSON file (`zap-shift-firebase-adminsdk.json`) if you're using it directly (though the code uses base64 encoded version)
- For production, use environment variables provided by your hosting platform (Vercel, etc.)

