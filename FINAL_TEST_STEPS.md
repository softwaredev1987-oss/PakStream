# Final Testing Steps - Complete Checklist

## 🎯 Goal
Test your complete CDN implementation with all services running.

---

## ✅ Step 1: Start Backend (Terminal 1)

```bash
cd ~/projects/PakStream/backend
npm start
```

**Expected Output:**
```
Server is running on port 5000
Socket.IO server is running on port 5000
Connected to MongoDB
```

**Keep this terminal open!** ✅

---

## ✅ Step 2: Check Edge Server (Nginx)

```bash
# Check if Nginx is running
sudo systemctl status nginx

# If not running, start it:
sudo systemctl start nginx

# Test edge server
curl http://localhost:8080/health
```

**Expected:** "Edge server is healthy" ✅

---

## ✅ Step 3: Start Frontend (Terminal 2)

```bash
cd ~/projects/PakStream/frontend
npm start
```

**Expected Output:**
```
Compiled successfully!
Local: http://localhost:3000
```

**Keep this terminal open!** ✅

---

## ✅ Step 4: Open Chrome and Test

### 4.1 Open Chrome
```
http://localhost:3000
```

### 4.2 Open DevTools
- Press **F12**
- Go to **Network** tab
- **Uncheck** "Disable cache"

### 4.3 Test Your App
- Login (if needed)
- Browse videos
- Play a video

### 4.4 Check Cache in Network Tab
- Find `.m3u8` or `.ts` files
- Click on one
- Look at **Response Headers**
- Should see: `X-Cache-Status: MISS` (first time)

### 4.5 Refresh and Check Again
- Press **F5** to refresh
- Same video file
- Should see: `X-Cache-Status: HIT` (faster!)

---

## ✅ Step 5: Verify Everything is Working

### Checklist:

- [ ] Backend running on port 5000
- [ ] Edge server responding on port 8080
- [ ] Frontend loads at localhost:3000
- [ ] Can login successfully
- [ ] Videos load and play
- [ ] No CORS errors in console
- [ ] Cache shows MISS → HIT
- [ ] HIT request is faster

**If ALL checked: SUCCESS!** ✅

---

## 🎉 Success Indicators

### ✅ Edge Server Working
```bash
curl http://localhost:8080/health
# Output: "Edge server is healthy"
```

### ✅ API Working
```bash
curl http://localhost:8080/api/videos
# Output: JSON with video data
```

### ✅ Frontend Working
- Opens at localhost:3000
- No errors in console
- All features work

### ✅ Caching Working
- First video request: X-Cache-Status: MISS
- Second video request: X-Cache-Status: HIT
- HIT is 5-10x faster

---

## 🐛 Troubleshooting

### Issue: Port 5000 in use
```bash
# Find what's using port 5000
sudo lsof -i :5000
# Kill the process if needed
```

### Issue: Port 3000 in use
```bash
# Find what's using port 3000
sudo lsof -i :3000
# Kill or use different port
```

### Issue: Nginx not running
```bash
# Check Nginx status
sudo systemctl status nginx

# Start Nginx
sudo systemctl start nginx

# Check configuration
sudo nginx -t
```

### Issue: CORS errors
- Make sure backend is running on port 5000
- Check CORS allows localhost:8080

### Issue: Videos not loading
- Check video status is "ready"
- Check backend logs for errors

---

## 📊 Expected Results

### Performance Comparison

| Request | Cache Status | Time | Speed |
|---------|-------------|------|-------|
| 1st | MISS | 145ms | Baseline |
| 2nd | HIT | 12ms | 12x faster! |

### Cache Directory

```bash
# Check cache is growing
ls -lh /var/cache/nginx/pakstream/

# Should show cached files after playing videos
```

---

## ✅ Final Verification

Run this in a new terminal:

```bash
# Test 1: Health
curl http://localhost:8080/health

# Test 2: API
curl http://localhost:8080/api/videos

# Test 3: Frontend .env
cat ~/projects/PakStream/frontend/.env

# Should show:
# REACT_APP_API_URL=http://localhost:8080/api
# REACT_APP_SOCKET_URL=http://localhost:8080
```

---

## 🎯 Summary

**What's Running:**
- ✅ Backend on port 5000 (Origin)
- ✅ Edge server on port 8080 (Nginx)
- ✅ Frontend on port 3000 (Dev)

**What Happens:**
- Frontend calls → localhost:8080
- Edge server checks cache
- If MISS → fetch from localhost:5000
- If HIT → serve from cache (fast!)
- Edge caches the response for next time

**Result:**
- Faster video loading ⚡
- Reduced origin load 📉
- Better performance 🚀

---

## 🎊 You're Ready!

If all tests pass, your CDN implementation is complete and working!

**Next:** Review the report (`PAKSTREAM_CDN_IMPLEMENTATION_REPORT.md`) for presentation.

Good luck! 🚀
