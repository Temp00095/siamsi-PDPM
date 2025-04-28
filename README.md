# siamsi-PDPM
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>เซียมซีออนไลน์</title>
    <style>
        body {
            font-family: 'Sarabun', sans-serif;
            background-color: #f5f5f5;
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
            background-image: url('https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/temple_bg.jpg');
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
            color: #333;
        }

        .container {
            background-color: rgba(255, 255, 255, 0.9);
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
            padding: 20px;
            margin: 20px;
            text-align: center;
            max-width: 500px;
            width: 90%;
        }

        h1 {
            color: #d4145a;
            margin-bottom: 10px;
            font-size: 2rem;
        }

        .fortune {
            display: none;
            margin: 20px 0;
        }

        .fortune-number {
            font-size: 5rem;
            font-weight: bold;
            color: #d4145a;
            display: block;
            margin: 20px 0;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
        }

        .fortune-text {
            font-size: 1.2rem;
            line-height: 1.6;
            margin-bottom: 20px;
            text-align: left;
            padding: 0 10px;
        }

        button {
            background-color: #d4145a;
            color: white;
            border: none;
            padding: 12px 24px;
            font-size: 1.2rem;
            border-radius: 30px;
            cursor: pointer;
            margin: 10px 0;
            transition: background-color 0.3s, transform 0.2s;
            font-family: 'Sarabun', sans-serif;
        }

        button:hover {
            background-color: #ac0e49;
            transform: scale(1.05);
        }

        button:active {
            transform: scale(0.98);
        }

        .shake-animation {
            animation: shake 0.5s ease-in-out;
        }

        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            20%, 60% { transform: translateX(-10px); }
            40%, 80% { transform: translateX(10px); }
        }

        .qr-section {
            margin-top: 20px;
        }

        .qr-code {
            margin: 10px auto;
            padding: 10px;
            background: white;
            border-radius: 5px;
            display: inline-block;
        }

        footer {
            margin-top: auto;
            padding: 10px;
            color: white;
            text-align: center;
            background-color: rgba(0, 0, 0, 0.5);
            width: 100%;
        }
        
        @media (max-width: 480px) {
            h1 {
                font-size: 1.7rem;
            }
            .fortune-number {
                font-size: 4rem;
            }
        }
    </style>
    <link href="https://fonts.googleapis.com/css2?family=Sarabun:wght@400;700&display=swap" rel="stylesheet">
</head>
<body>
    <div class="container">
        <h1>เซียมซีออนไลน์</h1>
        <p>อธิษฐานจิต แล้วกดปุ่มเพื่อเสี่ยงเซียมซี</p>
        
        <button id="drawButton">เสี่ยงเซียมซี</button>
        
        <div id="fortune" class="fortune">
            <span class="fortune-number" id="fortuneNumber"></span>
            <p class="fortune-text" id="fortuneText"></p>
            <button id="drawAgainButton">เสี่ยงอีกครั้ง</button>
        </div>
    </div>

    <div class="container qr-section">
        <h2>แชร์เซียมซีให้เพื่อน</h2>
        <p>สแกน QR Code นี้เพื่อเข้าถึงเซียมซีออนไลน์</p>
        <div id="qrcode" class="qr-code"></div>
    </div>

    <footer>
        &copy; 2025 เซียมซีออนไลน์ - ใช้เพื่อความเพลิดเพลินเท่านั้น
    </footer>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    <script>
        // คำทำนายสำหรับเซียมซีแต่ละใบ
        const fortunes = {
            1: "เซียมซีใบที่ 1 (ดีมาก): ดวงของท่านกำลังเฟื่องฟู ความสำเร็จรออยู่ข้างหน้า ทุกสิ่งที่ท่านวางแผนจะสมหวังดังใจ ความสุขจะเข้ามาในชีวิตท่านในเร็ววัน มีโชคลาภทางการเงิน",
            
            2: "เซียมซีใบที่ 2 (ดี): ความพยายามของท่านจะเกิดผล แม้จะมีอุปสรรคบ้าง แต่ท่านจะผ่านไปได้ด้วยดี การงานจะมีความก้าวหน้า คนรอบข้างให้การสนับสนุน ระวังการใช้จ่ายฟุ่มเฟือย",
            
            3: "เซียมซีใบที่ 3 (ปานกลาง): ชีวิตมีขึ้นมีลง ปัญหาที่ผ่านมาจะค่อยๆ คลี่คลาย ควรใช้สติในการตัดสินใจ อย่ารีบร้อนหรือใจร้อน การเงินไม่ขัดสน แต่ไม่ควรประมาท ความรักต้องใช้ความอดทน",
            
            4: "เซียมซีใบที่ 4 (ปานกลาง): ชีวิตกำลังอยู่ในช่วงเปลี่ยนผ่าน ท่านอาจรู้สึกสับสนหรือไม่มั่นใจ ควรพิจารณาสิ่งต่างๆ ให้รอบคอบ สุขภาพควรได้รับการดูแลเป็นพิเศษ มิตรแท้จะช่วยเหลือในยามยาก",
            
            5: "เซียมซีใบที่ 5 (ดี): โชคดีกำลังเข้าข้างท่าน ปัญหาที่เคยมีจะได้รับการแก้ไข มีคนคอยสนับสนุนช่วยเหลือ การงานมีความก้าวหน้า การเงินมีเข้ามาอย่างต่อเนื่อง ความรักเริ่มชัดเจนขึ้น",
            
            6: "เซียมซีใบที่ 6 (ปานกลาง): ต้องอดทนและรอคอย ความสำเร็จไม่ได้มาง่ายๆ ระวังคนใกล้ตัวที่ไม่หวังดี ควรรักษาเงินเก็บไว้ใช้ยามจำเป็น เรื่องรักอาจมีอุปสรรค แต่จะผ่านไปได้หากมีความจริงใจ",
            
            7: "เซียมซีใบที่ 7 (ดีมาก): ชีวิตกำลังรุ่งเรือง สิ่งที่ทำไว้จะได้รับผลตอบแทนคุ้มค่า มีโอกาสดีๆ เข้ามา ควรฉวยไว้ให้มั่น การเงินมั่นคง ความรักหวานชื่น สุขภาพแข็งแรง",
            
            8: "เซียมซีใบที่ 8 (ไม่ดี): ระวังปัญหาที่ไม่คาดคิด อย่าประมาทในทุกด้าน การเงินอาจมีรายจ่ายฉุกเฉิน ควรระมัดระวังคำพูดที่อาจนำไปสู่ความขัดแย้ง ดูแลสุขภาพให้ดี ปัญหาจะผ่านไปในที่สุด",
            
            9: "เซียมซีใบที่ 9 (ปานกลาง): ชีวิตกำลังอยู่ในช่วงเปลี่ยนแปลง สิ่งเก่าจะจากไป สิ่งใหม่จะเข้ามา ไม่ควรยึดติดกับอดีตมากเกินไป เปิดใจให้กว้าง โอกาสดีรออยู่ข้างหน้า การเงินพอมีพอใช้",
            
            10: "เซียมซีใบที่ 10 (ดีมาก): ดวงดีเป็นพิเศษ ความสำเร็จจะมาถึงในเร็ววัน อุปสรรคจะหมดไป มีลาภลอยมาอย่างไม่คาดฝัน คนรอบข้างชื่นชม การงานก้าวหน้า ความรักสดใส"
        };

        document.addEventListener('DOMContentLoaded', function() {
            const drawButton = document.getElementById('drawButton');
            const drawAgainButton = document.getElementById('drawAgainButton');
            const fortuneDiv = document.getElementById('fortune');
            const fortuneNumber = document.getElementById('fortuneNumber');
            const fortuneText = document.getElementById('fortuneText');
            const container = document.querySelector('.container');

            // สร้าง QR Code
            const currentUrl = window.location.href;
            new QRCode(document.getElementById("qrcode"), {
                text: currentUrl,
                width: 150,
                height: 150,
                colorDark: "#000000",
                colorLight: "#ffffff",
                correctLevel: QRCode.CorrectLevel.H
            });

            // ฟังก์ชันสุ่มเลขเซียมซี
            function drawFortune() {
                container.classList.add('shake-animation');
                
                // เล่นเสียงเขย่าเซียมซี (ถ้ามี)
                
                // หน่วงเวลาสักครู่เพื่อให้มีเอฟเฟกต์เขย่า
                setTimeout(function() {
                    // สุ่มเลข 1-10
                    const randomNumber = Math.floor(Math.random() * 10) + 1;
                    
                    // แสดงผลลัพธ์
                    fortuneNumber.textContent = randomNumber;
                    fortuneText.textContent = fortunes[randomNumber];
                    fortuneDiv.style.display = 'block';
                    drawButton.style.display = 'none';
                    
                    container.classList.remove('shake-animation');
                }, 500);
            }

            // เมื่อกดปุ่มเสี่ยงเซียมซี
            drawButton.addEventListener('click', drawFortune);
            
            // เมื่อกดปุ่มเสี่ยงอีกครั้ง
            drawAgainButton.addEventListener('click', function() {
                fortuneDiv.style.display = 'none';
                drawButton.style.display = 'block';
            });
        });
    </script>
</body>
</html>
