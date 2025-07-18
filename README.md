# Job Import System

A full-stack, scalable job import system that fetches job listings from multiple external XML APIs, processes them through a Redis-backed queue system, and provides a comprehensive admin interface for monitoring import activities.

## 🚀 Features

- **Multi-Source Job Import**: Fetches jobs from multiple XML/RSS feeds
- **Queue-Based Processing**: Uses Redis and Bull for reliable background processing
- **Real-time Monitoring**: Live dashboard showing import statistics and queue status
- **Automated Scheduling**: Configurable cron jobs for regular imports
- **Comprehensive Logging**: Detailed import history with success/failure tracking
- **Scalable Architecture**: Modular design with separate worker processes
- **Modern UI**: Beautiful React-based admin interface with real-time updates

## 🏗️ Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Frontend      │    │   Backend API   │    │   Worker        │
│   (React)       │◄──►│   (Express)     │◄──►│   (Bull Queue)  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                │                        │
                                ▼                        ▼
                       ┌─────────────────┐    ┌─────────────────┐
                       │   MongoDB       │    │   Redis         │
                       │   (Data Store)  │    │   (Queue Store) │
                       └─────────────────┘    └─────────────────┘
```

## 📋 Prerequisites

- Node.js (v16 or higher)
- MongoDB (local or MongoDB Atlas)
- Redis (local or Redis Cloud)
- npm or yarn

## 🛠️ Installation & Setup

### 1. Clone and Install Dependencies

```bash
# Install frontend dependencies
npm install

# Install backend dependencies
cd server
npm install
```

### 2. Environment Configuration

Create a `.env` file in the `server` directory:

```bash
cp server/.env.example server/.env
```

Update the `.env` file with your configuration:

```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/job-import-system
REDIS_URL=redis://localhost:6379
NODE_ENV=development

# Job Import Settings
IMPORT_INTERVAL_HOURS=1
BATCH_SIZE=50
WORKER_CONCURRENCY=5

# API Sources (comma-separated)
JOB_SOURCES=https://jobicy.com/?feed=job_feed,https://jobicy.com/?feed=job_feed&job_categories=smm&job_types=full-time,https://jobicy.com/?feed=job_feed&job_categories=design-multimedia,https://www.higheredjobs.com/rss/articleFeed.cfm
```

### 3. Database Setup

Ensure MongoDB is running locally or update the `MONGODB_URI` to point to your MongoDB Atlas cluster.

### 4. Redis Setup

Ensure Redis is running locally or update the `REDIS_URL` to point to your Redis instance.

## 🚀 Running the Application

### Development Mode

```bash
# Terminal 1: Start the backend API server
cd server
npm run dev

# Terminal 2: Start the worker process
cd server
npm run worker

# Terminal 3: Start the frontend development server
npm run dev
```

### Production Mode

```bash
# Build and start backend
cd server
npm start

# In another terminal, start worker
cd server
npm run worker

# Build and serve frontend
npm run build
npm run preview
```

## 📊 API Endpoints

### Import Management
- `GET /api/imports/logs` - Get import history with pagination
- `GET /api/imports/logs/:id` - Get specific import log details
- `GET /api/imports/stats` - Get import statistics and trends
- `POST /api/imports/trigger` - Manually trigger import jobs
- `GET /api/imports/queue/stats` - Get queue statistics

### Job Management
- `GET /api/jobs` - Get jobs with filtering and pagination
- `GET /api/jobs/:id` - Get specific job details
- `GET /api/jobs/stats/overview` - Get job statistics

### System Health
- `GET /api/health` - Health check endpoint

## 🔧 Configuration Options

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `PORT` | API server port | `5000` |
| `MONGODB_URI` | MongoDB connection string | `mongodb://localhost:27017/job-import-system` |
| `REDIS_URL` | Redis connection string | `redis://localhost:6379` |
| `IMPORT_INTERVAL_HOURS` | Cron job interval in hours | `1` |
| `BATCH_SIZE` | Number of jobs processed per batch | `50` |
| `WORKER_CONCURRENCY` | Number of concurrent workers | `5` |
| `JOB_SOURCES` | Comma-separated list of job feed URLs | See example |

### Supported Job Sources

The system supports various XML/RSS feed formats:
- RSS 2.0 feeds
- Atom feeds
- Custom job XML formats

Current configured sources:
- Jobicy.com general feed
- Jobicy.com SMM full-time jobs
- Jobicy.com design & multimedia jobs
- HigherEdJobs.com RSS feed

## 📈 Monitoring & Analytics

The admin dashboard provides:

- **Real-time Statistics**: Total imports, success rates, recent activity
- **Queue Monitoring**: Active, waiting, completed, and failed jobs
- **Import History**: Detailed logs with timestamps and results
- **Source Performance**: Per-source success rates and processing times
- **Trend Analysis**: Historical data visualization

## 🔄 Queue Management

The system uses Bull queues with Redis for:
- **Job Scheduling**: Automatic and manual job triggers
- **Retry Logic**: Exponential backoff for failed jobs
- **Concurrency Control**: Configurable worker processes
- **Progress Tracking**: Real-time job progress updates

## 📝 Data Models

### Job Schema
```javascript
{
  title: String,
  description: String,
  company: String,
  location: String,
  jobType: String,
  category: String,
  salary: { min: Number, max: Number, currency: String },
  requirements: [String],
  benefits: [String],
  applicationUrl: String,
  sourceUrl: String,
  externalId: String,
  publishedDate: Date,
  // ... additional fields
}
```

### Import Log Schema
```javascript
{
  timestamp: Date,
  fileName: String,
  sourceUrl: String,
  status: String,
  totalFetched: Number,
  newJobs: Number,
  updatedJobs: Number,
  failedJobs: Number,
  failedJobsReasons: [Object],
  processingTime: Number,
  metadata: Object
}
```

## 🚀 Deployment

### Using Docker (Recommended)

```bash
# Build and run with Docker Compose
docker-compose up -d
```

### Manual Deployment

1. **Backend**: Deploy to services like Heroku, Railway, or DigitalOcean
2. **Frontend**: Deploy to Vercel, Netlify, or similar
3. **Database**: Use MongoDB Atlas for production
4. **Redis**: Use Redis Cloud or similar managed service

### Environment Setup for Production

- Set `NODE_ENV=production`
- Use secure MongoDB and Redis connections
- Configure proper CORS settings
- Set up monitoring and logging
- Use process managers like PM2

## 🧪 Testing

```bash
# Run backend tests
cd server
npm test

# Run frontend tests
npm test
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🆘 Troubleshooting

### Common Issues

1. **MongoDB Connection Failed**
   - Ensure MongoDB is running
   - Check connection string format
   - Verify network connectivity

2. **Redis Connection Failed**
   - Ensure Redis server is running
   - Check Redis URL format
   - Verify Redis authentication

3. **Jobs Not Processing**
   - Check worker process is running
   - Verify queue configuration
   - Check Redis connectivity

4. **Import Failures**
   - Verify source URLs are accessible
   - Check XML format compatibility
   - Review error logs for details

### Logs and Debugging

- Backend logs: Check console output or log files
- Queue monitoring: Use Redis CLI or admin tools
- Database queries: Use MongoDB Compass or CLI
- Frontend errors: Check browser developer tools

## 📞 Support

For support and questions:
- Create an issue on GitHub
- Check the documentation
- Review the troubleshooting guide

---

Built with ❤️ using Node.js, React, MongoDB, and Redis