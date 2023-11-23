# minikube-wsl2-openlens-without-docker-desktop
การติดตั้ง Kubernetes, Ingress controllers บน WSL2 โดยไม่ใช้ Docker Desktop และการใช้ OpenLens 

การเริ่มต้นใช้งาน Kubernetes cluster บน cloud ต่างๆ ไม่ว่าจะเป็น AWS, GCP, Azure นั้นทำได้ง่ายมากๆ
แต่สิ่งที่เราควรทำก็คือ การจำลอง environment บนเครื่อง local ของเรา ให้มีความใกล้เคียงกับบน cloud ให้มากที่สุด
เพื่อให้การ develop จากข้างล่างขึ้นไปข้างบนมีความ smooth ที่สุด รวมไปถึงการทำเพื่อ debug, test ที่เครื่อง local ของเราด้วย

บทความนี้จะเป็นการแนะนำขั้นตอนการติดตั้ง Kubernetes, Ingress controller และใช้ OpenLens เพื่อจัดการได้ง่ายขึ้น
ซึ่งในบทความนี้ จะเป็นการติดตั้งบน Windows 10, 11 แต่ก่อนที่จะไปต่อ เรากลับมาตอบคำถามบนหัวข้อของเราก่อนว่า ทำไม ทำไม และทำไม

ถาม: ทำไมไม่ใช้ Docker Desktop?
ตอบ: จริงอยู่ว่า Docker Desktop เป็นของที่ดีมากๆ  และสามารถใช้งานได้ฟรีๆ (ถ้าคุณทำงานเป็น individual developer)
แต่เมื่อไหร่ก็ตามที่องค์กรคุณเป็นองค์กรขนาดกลางขึ้นไป มีคนใช้งาน Docker Desktop เยอะ (แน่นอนล่ะ ของดีๆ ใครก็อยากใช้)
ทาง Docker จะจับได้ ผ่าน IP, login, pull ของคน, system ทั้งหลายในองค์กรคุณที่ใช้งาน Docker Desktop
เค้าก็จะแนะนำให้องค์กรคุณ subscriptions ในราคาของ Team หรืออาจจะ Business (ถ้ามีการใช้งานเกิน 100 users)
ถ้าองค์กรของคุณ ไม่จ่ายแผนตามที่เค้าแนะนำ ทั้งองค์กรคุณอาจจะโดน block IP และใช้งาน Docker Desktop ไม่ได้อีกต่อไป

ถาม: ทำไมต้องใช้ WSL2
ตอบ: เพราะมันมีความเป็น Linux มากกว่า ทำอะไรได้มากกว่า Hyper-V ประสิทธิภาพดีกว่า และใช้ทรัพยากรน้อยกว่า ก็คือดีกว่าในหลายๆด้าน
อีกอย่าง ถ้า Windows ไม่ใช่ตัว Pro ขึ้นไป ก็หมดสิทธิ์ใช้ Hyper-V ซึ่งในมุมมองของผม จากที่เปรียบเทียบดู ก็ไม่ได้น่าใช้ตั้งแต่แรกอยู่แล้ว

ถาม: ทำไมต้องใช้ OpenLens?
ตอบ: มันจะมี Lens Desktop Pro ที่ปัจจุบันไม่มีเวอร์ชั่นฟรีแล้ว ต้องจ่ายราย users ซึ่งถ้าคุณหรือองค์กรคุณจ่ายไหว คุณก็ไป Lens
แต่ถ้าคุณหรือองค์กรคุณจ่ายไม่ไหว OpenLens ก็เป็นเครื่องมือที่ใช้จัดการ Kubernetes เวอร์ชั่นฟรี ที่ใช้ได้ดีเหมือนกันนะ 
ถ้าคุณยังไม่สนใจ นอกจาก 2 ตัวนี้ ก็ยังมี Devtron, Monokle, k9s ที่เป็น tools คล้ายๆกัน ก็ลองเปรียบเทียบและเลือกใช้ดูครับ

กลับไปทำตามหัวข้อของเรากัน โดยเริ่มติดตั้งกันเลย

1. WSL2 (Windows Subsystem for Linux version 2)
เป็น feature บน Windows ที่จะทำให้คุณใช้งาน Linux environment ได้อย่างดี การติดตั้งทำได้ง่ายมาก บน Windows 10/11 
เพียงแค่เปิด PowerShell หรือ CMD ด้วย "Run as administrator" และ run คำสั่งนี้

```
wsl --install
```

คุณก็จะได้ WSL2 แล้ว เพราะการติดตั้งปัจจุบันจะเลือก version 2 ให้โดย default อยู่แล้ว
