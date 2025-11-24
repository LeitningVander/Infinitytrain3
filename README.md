# InfinityTrain - Learning Tracker

A comprehensive employee training and learning management system with progress tracking, collaborative comments, and rich resource management.

## Features

- **Topic Management**: Organize training content into topics with customizable icons
- **Subtopics & Resources**: Create detailed subtopics with markdown-based resources
- **Progress Tracking**: Track employee understanding levels (Not Addressed, Basic, Good, Fully Understood)
- **Collaborative Learning**: Add comments, images, and drawings to subtopics
- **User Roles**: Admin and employee roles with appropriate permissions
- **Persistent Storage**: SQLite database for reliable data persistence

## Technology Stack

- **Frontend**: React 19, TypeScript, Vite, TailwindCSS, Radix UI
- **Backend**: Express.js, Node.js
- **Database**: SQLite (better-sqlite3) - lightweight and perfect for cloud deployments
- **Deployment**: Optimized for Render.com

## Local Development

### Prerequisites

- Node.js 20.x or higher
- npm

### Setup

1. Clone the repository:
   ```bash
   git clone <your-repo-url>
   cd Infinitytrain3
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Start the development server:
   ```bash
   npm run dev
   ```
   
   This will start both the backend API and the Vite dev server. The application will be available at `http://localhost:5000`.

### Development Scripts

- `npm run dev` - Start development server with hot reload
- `npm run dev:client` - Start only the Vite client dev server
- `npm run build` - Build for production
- `npm start` - Start production server
- `npm run check` - Run TypeScript type checking

## Deploying to Render

### Quick Deploy

This application is configured for easy deployment to Render using the included `render.yaml` Blueprint.

1. **Push your code to GitHub** (if not already done)

2. **Create a new Web Service on Render**:
   - Go to [Render Dashboard](https://dashboard.render.com/)
   - Click "New +" and select "Web Service"
   - Connect your GitHub repository
   - Render will automatically detect the `render.yaml` file

3. **Configure the service** (if not using Blueprint):
   - **Name**: infinitytrain (or your preferred name)
   - **Environment**: Node
   - **Build Command**: `npm install && npm run build`
   - **Start Command**: `npm start`
   - **Plan**: Free (or your preferred plan)

4. **Add Persistent Disk** (Critical for database persistence):
   - In the service settings, go to "Disks"
   - Add a new disk:
     - **Name**: infinitytrain-data
     - **Mount Path**: `/opt/render/project/data`
     - **Size**: 1GB (or as needed)

5. **Deploy**: Click "Create Web Service" or "Deploy"

### Environment Variables

The application automatically detects when running on Render via the `RENDER` environment variable. No additional configuration is needed.

### Database Storage

- **Local Development**: Database stored as `training.db` in the project root
- **Render Production**: Database stored in persistent disk at `/opt/render/project/data/training.db`

The SQLite database ensures:
- ✅ Lightweight and efficient
- ✅ No external database service required
- ✅ Persistent storage across deployments (when using Render Disks)
- ✅ Zero configuration needed
- ✅ Perfect for small to medium-sized teams

### Render Blueprint Configuration

The included `render.yaml` file configures:
- Node.js 20.19.0 runtime
- Automatic build and start commands
- Persistent disk for database storage
- Free tier optimization

## Default Users

The application comes with pre-configured demo users:

- **Admin**: admin@oceaninfinity.com
- **Employee 1**: sarah@oceaninfinity.com
- **Employee 2**: john@oceaninfinity.com

## Project Structure

```
├── client/                 # Frontend React application
│   ├── src/
│   │   ├── components/    # Reusable UI components
│   │   ├── pages/         # Page components
│   │   ├── hooks/         # Custom React hooks
│   │   └── lib/           # Utilities and store
│   └── public/            # Static assets
├── server/                 # Backend Express application
│   ├── app.ts             # Express app setup
│   ├── routes.ts          # API routes
│   ├── sqlite-storage.ts  # Database layer
│   └── index-prod.ts      # Production entry point
├── render.yaml            # Render deployment config
└── package.json           # Dependencies and scripts
```

## API Endpoints

### Topics
- `GET /api/topics` - Get all topics
- `POST /api/topics` - Create a new topic
- `PUT /api/topics/:id` - Update a topic
- `DELETE /api/topics/:id` - Soft delete a topic
- `POST /api/topics/:id/restore` - Restore a deleted topic

### Progress
- `GET /api/progress/:userId` - Get user progress
- `POST /api/progress` - Save progress update

### Users
- `GET /api/users/:id` - Get user by ID

### Comments
- `POST /api/comments` - Add a comment to a subtopic

## Production Considerations

### Scaling

- The SQLite database is suitable for small to medium deployments (up to thousands of users)
- For larger deployments, consider migrating to PostgreSQL
- Render's persistent disk ensures data survives across deployments

### Backups

- Render Disks are backed up automatically
- For additional safety, implement periodic database exports
- The SQLite file can be easily downloaded and backed up externally

### Performance

- Static assets are served efficiently via Express
- Vite optimizes the frontend bundle
- SQLite provides fast read/write operations

## Support & Issues

For issues, feature requests, or contributions, please open an issue in the GitHub repository.

## License

MIT
