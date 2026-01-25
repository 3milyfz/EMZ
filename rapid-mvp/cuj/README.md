# A2 Sprint: Classroom Presentation Randomizer

A minimal MVP testbed that helps instructors manage in-person presentation sessions by randomly selecting teams, displaying team metadata, and running a dual-phase timer with visual warnings.

## Code Repository

**Repository:** [https://github.com/3milyfz/classroom-presentation-generator](https://github.com/3milyfz/classroom-presentation-generator)

The codebase is organized into:
- `backend/` - Express API server
- `frontend/` - React + Vite application

## Local Setup

### Prerequisites

- **Node.js**: v18+ (or latest LTS)
- **npm**: v9+ (comes with Node.js)
- Two terminal windows/tabs

### Backend Setup

```bash
cd backend
npm install
npm start
```

The backend API will run on **http://localhost:5001**

### Frontend Setup

In a separate terminal:

```bash
cd frontend
npm install
npm start
```

The frontend application will run on **http://localhost:3000**

## Environment Notes

- **Backend Port**: `5001` (default, hardcoded in `server.js`)
- **Frontend Port**: `3000` (default, configured in `vite.config.js`)
- **API Base URL**: Can be overridden via environment variable:
  ```bash
  export VITE_API_BASE=http://localhost:5001
  ```
- **No `.env` file required** - all configuration uses defaults

## How to Use (Instructor Flow)

1. **Add Teams**: Use the "Team Metadata" form in the Control Panel to add teams (team name required; members and topic are optional).

2. **Randomize**: Click "Select Next Team" to randomly pick a team that hasn't presented yet. The selected team will be highlighted in the Dashboard.

3. **Start Timer**: Use the Dual-Phase Timer controls:
   - Adjust presentation/Q&A durations (default: 7 min / 3 min)
   - Click "Start" to begin the presentation countdown
   - Timer turns red/bold when 2 minutes remain
   - Automatically switches to Q&A phase after presentation ends

4. **Reset**: Use "Reset Round" to allow all teams to be selected again, or "Reset Teams" to clear the entire roster.

## Architecture

- **Decoupled Design**: Frontend and backend run independently on separate ports, ready for cloud deployment.
- **State Management**: Backend maintains team roster and randomizer state in memory.
- **No Database**: All data is ephemeral and resets on server restart.