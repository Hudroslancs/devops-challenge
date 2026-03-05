Provide your solution here:

## Problem 3: Diagnose Me Doctor

## Step 1 - Figure out whats eating the disk. Check disk usage first. df -h

Find which folder is taking the most space:
du -sh /* 2>/dev/null | sort -rh | head -10

Find biggest folder:
du -sh /var/log/nginx/* | sort -rh | head -10

## Possible causes

1. Nginnx  access logs
  logs fills up the disks, NGINX cannot write logs, crash.
2. No config of log rotation.
  old log files never gets deleted, disk eventuall full
3. Core dump files
  Large debug file to disk after crashing.
  Find and delete. Investigate why its crashed. fix the roout cause 

## Prevention

### Log Rotation
Configure /etc/logrotate.d/nginx to:
- Rotate logs daily
- Keep only 7 days of logs
- Compress old logs automatically

### Monitoring
- Set up disk usage alerts at 80% so you catch it before it hits 99%
- Monitor log file sizes daily

### Long Term
- Ship NGINX logs to a centralized logging service like AWS CloudWatch
- This way logs are stored on AWS instead of on the VM disk
- VM disk never fills up from logs again
  
