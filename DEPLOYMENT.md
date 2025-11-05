# 🚀 Deployment Guide - Urban Company Clone

This guide provides step-by-step instructions to deploy the Urban Company Clone application using free hosting services.

## 📋 Prerequisites

- GitHub account
- Node.js (v14 or higher) installed locally
- MongoDB Atlas account (free tier)
- Vercel account (for frontend)
- Railway/Render account (for backend)

## 🏗️ Project Architecture

```
Urban_Company_Clone/
├── frontend/          # React.js application
├── backend/           # Node.js/Express API
└── DEPLOYMENT.md      # This file
```

## 📦 Pre-Deployment Setup

### 1. Environment Variables Setup

#### Backend Environment Variables
Create a `.env` file in the `backend/` directory:

```env
# Database Configuration
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/urban_company_clone?retryWrites=true&w=majority

# JWT Configuration
JWT_SECRET=your_super_secret_jwt_key_here_make_it_long_and_complex

# Server Configuration
PORT=5000
NODE_ENV=production

# CORS Configuration
FRONTEND_URL=https://your-frontend-domain.vercel.app
```

#### Frontend Environment Variables
Create a `.env` file in the `frontend/` directory:

```env
# API Configuration
REACT_APP_API_URL=https://your-backend-domain.railway.app/api

# Other Configuration (if needed)
REACT_APP_ENVIRONMENT=production
```

### 2. Update Package.json Files

#### Backend package.json
Ensure your `backend/package.json` has the correct start script:

```json
{
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "engines": {
    "node": ">=14.0.0"
  }
}
```

#### Frontend package.json
Ensure your `frontend/package.json` has the correct build script:

```json
{
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "engines": {
    "node": ">=14.0.0"
  }
}
```

## 🗄️ Database Setup (MongoDB Atlas - Free)

### Step 1: Create MongoDB Atlas Account
1. Go to [MongoDB Atlas](https://cloud.mongodb.com/)
2. Sign up for a free account
3. Create a new project

### Step 2: Create a Cluster
1. Click "Build a Database"
2. Choose "FREE" tier (M0 Sandbox)
3. Select a cloud provider and region
4. Name your cluster (e.g., "urban-company-cluster")
5. Click "Create Cluster"

### Step 3: Configure Database Access
1. Go to "Database Access" in the left sidebar
2. Click "Add New Database User"
3. Choose "Password" authentication
4. Create a username and strong password
5. Set user privileges to "Read and write to any database"
6. Click "Add User"

### Step 4: Configure Network Access
1. Go to "Network Access" in the left sidebar
2. Click "Add IP Address"
3. Choose "Allow Access from Anywhere" (0.0.0.0/0)
4. Click "Confirm"

### Step 5: Get Connection String
1. Go to "Clusters" and click "Connect"
2. Choose "Connect your application"
3. Copy the connection string
4. Replace `<password>` with your database user password
5. Replace `<dbname>` with "urban_company_clone"

## 🌐 Backend Deployment (Railway - Free)

### Step 1: Prepare Backend for Deployment
1. Ensure your `server.js` has the correct CORS configuration:

```javascript
app.use(cors({
  origin: process.env.FRONTEND_URL || '*',
  credentials: true
}));
```

2. Make sure the server listens on the correct port:

```javascript
const PORT = process.env.PORT || 5000;
app.listen(PORT, '0.0.0.0', () => {
  console.log(`Server running on port ${PORT}`);
});
```

### Step 2: Deploy to Railway
1. Go to [Railway](https://railway.app/)
2. Sign up with your GitHub account
3. Click "New Project"
4. Choose "Deploy from GitHub repo"
5. Select your Urban Company Clone repository
6. Choose the root directory (Railway will auto-detect the backend)

### Step 3: Configure Environment Variables
1. In your Railway project dashboard, click on your service
2. Go to the "Variables" tab
3. Add all the environment variables from your `.env` file:
   - `MONGODB_URI`
   - `JWT_SECRET`
   - `NODE_ENV=production`
   - `FRONTEND_URL` (you'll update this after frontend deployment)

### Step 4: Configure Build Settings
1. Go to "Settings" tab
2. Set "Root Directory" to `backend`
3. Set "Build Command" to `npm install`
4. Set "Start Command" to `npm start`

### Step 5: Deploy
1. Railway will automatically deploy your backend
2. Wait for deployment to complete
3. Copy your backend URL (e.g., `https://your-app-name.railway.app`)

## 🎨 Frontend Deployment (Vercel - Free)

### Step 1: Prepare Frontend for Deployment
1. Update your `frontend/.env` file with the Railway backend URL:

```env
REACT_APP_API_URL=https://your-backend-domain.railway.app/api
```

2. Test the build locally:

```bash
cd frontend
npm run build
```

### Step 2: Deploy to Vercel
1. Go to [Vercel](https://vercel.com/)
2. Sign up with your GitHub account
3. Click "New Project"
4. Import your GitHub repository
5. Configure project settings:
   - **Framework Preset**: Create React App
   - **Root Directory**: `frontend`
   - **Build Command**: `npm run build`
   - **Output Directory**: `build`

### Step 3: Configure Environment Variables
1. In the Vercel project settings, go to "Environment Variables"
2. Add your environment variable:
   - **Name**: `REACT_APP_API_URL`
   - **Value**: `https://your-backend-domain.railway.app/api`
   - **Environment**: Production

### Step 4: Deploy
1. Click "Deploy"
2. Wait for deployment to complete
3. Copy your frontend URL (e.g., `https://your-app-name.vercel.app`)

## 🔄 Final Configuration

### Step 1: Update Backend CORS
1. Go back to your Railway project
2. Update the `FRONTEND_URL` environment variable with your Vercel URL
3. Redeploy the backend (Railway will auto-redeploy on environment changes)

### Step 2: Test Your Application
1. Visit your Vercel frontend URL
2. Test user registration and login
3. Test service browsing and booking
4. Verify all API calls are working correctly

## 🛠️ Alternative Free Hosting Options

### Backend Alternatives
1. **Render** (Alternative to Railway)
   - Similar setup process
   - Free tier available
   - Good for Node.js applications

2. **Heroku** (Limited free tier)
   - Classic choice for backend hosting
   - Easy GitHub integration

### Frontend Alternatives
1. **Netlify** (Alternative to Vercel)
   - Drag-and-drop deployment
   - Great for React applications
   - Free tier available

2. **GitHub Pages** (For static sites)
   - Free hosting for public repositories
   - Good for frontend-only deployments

## 🔧 Troubleshooting

### Common Issues and Solutions

#### CORS Errors
```javascript
// In your backend server.js, ensure proper CORS configuration:
app.use(cors({
  origin: [
    'http://localhost:3000',
    'https://your-frontend-domain.vercel.app'
  ],
  credentials: true
}));
```

#### MongoDB Connection Issues
- Verify your connection string is correct
- Ensure IP address 0.0.0.0/0 is whitelisted
- Check database user permissions

#### Environment Variables Not Working
- Ensure environment variables are set correctly in hosting platforms
- Frontend environment variables must start with `REACT_APP_`
- Restart/redeploy after changing environment variables

#### Build Failures
```bash
# Clear npm cache and reinstall dependencies
npm cache clean --force
rm -rf node_modules package-lock.json
npm install
```

## 📊 Monitoring and Maintenance

### Performance Monitoring
1. Use Railway/Vercel built-in monitoring
2. Set up MongoDB Atlas monitoring
3. Monitor API response times

### Security Best Practices
1. Regularly update dependencies
2. Use strong JWT secrets
3. Implement rate limiting
4. Monitor for security vulnerabilities

### Backup Strategy
1. MongoDB Atlas automatic backups (free tier)
2. Regular code commits to GitHub
3. Environment variables backup

## 💰 Cost Considerations

### Free Tier Limitations

#### MongoDB Atlas (Free)
- 512 MB storage
- Shared RAM and vCPU
- No backup/restore
- Perfect for development and small projects

#### Railway (Free)
- $5 credit per month
- Usage-based billing
- Good for small to medium applications

#### Vercel (Free)
- 100 GB bandwidth per month
- 1000 serverless function executions per day
- Excellent for frontend hosting

### Scaling Options
When you outgrow free tiers:
1. **Database**: Upgrade MongoDB Atlas plan
2. **Backend**: Add Railway credits or switch to paid plan
3. **Frontend**: Upgrade Vercel plan for more bandwidth

## 🚀 Quick Deploy Checklist

- [ ] MongoDB Atlas cluster created and configured
- [ ] Environment variables configured for both frontend and backend
- [ ] Backend deployed to Railway
- [ ] Frontend deployed to Vercel
- [ ] CORS configuration updated with production URLs
- [ ] Application tested end-to-end
- [ ] Environment variables secured and backed up

## 📞 Support

If you encounter issues:
1. Check the troubleshooting section above
2. Review hosting platform documentation
3. Check application logs in Railway/Vercel dashboards
4. Verify environment variables are correctly set

---

**🎉 Congratulations!** Your Urban Company Clone is now deployed and accessible worldwide for free!

**Frontend URL**: `https://your-app-name.vercel.app`
**Backend URL**: `https://your-app-name.railway.app`
**Database**: MongoDB Atlas Cluster

Remember to keep your environment variables secure and regularly update your application dependencies.
