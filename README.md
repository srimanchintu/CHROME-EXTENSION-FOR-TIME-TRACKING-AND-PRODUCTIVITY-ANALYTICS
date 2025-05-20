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

# i have this task 4 CHROME-EXTENSION-FOR-TIME-TRACKING-AND-PRODUCTIVITY-ANALYTICS by using main. python

#OUTPUT

from fastapi import FastAPI
from pydantic import BaseModel
from datetime import datetime
from typing import List

app = FastAPI()

# In-memory store for simplicity (replace with DB in production)
time_logs = []

class LogEntry(BaseModel):
    url: str
    duration: int  # seconds
    timestamp: datetime

@app.post("/log")
def log_time(entry: LogEntry):
    time_logs.append(entry.dict())
    return {"status": "received", "total_logs": len(time_logs)}

@app.get("/stats")
def get_stats():
    total_time = sum(log['duration'] for log in time_logs)
    domain_time = {}
    for log in time_logs:
        domain = log['url'].split('/')[2] if '//' in log['url'] else log['url']
        domain_time[domain] = domain_time.get(domain, 0) + log['duration']
    return {"total_seconds": total_time, "per_domain": domain_time}
