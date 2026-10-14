# ANSWER — ดุลยฤทธิ์ บุญเรือน 671540005007-3

- Platform: Vercel
- Production URL: https://ksu-coffee-671540005007-3.vercel.app

## คำตอบ

1. เลือก Vercel ซึ่งรันโค้ดแบบ Serverless โดยไฟล์ใน `public/` ถูกเสิร์ฟเป็น static ผ่าน CDN และ `api/info.js` ถูกแปลงเป็น serverless function ที่ทำงานเฉพาะตอนมี request (เห็นได้จาก Function Invocations ใน dashboard) บน production จึงไม่ได้ใช้ `server.js` เพราะ Vercel ไม่รัน server ค้างไว้ตลอด `server.js` ใช้เฉพาะตอนทดสอบในเครื่อง

2. ตั้งค่า STUDENT_ID ที่ Vercel → Project → Settings → Environment Variables (เลือก Production) ห้าม commit `.env` เพราะ repo เป็น Public ค่าลับจะรั่ว และค่าควรแยกตาม environment ถ้า `/api/info` แสดง NOT SET อาจเกิดจาก ตั้ง env หลัง deploy แล้วยังไม่ redeploy, สะกดชื่อตัวแปรผิด หรือไม่ได้เลือก environment Production

3. ครั้งแรก 1.465915 วินาที / ครั้งที่สอง 0.442189 วินาที — ครั้งแรกช้ากว่าประมาณ 3 เท่า เพราะเกิด cold start คือ Vercel ต้องเริ่ม serverless function ขึ้นมาใหม่ก่อนประมวลผล ครั้งที่สองเรียกซ้ำทันที function ยังอุ่นอยู่ (warm) จึงตอบได้เร็วกว่า

4. git push → GitHub ส่ง webhook ไปยัง Vercel → Vercel ดึง commit ล่าสุดจาก branch main มา build → สร้าง deployment ใหม่ → เมื่อสถานะ Ready จะสลับ production domain ไปชี้ deployment ใหม่ หน้าเว็บจึงเป็น Menu v2 ค่า commit ใน /api/info (219e1ef) ตรงกับ git log และตรงกับ Source ใน Vercel dashboard

5. นำมา deploy ตรง ๆ ไม่ได้ เพราะ Vercel ไม่รองรับการรัน Docker container หรือ docker-compose เลย รองรับแค่ static files และ serverless functions ระบบข้อ 1 ที่ใช้ nginx container และ image promo จาก Docker Hub จึงต้องย้ายไปแพลตฟอร์มที่รัน container ได้ เช่น VPS หรือ cloud ที่รองรับ Docker