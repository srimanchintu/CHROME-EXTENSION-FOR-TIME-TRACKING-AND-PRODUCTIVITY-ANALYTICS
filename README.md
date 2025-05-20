from fastapi import FastAPI
from pydantic import BaseModel
from datetime import datetime

app = FastAPI()

# In-memory for demo
data = []

class LogEntry(BaseModel):
    url: str
    duration: int  # in seconds
    timestamp: datetime

@app.post("/log")
def receive_log(entry: LogEntry):
    data.append(entry.dict())
    return {"status": "success", "count": len(data)}

@app.get("/stats")
def get_stats():
    total_time = sum(d['duration'] for d in data)
    return {"total_seconds": total_time}

