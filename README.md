<!DOCTYPE html>
<html lang="vi">
<head>
    <style>
        .pixel-fullscreen-overlay {
            position: fixed;
            top: 0; left: 0; width: 100vw; height: 100vh;
            background: #fff;
            z-index: 99999;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
        }
        .pixel-fullscreen-text {
            font-size: clamp(4rem, 15vw, 12rem);
            font-weight: bold;
            color: #007bff;
            text-align: center;
            user-select: none;
            text-shadow: 0 2px 16px #aaa;
        }
    </style>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bài Tập Lập Trình Web</title>
    <style>
        /* --- Giao diện Sáng (Mặc định) --- */
        :root {
            --bg-color: #f0f2f5;
            --container-bg: white;
            --text-color: #1c1e21;
            --link-color: #007bff;
            --border-color: #ddd;
            --hover-bg: #e9ecef;
            --list-item-bg: #f8f9fa;
            --correct-bg: #d4edda;
            --correct-border: #28a745;
            --correct-text: #155724;
            --incorrect-bg: #f8d7da;
            --incorrect-border: #dc3545;
            --incorrect-text: #721c24;
            --passage-text: #555;
        }

        body { 
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; 
            background-color: var(--bg-color); 
            color: var(--text-color);
            padding: 40px; 
            margin: 0; 
            transition: background-color 0.3s, color 0.3s;
        }

        .container { 
            /* max-width: 800px; */
            margin: 0 auto; 
            background: var(--container-bg); 
            padding: 20px 40px; 
            border-radius: 12px; 
            box-shadow: 0 6px 20px rgba(0,0,0,0.08); 
            transition: background-color 0.3s;
            position: relative;
        }

        h1, h2, h3 { color: var(--text-color); }
        p, li { line-height: 1.6; }
        hr { border: none; border-top: 1px solid var(--border-color); margin: 30px 0; }
        
        .back-link, a { 
            color: var(--link-color); 
            text-decoration: none; 
            font-weight: 500; 
        }

        .exercise-link {
            font-size: 1.2em; 
            background-color: var(--list-item-bg); 
            display: block; 
            padding: 20px; 
            border-radius: 8px; 
            border: 1px solid var(--border-color);
            transition: background-color 0.2s, transform 0.2s ease-out;
        }
        .exercise-link:hover {
            background-color: var(--hover-bg);
            transform: scale(1.02);
        }

        /* Quiz Styles */
        .passage {
            font-size: 0.9em;
            font-style: italic;
            color: var(--passage-text);
            background-color: var(--list-item-bg);
            padding: 10px 15px;
            border-radius: 6px;
            margin: 10px 0;
            border-left: 4px solid var(--border-color);
        }

        .quiz-form .question-block, .result-detail .question-block { 
            margin-bottom: 30px; 
            opacity: 0;
            transform: translateY(20px);
            animation: fadeInUP 0.5s ease-out forwards;
        }
        .quiz-form .options label { 
            display: block; margin: 8px 0; padding: 10px; 
            border: 1px solid var(--border-color); border-radius: 6px; 
            cursor: pointer; transition: background-color 0.2s;
        }
        .quiz-form .options label:hover { background-color: var(--hover-bg); }
        .quiz-form .options input { margin-right: 10px; }
        .fill-in-blank-input { 
            padding: 10px; font-size: 1em; width: 95%; 
            border: 1px solid var(--border-color); border-radius: 4px;
            background-color: var(--container-bg); color: var(--text-color);
        }
        .quiz-submit-btn { 
            background-color: #007bff; color: white; padding: 12px 25px; 
            border: none; border-radius: 6px; cursor: pointer; font-size: 1.1em;
            transition: transform 0.2s, box-shadow 0.2s;
        }
        .quiz-submit-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 10px rgba(0, 123, 255, 0.3);
        }
        
        /* --- HIỆU ỨNG CHUYỂN TRANG --- */
        .page-content { 
            display: none;
            opacity: 0;
            transition: opacity 0.4s ease-in-out;
        }
        .page-content.active {
            display: block;
            opacity: 1;
        }
        
        /* --- HIỆU ỨNG TẢI TRANG (PRELOADER) --- */
        #preloader {
            position: fixed; top: 0; left: 0;
            width: 100%; height: 100%;
            background-color: var(--bg-color);
            z-index: 9999;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: opacity 0.5s ease;
        }
        #preloader.hidden {
            opacity: 0;
            pointer-events: none;
        }
        .spinner {
            border: 4px solid var(--hover-bg);
            border-top: 4px solid var(--link-color);
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* --- KEYFRAME CHO HIỆU ỨNG CÂU HỎI --- */
        @keyframes fadeInUP {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        /* --- Result Styles --- */
        .result-summary { text-align: center; padding: 20px; background: var(--list-item-bg); border-radius: 8px; margin-bottom: 20px; }
        .result-summary h2 { font-size: 2.5em; color: var(--link-color); margin-bottom: 5px; }
        .result-summary p { margin-top: 5px; font-size: 1.1em; color: var(--text-color); }
        .result-detail .question-block { padding: 15px; border-radius: 6px; }
        .result-detail .correct { border-left: 5px solid var(--correct-border); background-color: var(--correct-bg); }
        .result-detail .incorrect { border-left: 5px solid var(--incorrect-border); background-color: var(--incorrect-bg); }
        .result-detail .user-answer, .result-detail .correct-answer-text { font-weight: bold; }
        .result-detail .correct-answer-text { color: var(--correct-text); }
        .result-detail .correct, .result-detail .correct p, .result-detail .correct b, .result-detail .correct i { color: var(--correct-text); }
        .result-detail .incorrect, .result-detail .incorrect p, .result-detail .incorrect b, .result-detail .incorrect i { color: var(--incorrect-text); }

        /* --- Nút Chuyển Giao Diện --- */
        .theme-switcher {
            position: absolute; top: 20px; right: 20px; cursor: pointer;
            background-color: var(--hover-bg); border: 1px solid var(--border-color);
            border-radius: 50%; width: 40px; height: 40px;
            display: flex; align-items: center; justify-content: center;
            font-size: 20px; user-select: none; z-index: 1001;
            transition: transform 0.2s;
        }
        .theme-switcher:hover { transform: scale(1.1); }
        .theme-switcher .icon { display: none; }

        /* --- Giao diện Tối --- */
        body.dark-mode {
            --bg-color: #121212;
            --container-bg: #1e1e1e;
            --text-color: #e0e0e0;
            --link-color: #8ab4f8;
            --border-color: #444;
            --hover-bg: #2a2a2a;
            --list-item-bg: #252525;
            --correct-bg: #2a3b2e;
            --correct-text: #a5d6a7;
            --incorrect-bg: #402c2f;
            --incorrect-text: #ef9a9a;
            --passage-text: #aaa;
        }
        body.dark-mode .theme-switcher .sun-icon { display: block; }
        body:not(.dark-mode) .theme-switcher .moon-icon { display: block; }
        
        /* --- HIỆU ỨNG PHÁO HOA VÀ MODAL --- */
        .fireworks-container {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            z-index: 1000; background: rgba(0, 0, 0, 0.5); display: none;
        }
        .celebration-modal {
            position: fixed; top: 50%; left: 50%;
            transform: translate(-50%, -50%) scale(0.9);
            background-color: var(--container-bg); padding: 30px 40px; border-radius: 12px;
            text-align: center; box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            z-index: 1001; opacity: 0; transition: opacity 0.3s, transform 0.3s;
        }
        .fireworks-container.active .celebration-modal {
            opacity: 1; transform: translate(-50%, -50%) scale(1);
        }
        .celebration-modal h2 { font-size: 2em; margin-bottom: 10px; }
        .celebration-modal p { font-size: 3em; font-weight: bold; color: var(--link-color); margin: 10px 0; }
        .celebration-modal button {
            background-color: #007bff; color: white; padding: 10px 30px; 
            border: none; border-radius: 6px; cursor: pointer; font-size: 1.1em; margin-top: 20px;
            transition: transform 0.2s, box-shadow 0.2s;
        }
        .celebration-modal button:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 10px rgba(0, 123, 255, 0.3);
        }
    </style>
</head>
<body>

<div id="preloader">
    <div class="spinner"></div>
</div>

<div class="container">
    <div class="theme-switcher" id="themeToggle" title="Chuyển đổi giao diện Sáng/Tối">
        <span class="icon sun-icon">☀️</span>
        <span class="icon moon-icon">🌙</span>
    </div>

    <div id="page-home" class="page-content">
        <h1>📚 Danh Sách Bài Tập 📚</h1>
        <ul style="list-style: none; padding: 0;">
            <li style="margin: 15px 0;">
                <a href="#bai1" class="exercise-link">Bài tập 1: Các thẻ HTML cơ bản</a>
            </li>
            <li style="margin: 15px 0;">
                <a href="#bai7" class="exercise-link">Bài tập 7: Ngoại Ngữ Ngành CNTT (Bài đầy đủ)</a>
            </li>
        </ul>
    </div>

    <div id="page-bai1" class="page-content">
        <a class="back-link" href="#home">← Quay lại danh sách</a>
        <h1>📝 Bài tập 1: Các thẻ HTML cơ bản</h1>
        <div>
            <p><strong>Yêu cầu:</strong> Dựa vào kiến thức đã học, hãy tạo một file HTML duy nhất thể hiện các yêu cầu sau:</p>
            <ol>
                <li>Tạo một tiêu đề chính `<h1>` với nội dung "Trang Web Đầu Tiên Của Tôi".</li>
                <li>Tạo một đoạn văn `<p>` giới thiệu về bản thân bạn.</li>
                <li>Tạo một danh sách không có thứ tự `<ul>` liệt kê 3 sở thích của bạn.</li>
            </ol>
            <p>Sau khi làm xong, hãy nén file HTML đó thành file `.zip` và nộp xuống form bên dưới.</p>
        </div>
        <hr>
        <div style="background: var(--list-item-bg); padding: 30px; border-radius: 8px; border: 1px solid var(--border-color);">
            <h2>Nộp Bài Của Bạn Tại Đây</h2>
            <form>
                <p>Chọn tệp bạn muốn nộp:</p>
                <input type="file" name="fileToUpload"><br><br>
                <input type="submit" value="Nộp Bài" style="background-color: #28a745; color: white; padding: 12px 25px; border: none; border-radius: 6px; cursor: pointer; font-size: 1em;">
            </form>
            <p><small><i>Lưu ý: Form nộp bài này chỉ là giao diện và không có chức năng xử lý.</i></small></p>
        </div>
    </div>

    <div id="page-bai7" class="page-content">
        <a class="back-link" href="#home">← Quay lại danh sách</a>
        <h1>📝 Bài tập 7: Ngoại Ngữ Ngành CNTT (Bài đầy đủ)</h1>
        <form class="quiz-form" id="quizForm">
        </form>
    </div>

    <div id="page-results" class="page-content">
    </div>
</div>

<div class="fireworks-container" id="fireworksContainer">
    <div class="celebration-modal">
        <h2>Chúc mừng!</h2>
        <p id="finalScore"></p>
        <button id="okButton">OK</button>
    </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/fireworks-js@2.10.7/dist/index.umd.js"></script>

<script>
    // Hiệu ứng full màn hình cho chữ pixel
    document.addEventListener('DOMContentLoaded', function() {
        document.querySelectorAll('.pixel-line').forEach(function(el) {
            el.addEventListener('click', function() {
                // Tạo overlay
                let overlay = document.createElement('div');
                overlay.className = 'pixel-fullscreen-overlay';
                let text = document.createElement('div');
                text.className = 'pixel-fullscreen-text';
                text.textContent = el.textContent;
                overlay.appendChild(text);
                document.body.appendChild(overlay);
                // Ẩn container
                document.querySelector('.container').style.visibility = 'hidden';
                // Thoát khi bấm overlay
                overlay.addEventListener('click', function() {
                    overlay.remove();
                    document.querySelector('.container').style.visibility = 'visible';
                });
            });
        });
    });
    // =====================================================================
    // DỮ LIỆU BÀI TẬP VÀ ĐÁP ÁN
    // =====================================================================
    const quizzes = {
   "BÀI 1":[
    { type:"mc", question:"1. Để mở một tập tin, bạn nên nhấp đúp vào biểu tượng của nó.", answers:{a:"To delete a file, you should double-click its icon.", b:"To open a file, you should double-click its icon.", c:"To close a file, you should right-click its icon."}, correctAnswer:"b" },
    { type:"mc", question:"2. Tớ không thể kết nối Wi-Fi từ biểu tượng mạng.", answers:{a:"I can’t download files from the folder icon.", b:"I can’t connect to the printer from the recycle bin.", c:"I can’t connect to the Wi-Fi from the network icon."}, correctAnswer:"c" },
    { type:"mc", question:"3. Nếu bạn muốn phóng to cửa sổ, hãy nhấn nút maximize.", answers:{a:"If you want to minimize a window, press the maximize button.", b:"If you want to close a window, press the icon.", c:"If you want to maximize a window, press the maximize button."}, correctAnswer:"c" },
    { type:"mc", question:"4. Tớ cần thêm RAM để máy tính chạy nhanh hơn.", answers:{a:"I need more webcam to make my computer clearer.", b:"I need more hard drive to make my computer slower.", c:"I need more RAM to make my computer faster."}, correctAnswer:"c" },
    { type:"mc", question:"5. Tớ không thể mở ứng dụng từ biểu tượng trên màn hình.", answers:{a:"I can’t delete any apps from the desktop folder.", b:"I can’t open the app from the icon on the desktop.", c:"I can’t open any external hardware drives in the taskbar."}, correctAnswer:"b" },
    { type:"mc", question:"6. A: I'm not sure why the installation failed. B: .......... your antivirus software and try again. It might block the setup.", answers:{a:"To turn off", b:"Turns off", c:"Turn off"}, correctAnswer:"c" },
    { type:"mc", question:"7. A: The icons on my desktop are too small to read. B: ........... the screen resolution in display settings to make them larger.", answers:{a:"Adjusts", b:"To adjust", c:"Adjust"}, correctAnswer:"c" },
    { type:"mc", question:"8. .......... the icon to open the application.", answers:{a:"Double-click", b:"Double-clicked", c:"Double-clicking"}, correctAnswer:"a" },
    { type:"mc", question:"9. …………… on the \"Start\" button to access the system settings menu.", answers:{a:"Click", b:"Clicks", c:"Clicked"}, correctAnswer:"a" },
    { type:"mc", question:"10. ........ the minimum system requirements before installing the new graphic software.", answers:{a:"Checking", b:"Check", c:"Checks"}, correctAnswer:"b" },

    { type:"mc", question:"11. What does a computer need to run multiple tasks at a time smoothly?", answers:{a:"enough RAM", b:"the system performance", c:"RAM"}, correctAnswer:"a", passage:"RAM (Random Access Memory) is very important for the performance of a computer system. The more RAM a computer has, the more smoothly it can run multiple programs at the same time. For example, if you are running a web browser, a music player, and a game at the same time, your computer needs enough RAM to handle all these tasks. If the RAM is too low, the system may slow down or even freeze."},
    { type:"mc", question:"12. What is one main purpose of a GUI?", answers:{a:"To help users interact with the computer more easily", b:"To increase processor speed", c:"To install more storage space"}, correctAnswer:"a", passage:"A Graphical User Interface (GUI) uses icons, buttons, and windows that allow users to interact with the computer easily. Unlike a Command Line Interface, a GUI is more user-friendly because users can click and drag items with a mouse. Most operating systems like Windows, macOS, and Linux have GUIs. It helps beginners navigate and operate software without typing commands."},
    { type:"mc", question:"13. Why is it important to check the system requirements before installing software?", answers:{a:"To make sure the software is free", b:"To ensure the software works properly on the computer", c:"To choose the best brand of computer"}, correctAnswer:"b", passage:"Before installing software, users must check the system requirements. These include the operating system, processor speed, RAM, and storage space. If the computer does not meet the minimum requirements, the software may not run properly. Some programs also need a specific type of graphics card or internet connection. Always read the specifications carefully before downloading."},
    { type:"mc", question:"14. What is the effect of a higher screen resolution?", answers:{a:"It uses fewer pixels.", b:"It gives good quality images and more detail.", c:"It makes the screen smaller."}, correctAnswer:"b", passage:"Screen resolution refers to the number of pixels on a screen. A higher resolution means sharper images and more detail. Common resolutions are 1366x768 or 1920x1080. When choosing a monitor or adjusting display settings, it is important to consider resolution. A low-resolution screen may make text look shadowy or images unclear. This affects both comfort and productivity, especially during long use."},
    { type:"mc", question:"15. Which is not an input device?", answers:{a:"tables", b:"touchscreens", c:"keyboards"}, correctAnswer:"a", passage:"Input devices like a mouse and keyboard are essential for using a Graphical User Interface (GUI). A mouse helps move the pointer and select icons. A keyboard is used to enter text and shortcuts. Touchscreens, found in tablets and smartphones, are also input devices that support GUI operations. These tools make it easier for users to work with software."},

    { 
      type:"fill", 
      question:"Summary: A GUI uses icons and windows, letting users interact easily by clicking and dragging. It’s **...........** than command-lines, helping beginners use software without typing commands.", 
      answer:"more user-friendly", 
      passage:`A Graphical User Interface (GUI) uses icons, buttons, and windows that allow users to interact with the computer easily. Unlike a Command Line Interface, a GUI is more user-friendly because users can click and drag items with a mouse. Most operating systems like Windows, macOS, and Linux have GUIs. It helps beginners navigate and operate software without typing commands.`
    },
    { 
      type:"fill", 
      question:"Summary: Check system specifications like processor, RAM, storage, OS, and graphics card when buying a computer. These help **...........** and choose the right device for tasks like gaming or video editing.", 
      answer:"the computer run smoothly", 
      passage:`When buying a computer, it’s important to check system specifications such as processor type, RAM size, storage capacity, operating system, and graphics card. A fast processor and sufficient RAM help the computer run smoothly. A strong graphics card is essential for gaming or video editing. Understanding these specifications helps avoid future problems like slow performance or software issues.`
    },
    { 
      type:"fill", 
      question:"Summary: A<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bài Tập Lập Trình Web</title>
    <style>
        .pixel-fullscreen-overlay {
            position: fixed;
            top: 0; left: 0; width: 100vw; height: 100vh;
            background: #fff;
            z-index: 99999;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
        }
        .pixel-fullscreen-text {
            font-size: clamp(4rem, 15vw, 12rem);
            font-weight: bold;
            color: #007bff;
            text-align: center;
            user-select: none;
            text-shadow: 0 2px 16px #aaa;
        }
    
        /* --- Giao diện Sáng (Mặc định) --- */
        :root {
            --bg-color: #f0f2f5;
            --container-bg: white;
            --text-color: #1c1e21;
            --link-color: #007bff;
            --border-color: #ddd;
            --hover-bg: #e9ecef;
            --list-item-bg: #f8f9fa;
            --correct-bg: #d4edda;
            --correct-border: #28a745;
            --correct-text: #155724;
            --incorrect-bg: #f8d7da;
            --incorrect-border: #dc3545;
            --incorrect-text: #721c24;
            --passage-text: #555;
        }

        body { 
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; 
            background-color: var(--bg-color); 
            color: var(--text-color);
            padding: 40px; 
            margin: 0; 
            transition: background-color 0.3s, color 0.3s;
        }

        .container { 
            max-width: 800px;
            margin: 0 auto; 
            background: var(--container-bg); 
            padding: 20px 40px; 
            border-radius: 12px; 
            box-shadow: 0 6px 20px rgba(0,0,0,0.08); 
            transition: background-color 0.3s;
            position: relative;
        }

        h1, h2, h3 { color: var(--text-color); }
        p, li { line-height: 1.6; }
        hr { border: none; border-top: 1px solid var(--border-color); margin: 30px 0; }
        
        .back-link, a { 
            color: var(--link-color); 
            text-decoration: none; 
            font-weight: 500; 
        }

        .exercise-link {
            font-size: 1.2em; 
            background-color: var(--list-item-bg); 
            display: block; 
            padding: 20px; 
            border-radius: 8px; 
            border: 1px solid var(--border-color);
            transition: background-color 0.2s, transform 0.2s ease-out;
        }
        .exercise-link:hover {
            background-color: var(--hover-bg);
            transform: scale(1.02);
        }

        /* Quiz Styles */
        .passage {
            font-size: 0.9em;
            font-style: italic;
            color: var(--passage-text);
            background-color: var(--list-item-bg);
            padding: 10px 15px;
            border-radius: 6px;
            margin: 10px 0;
            border-left: 4px solid var(--border-color);
        }

        .quiz-form .question-block, .result-detail .question-block { 
            margin-bottom: 30px; 
            opacity: 0;
            transform: translateY(20px);
            animation: fadeInUP 0.5s ease-out forwards;
        }
        .quiz-form .options label { 
            display: block; margin: 8px 0; padding: 10px; 
            border: 1px solid var(--border-color); border-radius: 6px; 
            cursor: pointer; transition: background-color 0.2s;
        }
        .quiz-form .options label:hover { background-color: var(--hover-bg); }
        .quiz-form .options input { margin-right: 10px; }
        
        .fill-in-blank-input { 
            padding: 10px; font-size: 1em; width: 95%; 
            border: 1px solid var(--border-color); border-radius: 4px;
            background-color: var(--container-bg); color: var(--text-color);
            margin-top: 10px;
        }

        /* Styles for "order" question tokens */
        .tokens-container {
            padding: 10px;
            background: var(--list-item-bg);
            border: 1px dashed var(--border-color);
            border-radius: 6px;
            margin: 10px 0;
            display: flex;
            flex-wrap: wrap;
            gap: 8px;
        }
        .token-drag {
            background: var(--hover-bg);
            padding: 5px 10px;
            border-radius: 4px;
            font-family: monospace;
            user-select: none;
        }

        .quiz-submit-btn { 
            background-color: #007bff; color: white; padding: 12px 25px; 
            border: none; border-radius: 6px; cursor: pointer; font-size: 1.1em;
            transition: transform 0.2s, box-shadow 0.2s;
        }
        .quiz-submit-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 10px rgba(0, 123, 255, 0.3);
        }
        
        /* --- HIỆU ỨNG CHUYỂN TRANG --- */
        .page-content { 
            display: none;
            opacity: 0;
            transition: opacity 0.4s ease-in-out;
        }
        .page-content.active {
            display: block;
            opacity: 1;
        }
        
        /* --- HIỆU ỨNG TẢI TRANG (PRELOADER) --- */
        #preloader {
            position: fixed; top: 0; left: 0;
            width: 100%; height: 100%;
            background-color: var(--bg-color);
            z-index: 9999;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: opacity 0.5s ease;
        }
        #preloader.hidden {
            opacity: 0;
            pointer-events: none;
        }
        .spinner {
            border: 4px solid var(--hover-bg);
            border-top: 4px solid var(--link-color);
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        /* --- KEYFRAME CHO HIỆU ỨNG CÂU HỎI --- */
        @keyframes fadeInUP {
            from {
                opacity: 0;
                transform: translateY(20px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        /* --- Result Styles --- */
        .result-summary { text-align: center; padding: 20px; background: var(--list-item-bg); border-radius: 8px; margin-bottom: 20px; }
        .result-summary h2 { font-size: 2.5em; color: var(--link-color); margin-bottom: 5px; }
        .result-summary p { margin-top: 5px; font-size: 1.1em; color: var(--text-color); }
        .result-detail .question-block { padding: 15px; border-radius: 6px; }
        .result-detail .correct { border-left: 5px solid var(--correct-border); background-color: var(--correct-bg); }
        .result-detail .incorrect { border-left: 5px solid var(--incorrect-border); background-color: var(--incorrect-bg); }
        .result-detail .user-answer, .result-detail .correct-answer-text { font-weight: bold; }
        
        /* Box for the correct answer text */
        .correct-answer-text-box {
            background: var(--correct-bg);
            border: 2px solid var(--correct-border);
            color: var(--correct-text);
            padding: 8px 12px;
            border-radius: 6px;
            display: inline-block;
            margin-top: 8px;
            font-weight: bold;
        }
        .result-detail .correct .correct-answer-text-box {
             background: var(--correct-bg);
             color: var(--correct-text);
             border-color: var(--correct-border);
        }
        .result-detail .incorrect .correct-answer-text-box {
            /* In dark mode, make the correct answer box stand out even on a red background */
            background: #d4edda; /* Always light green */
            color: #155724; /* Always dark green */
            border-color: #28a745;
        }


        .result-detail .correct, .result-detail .correct p, .result-detail .correct b, .result-detail .correct i { color: var(--correct-text); }
        .result-detail .incorrect, .result-detail .incorrect p, .result-detail .incorrect b, .result-detail .incorrect i { color: var(--incorrect-text); }

        /* --- Nút Chuyển Giao Diện --- */
        .theme-switcher {
            position: absolute; top: 20px; right: 20px; cursor: pointer;
            background-color: var(--hover-bg); border: 1px solid var(--border-color);
            border-radius: 50%; width: 40px; height: 40px;
            display: flex; align-items: center; justify-content: center;
            font-size: 20px; user-select: none; z-index: 1001;
            transition: transform 0.2s;
        }
        .theme-switcher:hover { transform: scale(1.1); }
        .theme-switcher .icon { display: none; }

        /* --- Giao diện Tối --- */
        body.dark-mode {
            --bg-color: #121212;
            --container-bg: #1e1e1e;
            --text-color: #e0e0e0;
            --link-color: #8ab4f8;
            --border-color: #444;
            --hover-bg: #2a2a2a;
            --list-item-bg: #252525;
            --correct-bg: #2a3b2e;
            --correct-text: #a5d6a7;
            --incorrect-bg: #402c2f;
            --incorrect-text: #ef9a9a;
            --passage-text: #aaa;
        }
        body.dark-mode .theme-switcher .sun-icon { display: block; }
        body:not(.dark-mode) .theme-switcher .moon-icon { display: block; }
        
        /* --- HIỆU ỨNG PHÁO HOA VÀ MODAL --- */
        .fireworks-container {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            z-index: 1000; background: rgba(0, 0, 0, 0.5); display: none;
        }
        .celebration-modal {
            position: fixed; top: 50%; left: 50%;
            transform: translate(-50%, -50%) scale(0.9);
            background-color: var(--container-bg); padding: 30px 40px; border-radius: 12px;
            text-align: center; box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            z-index: 1001; opacity: 0; transition: opacity 0.3s, transform 0.3s;
        }
        .fireworks-container.active .celebration-modal {
            opacity: 1; transform: translate(-50%, -50%) scale(1);
        }
        .celebration-modal h2 { font-size: 2em; margin-bottom: 10px; }
        .celebration-modal p { font-size: 3em; font-weight: bold; color: var(--link-color); margin: 10px 0; }
        .celebration-modal button {
            background-color: #007bff; color: white; padding: 10px 30px; 
            border: none; border-radius: 6px; cursor: pointer; font-size: 1.1em; margin-top: 20px;
            transition: transform 0.2s, box-shadow 0.2s;
        }
        .celebration-modal button:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 10px rgba(0, 123, 255, 0.3);
        }
    </style>
</head>
<body>

<div id="preloader">
    <div class="spinner"></div>
</div>

<div class="container">
    <div class="theme-switcher" id="themeToggle" title="Chuyển đổi giao diện Sáng/Tối">
        <span class="icon sun-icon">☀️</span>
        <span class="icon moon-icon">🌙</span>
    </div>

    <div id="page-home" class="page-content">
        <h1>📚 Danh Sách Bài Tập 📚</h1>
        <ul style="list-style: none; padding: 0;" id="exercise-list">
            </ul>
    </div>

    <div id="page-quiz" class="page-content">
        <a class="back-link" href="#home">← Quay lại danh sách</a>
        <h1 id="quiz-title">📝 Đang tải bài tập...</h1>
        <form class="quiz-form" id="quizForm">
            </form>
    </div>

    <div id="page-results" class="page-content">
        </div>
</div>

<div class="fireworks-container" id="fireworksContainer">
    <div class="celebration-modal">
        <h2>Chúc mừng!</h2>
        <p id="finalScore"></p>
        <button id="okButton">OK</button>
    </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/fireworks-js@2.10.7/dist/index.umd.js"></script>

<script>
    // =====================================================================
    // DỮ LIỆU BÀI TẬP VÀ ĐÁP ÁN
    // =====================================================================
    const quizzes = {
"BÀI 1":[
    { type:"mc", question:"1. Để mở một tập tin, bạn nên nhấp đúp vào biểu tượng của nó.", answers:{a:"To delete a file, you should double-click its icon.", b:"To open a file, you should double-click its icon.", c:"To close a file, you should right-click its icon."}, correctAnswer:"b" },
    { type:"mc", question:"2. Tớ không thể kết nối Wi-Fi từ biểu tượng mạng.", answers:{a:"I can’t download files from the folder icon.", b:"I can’t connect to the printer from the recycle bin.", c:"I can’t connect to the Wi-Fi from the network icon."}, correctAnswer:"c" },
    { type:"mc", question:"3. Nếu bạn muốn phóng to cửa sổ, hãy nhấn nút maximize.", answers:{a:"If you want to minimize a window, press the maximize button.", b:"If you want to close a window, press the icon.", c:"If you want to maximize a window, press the maximize button."}, correctAnswer:"c" },
    { type:"mc", question:"4. Tớ cần thêm RAM để máy tính chạy nhanh hơn.", answers:{a:"I need more webcam to make my computer clearer.", b:"I need more hard drive to make my computer slower.", c:"I need more RAM to make my computer faster."}, correctAnswer:"c" },
    { type:"mc", question:"5. Tớ không thể mở ứng dụng từ biểu tượng trên màn hình.", answers:{a:"I can’t delete any apps from the desktop folder.", b:"I can’t open the app from the icon on the desktop.", c:"I can’t open any external hardware drives in the taskbar."}, correctAnswer:"b" },
    { type:"mc", question:"6. A: I'm not sure why the installation failed. B: .......... your antivirus software and try again. It might block the setup.", answers:{a:"To turn off", b:"Turns off", c:"Turn off"}, correctAnswer:"c" },
    { type:"mc", question:"7. A: The icons on my desktop are too small to read. B: ........... the screen resolution in display settings to make them larger.", answers:{a:"Adjusts", b:"To adjust", c:"Adjust"}, correctAnswer:"c" },
    { type:"mc", question:"8. .......... the icon to open the application.", answers:{a:"Double-click", b:"Double-clicked", c:"Double-clicking"}, correctAnswer:"a" },
    { type:"mc", question:"9. …………… on the \"Start\" button to access the system settings menu.", answers:{a:"Click", b:"Clicks", c:"Clicked"}, correctAnswer:"a" },
    { type:"mc", question:"10. ........ the minimum system requirements before installing the new graphic software.", answers:{a:"Checking", b:"Check", c:"Checks"}, correctAnswer:"b" },

    { type:"mc", question:"11. What does a computer need to run multiple tasks at a time smoothly?", answers:{a:"enough RAM", b:"the system performance", c:"RAM"}, correctAnswer:"a", passage:"RAM (Random Access Memory) is very important for the performance of a computer system. The more RAM a computer has, the more smoothly it can run multiple programs at the same time. For example, if you are running a web browser, a music player, and a game at the same time, your computer needs enough RAM to handle all these tasks. If the RAM is too low, the system may slow down or even freeze."},
    { type:"mc", question:"12. What is one main purpose of a GUI?", answers:{a:"To help users interact with the computer more easily", b:"To increase processor speed", c:"To install more storage space"}, correctAnswer:"a", passage:"A Graphical User Interface (GUI) uses icons, buttons, and windows that allow users to interact with the computer easily. Unlike a Command Line Interface, a GUI is more user-friendly because users can click and drag items with a mouse. Most operating systems like Windows, macOS, and Linux have GUIs. It helps beginners navigate and operate software without typing commands."},
    { type:"mc", question:"13. Why is it important to check the system requirements before installing software?", answers:{a:"To make sure the software is free", b:"To ensure the software works properly on the computer", c:"To choose the best brand of computer"}, correctAnswer:"b", passage:"Before installing software, users must check the system requirements. These include the operating system, processor speed, RAM, and storage space. If the computer does not meet the minimum requirements, the software may not run properly. Some programs also need a specific type of graphics card or internet connection. Always read the specifications carefully before downloading."},
    { type:"mc", question:"14. What is the effect of a higher screen resolution?", answers:{a:"It uses fewer pixels.", b:"It gives good quality images and more detail.", c:"It makes the screen smaller."}, correctAnswer:"b", passage:"Screen resolution refers to the number of pixels on a screen. A higher resolution means sharper images and more detail. Common resolutions are 1366x768 or 1920x1080. When choosing a monitor or adjusting display settings, it is important to consider resolution. A low-resolution screen may make text look shadowy or images unclear. This affects both comfort and productivity, especially during long use."},
    { type:"mc", question:"15. Which is not an input device?", answers:{a:"tables", b:"touchscreens", c:"keyboards"}, correctAnswer:"a", passage:"Input devices like a mouse and keyboard are essential for using a Graphical User Interface (GUI). A mouse helps move the pointer and select icons. A keyboard is used to enter text and shortcuts. Touchscreens, found in tablets and smartphones, are also input devices that support GUI operations. These tools make it easier for users to work with software."},

    { 
      type:"fill", 
      question:"Summary: A GUI uses icons and windows, letting users interact easily by clicking and dragging. It’s **...........** than command-lines, helping beginners use software without typing commands.", 
      answer:"more user-friendly", 
      passage:`A Graphical User Interface (GUI) uses icons, buttons, and windows that allow users to interact with the computer easily. Unlike a Command Line Interface, a GUI is more user-friendly because users can click and drag items with a mouse. Most operating systems like Windows, macOS, and Linux have GUIs. It helps beginners navigate and operate software without typing commands.`
    },
    { 
      type:"fill", 
      question:"Summary: Check system specifications like processor, RAM, storage, OS, and graphics card when buying a computer. These help **...........** and choose the right device for tasks like gaming or video editing.", 
      answer:"the computer run smoothly", 
      passage:`When buying a computer, it’s important to check system specifications such as processor type, RAM size, storage capacity, operating system, and graphics card. A fast processor and sufficient RAM help the computer run smoothly. A strong graphics card is essential for gaming or video editing. Understanding these specifications helps avoid future problems like slow performance or software issues.`
    },
    { 
      type:"fill", 
      question:"Summary: A GUI lets users interact with computers easily by clicking icons and menus, making it **.................** for users. It helps people work efficiently without needing to learn commands.", 
      answer:"faster and more comfortable", 
      passage:`A GUI, or Graphical User Interface, makes it easy for users to interact with computers. Instead of typing commands, users can click icons, select items from menus, and drag windows. This visual method is faster and more comfortable for most people, especially beginners. Popular operating systems like Windows and macOS use GUIs to help users access files and applications easily. With a good GUI, users spend less time learning commands and more time working productively.`
    },

    { type:"mc", question:"19. The system specifications are only important for playing games. (True/False)", answers:{a:"True", b:"False", c:"Not sure"}, correctAnswer:"b", passage:"When buying a computer, it is important to check the system specifications. These include the processor speed, RAM size, hard drive capacity, and operating system. A faster processor helps the computer run programs quickly. More RAM allows for smoother multitasking. The operating system must support the software you plan to use. If you want to play games or edit videos, you’ll need a more powerful system."},
    { type:"mc", question:"20. GUIs are harder to use than command-line interfaces. (True/False)", answers:{a:"True", b:"False", c:"Depends"}, correctAnswer:"b", passage:"A Graphical User Interface (GUI) allows users to interact with a computer using images, icons, and windows. It is easier for beginners than a command-line interface, which requires typing commands. Most modern operating systems like Windows and macOS use a GUI."},

    { type:"fill", question:"21. A: The window is too big and covers the screen. B: _______ the side to make room. (move)", answer:"Move it to"},
    { type:"fill", question:"22. A: I'm going to close the file. B: Wait! _______ first. (save)", answer:"Save it"},
    { type:"fill", question:"23. A: This screen looks strange on my monitor. B: _______ display resolution in the settings. (adjust)", answer:"Adjust the"},
    { type:"fill", question:"24. A: I’m not sure if this program will run on my laptop. B: _______ system requirements first. (check)", answer:"Check the"},
    { type:"fill", question:"25. A: I want to open the settings, but I don’t know how to do. B: _______ icon on the taskbar. (click)", answer:"Click on the gear"},

    { type:"order", question:"26. Sắp xếp để thành câu hoàn chỉnh:", correctSentence:"Always check the system specifications carefully before buying a new computer.", tokens:["Always","check","the system","specifications","carefully before","buying","a new","computer."] },
    { type:"order", question:"27. Sắp xếp để thành câu hoàn chỉnh:", correctSentence:"To open the file, simply click on the icon in the Graphical User Interface (GUI).", tokens:["To","open","the file, simply","click on","the icon","in the","Graphical","User","Interface (GUI)."] },
    { type:"order", question:"28. Sắp xếp để thành câu hoàn chỉnh:", correctSentence:"Carefully select software that matches your system specifications before installing it.", tokens:["Carefully","select","software","that matches","your system","specifications","before installing","it."] },
    { type:"order", question:"29. Sắp xếp để thành câu hoàn chỉnh:", correctSentence:"Keep your system and software updated regularly for better performance.", tokens:["Keep","your system","and software","updated regularly","for better","performance."] },
    { type:"order", question:"30. Sắp xếp để thành câu hoàn chỉnh:", correctSentence:"Do not install programs if your system doesn't meet the requirements.", tokens:["Do not","install","programs","if your","system doesn't","meet","the requirements."] }
],
"BÀI 2":[
    { type:"mc", question:"1. Trình duyệt của tôi lưu mật khẩu để đăng nhập nhanh hơn.",
    answers:{a:"My browser hides passwords to protect data.",
             b:"My browser deletes passwords to log in faster.",
             c:"My browser saves passwords for quicker logins."},
    correctAnswer:"c" },
    { type:"mc", question:"2. Máy tính của tôi tự động cài đặt bản cập nhật khi khởi động lại.",
    answers:{a:"My computer automatically installs updates when shutting down.",
             b:"My computer automatically installs updates when restarting.",
             c:"My computer manually installs updates when restarting."},
    correctAnswer:"b" },
    { type:"mc", question:"3. Trình duyệt web này hỗ trợ nhiều thẻ cùng lúc.",
    answers:{a:"This web browser deletes multiple tabs at once.",
             b:"This web browser downloads multiple files at once.",
             c:"This web browser supports multiple tabs at once."},
    correctAnswer:"c" },
    { type:"mc", question:"4. Hệ điều hành Windows 10 đã phát hành năm 2015.",
    answers:{a:"The Windows 10 operating system will release in 2015.",
             b:"The Windows 10 operating system was released in 2015.",
             c:"The Windows 10 browser was updated in 2015."},
    correctAnswer:"b" },
    { type:"mc", question:"5. Tôi cần đánh dấu trang web này để đọc sau.",
    answers:{a:"I need to sync this website to read later.",
             b:"I need to refresh this website to read later.",
             c:"I need to bookmark this website to read later."},
    correctAnswer:"c" },
    { type:"mc", question:"6. Many people ............... private Browse when they visit websites on public computers.",
    answers:{a:"use", b:"is using", c:"are use"}, correctAnswer:"a" },
    { type:"mc", question:"7. She ............... her laptop to the internet through a mobile hotspot at the moment.",
    answers:{a:"connect", b:"is connecting", c:"connects"}, correctAnswer:"b" },
    { type:"mc", question:"8. Right now, the system ............... for updates to install the latest version.",
    answers:{a:"is checking", b:"checks", c:"checking"}, correctAnswer:"a" },
    { type:"mc", question:"9. My computer ................ slowly because too many programs are running in the background.",
    answers:{a:"is running", b:"running", c:"works"}, correctAnswer:"a" },
    { type:"mc", question:"10. The user usually .......... the browser before checking emails or visiting websites.",
    answers:{a:"is opening", b:"opens", c:"open"}, correctAnswer:"b" },
    { type:"mc", 
    question:"11. Why are updates important?", 
    passage:`Updates to operating systems and web browsers fix bugs, improve performance, and strengthen security. These regular updates may also include useful new features or tools and help protect your device from malware, viruses, or hackers. If you ignore updates, your system might become slow, outdated, or unsafe. Most modern devices allow automatic updates to save time and effort.`,
    answers:{a:"They make the device slower",
             b:"They fix bugs and improve security.",
             c:"They delete your files"},
    correctAnswer:"b" },
    { type:"mc", 
    question:"12. What is one purpose of cookies?", 
    passage:`Cookies are small files that websites store on your computer to collect and remember information. They help websites remember user settings, login details, and shopping cart items. While cookies can be helpful for a smoother browsing experience, having too many may slow down your browser. You can delete cookies anytime from your browser's settings menu to improve speed and performance.`,
    answers:{a:"To upgrade the browser", b:"To store user settings", c:"To increase download speed. lưu ý kiểm tra lại là C mới đúng"},
    correctAnswer:"b" },
    { type:"mc", 
    question:"13. Why is multitasking useful?", 
    passage:`An operating system that supports multitasking lets you run several programs at the same time. For example, you can listen to music, write a document, and browse the internet without closing any of them. Without multitasking, only one application could run at a time, which slows down productivity.`,
    answers:{a:"It increases screen brightness.", b:"It allows many programs to run at once.", c:"It makes the computer smaller."},
    correctAnswer:"b" },
    { type:"mc", 
    question:"14. What does private browsing do?", 
    passage:`Internet browsing is done through applications called web browsers. These include Chrome, Firefox, Safari, and Edge. Browsers allow users to visit websites, watch videos, and download files. They also save passwords and browsing history if allowed. To protect privacy, users can enable private browsing, which does not store history or cookies.`,
    answers:{a:"It stores passwords", b:"It does not store history or cookies", c:"It saves browsing history"},
    correctAnswer:"b" },
    { type:"mc", 
    question:"15. What does an operating system help manage?", 
    passage:`Operating systems control the basic functions of a computer. They manage hardware like the CPU, memory, and storage devices. Without an operating system, a computer cannot run programs or communicate with connected devices. Windows, macOS, and Linux are examples of operating systems used on personal computers and laptops.`,
    answers:{a:"Hardware and basic functions.", b:"Only software.", c:"Only the display screen."},
    correctAnswer:"a" },
    { type:"mc", 
    question:"16. A computer can work properly without an operating system. (True/False)", 
    passage:`Operating systems provide a platform for running software and managing hardware devices like printers, keyboards, and storage drives. Without an operating system, most computers cannot function. Operating systems also offer tools for file management and security. Users often choose an OS based on personal needs and the type of software they plan to use.`,
    answers:{a:"True", b:"False"}, correctAnswer:"b" },
    { type:"mc", 
    question:"17. Built-in security features in modern operating systems protect them against online threats. (True/False)", 
    passage:`Most modern operating systems come with built-in security features such as firewalls and automatic updates. These tools help protect the system from viruses, malware, and other online threats. Users should keep these features enabled and regularly update their system to ensure the highest level of protection.`,
    answers:{a:"True", b:"False"}, correctAnswer:"a" },
    { type:"fill", 
    question:"18. Summary: A search engine finds websites based on keywords and is an important tool for ........................ and online research.", 
    passage:`A search engine helps users find information online by showing a list of websites that match the keywords entered. Google, Bing, and Yahoo are some of the most popular search engines. Using clear and specific keywords gives better results. Search engines are essential tools for internet browsing and learning, making it easier and faster to access knowledge anytime.`,
    answer:"internet browsing" },
    { type:"fill", 
    question:"19. Summary: Updating the operating system helps improve security, fixes bugs, updates new features and makes the system work ..............", 
    passage:`An operating system update improves the security and performance of a computer. Updates may include bug fixes, new features, or protection against the latest viruses and threats. Keeping your OS up to date ensures the system runs efficiently and safely. Most systems can be set to update automatically in the background.`,
    answer:"efficiently and safely" },
    { type:"fill", 
    question:"20. Summary: Private browsing prevents the browser from saving history or cookies, helping users protect their ........... on shared devices.", 
    passage:`Most internet browsers offer a feature called private browsing or incognito mode. When using this mode, the browser does not save your history, cookies, or login details. This is useful when using a shared computer or when users want to keep their browsing activity private. However, private browsing does not make users completely anonymous online.`,
    answer:"browsing activity" },
    { type:"fill", question:"21. The app runs in the background ......................... manage battery usage. (help)", answer:"to help" },
    { type:"fill", question:"22. You should restart the computer .................... the installation. (complete)", answer:"in order to complete" },
    { type:"fill", question:"23. He is clearing the browser cache ............... faster. (website/load)", answer:"so that websites load" },
    { type:"fill", question:"24. The technician is installing a new firewall ........ a system. (protect)", answer:"to protect" },
    { type:"fill", question:"25. The user updates the browser regularly ........ safe online. (stay)", answer:"to stay" },
    { type:"order", 
    question:"26. Arrange the words to make a correct sentence.", 
    correctSentence:"The user is clicking on the link to open the settings page.", 
    tokens:["The user","is","clicking","on the","link","to","open","the settings page."] },
    { type:"order", 
    question:"27. Arrange the words to make a correct sentence.", 
    correctSentence:"The browser is loading several tabs with video content right now.", 
    tokens:["The browser","is loading","several tabs","with video","content","right now."] },
    { type:"order", 
    question:"28. Arrange the words to make a correct sentence.", 
    correctSentence:"Browsers store data so that websites load faster during future visits.", 
    tokens:["Browsers store","data so that","websites load","faster during","future visits."] },
    { type:"order", 
    question:"29. Arrange the words to make a correct sentence.", 
    correctSentence:"The system usually installs updates automatically when a new version is available.", 
    tokens:["The system","usually","installs updates automatically","when","a new version","is available."] },
    { type:"order", 
    question:"30. Arrange the words to make a correct sentence.", 
    correctSentence:"Most computers use operating systems to support multiple users and applications.", 
    tokens:["Most computers","use operating","systems to","support multiple","users and applications."] },
],
"BÀI 3":[
    { type:"mc", question:"1. Máy tính bảng của tôi không thể kết nối với mạng Bluetooth.",
    answers:{a:"My tablet cannot connect to the Bluetooth network.", b:"My laptop connects to Wi-Fi faster than it does to Ethernet cables.", c:"My phone doesn't need Wi-Fi because it uses Bluetooth for everything."},
    correctAnswer:"a" },
    { type:"mc", question:"2. Máy tính xách tay của anh ấy mất nhiều thời gian để kết nối với mạng không dây.",
    answers:{a:"His tablet instantly connects to the internet via mobile data.", b:"His desktop doesn't support any kind of network connection.", c:"His laptop takes longer to connect to the wireless network."},
    correctAnswer:"c" },
    { type:"mc", question:"3. Bạn nên sử dụng dữ liệu di động nếu không có Wi-Fi.",
    answers:{a:"You should use your mobile data when there is no available Wi-Fi.", b:"You might need to charge the phone before using mobile data again.", c:"You must switch off mobile data whenever you see a public Wi-Fi."},
    correctAnswer:"a" },
    { type:"mc", question:"4. Mạng Wi-Fi ở đây rất yếu và hay bị mất kết nối.",
    answers:{a:"The Wi-Fi connection in this place is weak and often gets disconnected.", b:"The mobile data is stronger than the Wi-Fi in this building.", c:"The Wi-Fi network here is extremely fast and never gets disconnected."},
    correctAnswer:"a" },
    { type:"mc", question:"5. Tớ không thể gửi tin nhắn vì không có tín hiệu di động.",
    answers:{a:"I can't send any text messages because there is no mobile signal.", b:"I can't open mobile apps because my phone has no storage left.", c:"I can't read the messages because my phone battery is fully charged."},
    correctAnswer:"a" },
    { type:"mc", question:"6. This is the application .............. allows users to share files over mobile networks.",
    answers:{a:"who", b:"where", c:"which"},
    correctAnswer:"c" },
    { type:"mc", question:"7. I know an engineer .............. designs network systems for mobile service providers.",
    answers:{a:"where", b:"who", c:"which"},
    correctAnswer:"b" },
    { type:"mc", question:"8. We installed a system .............. connects all smartphones to a secured local network.",
    answers:{a:"that", b:"where", c:"who"},
    correctAnswer:"a" },
    { type:"mc", question:"9. The technician ……………… helped us fix the internet issue was very professional.",
    answers:{a:"which", b:"where", c:"who"},
    correctAnswer:"c" },
    { type:"mc", question:"10. What can affect Wi-Fi signal strength?",
    passage:`Wi-Fi is a wireless network technology that connects devices to the internet without cables. Most homes and offices have a Wi-Fi router that sends signals to smartphones, laptops, and other devices. The signal strength depends on the distance from the router and walls or obstacles. A weak signal can cause slow connections or lost internet access.`,
    answers:{a:"The number of apps installed.", b:"The size of the hard drive.", c:"The distance from the router and obstacles."},
    correctAnswer:"c" },
    { type:"mc", question:"11. What is one disadvantage of using a mobile hotspot?",
    passage:`A hotspot is a physical location where people can access the internet through Wi-Fi. Some smartphones can also create personal hotspots by sharing mobile data. Hotspots are useful when Wi-Fi is not available. However, using mobile data as a hotspot can use a lot of data quickly and drain battery power fast.`,
    answers:{a:"It improves battery life.", b:"It cannot connect other devices.", c:"It uses a lot of data and battery."},
    correctAnswer:"c" },
    { type:"mc", question:"12. Why is mobile computing useful?",
    passage:`Mobile computing allows people to use computing devices while moving. Smartphones, tablets, and laptops are examples of mobile devices. These devices connect to networks using Wi-Fi, mobile data (3G/4G/5G), or Bluetooth. Mobile computing helps users send emails, attend online meetings, or browse the internet from almost anywhere. This makes work and communication more flexible.`,
    answers:{a:"It only works with desktop computers.", b:"It limits users to work in one place.", c:"It allows communication from almost anywhere."},
    correctAnswer:"c" },
    { type:"mc", question:"13. What type of network is not commonly used in homes and public places?",
    passage:`A computer network is a group of connected devices that can communicate with each other. These devices include computers, printers, phones, and servers. Networks can be wired, using cables, or wireless, using radio signals. Wireless networks like Wi-Fi are common in homes and public places. Networks allow users to share files, access the internet, and use printers or servers from different devices.`,
    answers:{a:"Satellite network", b:"Wired network", c:"Wireless network like Wi-Fi"},
    correctAnswer:"b" },
    { type:"mc", question:"14. What does Bluetooth help with?",
    passage:`Bluetooth is a wireless technology that allows short-range communication between devices. You can use Bluetooth to connect headphones, speakers, or share files between phones. Unlike Wi-Fi, Bluetooth works over shorter distances and uses less power. It is ideal for simple tasks like playing music or transferring small files without needing internet access.`,
    answers:{a:"Connecting nearby devices wirelessly", b:"High-speed internet connections", c:"Sharing large files online"},
    correctAnswer:"a" },
    { type:"mc", question:"15. What does an operating system help manage?",
    passage:`Operating systems control the basic functions of a computer. They manage hardware like the CPU, memory, and storage devices. Without an operating system, a computer cannot run programs or communicate with connected devices. Windows, macOS, and Linux are examples of operating systems used on personal computers and laptops.`,
    answers:{a:"Only software", b:"Hardware and basic functions", c:"Only the display screen"},
    correctAnswer:"b" },
    { type:"mc", question:"16. Wireless networks always provide strong signal strength, even if the distance is near or far. (True/False)",
    passage:`A wireless network allows devices to connect to the internet without using cables. It uses radio waves to send signals between devices and a router. Wi-Fi is the most common type of wireless network found in homes, schools, and offices. Wireless networks offer flexibility and convenience, but signal strength can drop if there are walls or distance between the device and router.`,
    answers:{a:"True", b:"False", c:"Not sure"},
    correctAnswer:"b" },
    { type:"mc", question:"17. Mobile data is unlimited for all users by default. (True/False)",
    passage:`Mobile data allows users to connect to the internet without Wi-Fi by using a cellular network. It is commonly used on smartphones and tablets. People use mobile data to browse websites, watch videos, and send emails on the go. However, streaming or downloading large files can use up data quickly, especially if the user has a limited data plan.`,
    answers:{a:"True", b:"False", c:"Not sure"},
    correctAnswer:"b" },
    { type:"fill", question:"18. Summary: Networks allow devices to share data and access the internet. They can ...................., depending on the connection type.",
    passage:`A computer network is a group of devices connected together to share data, files, and various resources. These devices include computers, printers, smartphones, tablets, and servers. Networks can be either wired, using physical cables, or wireless, using radio signals such as Wi-Fi. They are essential for communication, collaboration, internet access, and smooth operation within homes, schools, and workplaces.`,
    answer:"be wired or wireless" },
    { type:"fill", question:"19. Summary: Wi-Fi connects devices without cables and is widely used in homes and businesses. ...................... depends on distance and obstacles.",
    passage:`Wi-Fi is a wireless networking technology that uses radio signals to connect devices such as laptops, smartphones, and tablets to the internet. Most homes, offices, schools, and public places use Wi-Fi routers to provide internet access. However, signal strength can become weaker in areas far from the router or when blocked by thick walls, furniture, or electronic devices.`,
    answer:"Signal strength" },
    { type:"fill", question:"20. Summary: Mobile computing uses wireless technologies to connect devices and allows users to work from .................",
    passage:`Mobile computing allows people to use computers and access important data while on the move. Devices such as laptops, smartphones, and tablets connect to networks through Wi-Fi, mobile data, or Bluetooth. This flexibility gives users the freedom to work, study, browse the internet, or communicate with others from nearly any location, whether at home, outdoors, or while traveling.`,
    answer:"nearly any location" },
    { type:"fill", question:"21. The technician ................................ fixed the Wi-Fi in less than an hour. (come/yesterday)",
    answer:"who came yesterday" },
    { type:"fill", question:"22. The student .................... how computer networks work spoke very clearly and confidently. (explain)",
    answer:"who explained" },
    { type:"fill", question:"23. The laptop .................... strong wireless adapter connects to the network easily. (have)",
    answer:["which has a","that has a"] },
    { type:"fill", question:"24. The new modem.............  provides a faster internet connection. (install/you)",
    answer:["which you installed","that you installed"] },
    { type:"fill", question:"25. The app........... mobile data is useful when traveling abroad. (monitor)",
    answer:"which monitors" },
    { type:"order", question:"26. Sắp xếp để thành câu hoàn chỉnh:", 
    correctSentence:"I need to replace the old modem that keeps losing the connection.", 
    tokens:["I need","to replace","the old","modem that","keeps losing","the connection."] },
    { type:"order", question:"27. Sắp xếp để thành câu hoàn chỉnh:", 
    correctSentence:"Devices that are connected to a network can share files and printers.", 
    tokens:["Devices","that are","connected to","a network","can share files","and printers."] },
    { type:"order", question:"28. Sắp xếp để thành câu hoàn chỉnh:", 
    correctSentence:"The smartphone which you paired with Bluetooth speakers is working very well.", 
    tokens:["The smartphone","which","you paired","with Bluetooth","speakers is","working","very well."] },
    { type:"order", question:"29. Sắp xếp để thành câu hoàn chỉnh:", 
    correctSentence:"A wireless hotspot is a location that provides internet access without using cables.", 
    tokens:["A wireless","hotspot","is a location","that provides","internet access","without using cables."] },
    { type:"order", question:"30. Sắp xếp để thành câu hoàn chỉnh:", 
    correctSentence:"The technician who fixed the router also updated the firewall settings today.", 
    tokens:["The technician","who","fixed the router","also updated","the firewall","settings today."] }
],
"BÀI 4":[
    { type:"mc", question:"1. Hệ quản trị cơ sở dữ liệu giúp bạn lưu trữ và quản lý dữ liệu dễ dàng.",
    answers:{a:"The database management system helps you store and manage data easily.", b:"The primary key helps you store and manage data easily.", c:"The database form helps you store and manage data easily."},
    correctAnswer:"a" },
    { type:"mc", question:"2. Một báo cáo có thể hiển thị dữ liệu từ nhiều bảng cùng lúc.",
    answers:{a:"A query can display data from many tables at the same time.", b:"A record can display data from many tables at the same time.", c:"A report can display data from many tables at the same time."},
    correctAnswer:"c" },
    { type:"mc", question:"3. Mỗi bản ghi chứa nhiều trường thông tin khác nhau.",
    answers:{a:"Each report contains many different tables.", b:"Each field contains many different records.", c:"Each record contains many different fields."},
    correctAnswer:"c" },
    { type:"mc", question:"4. Dữ liệu có thể được tìm kiếm nhanh chóng bằng cách sử dụng chỉ mục.",
    answers:{a:"Data can be recorded quickly by using an index.", b:"Data can be retrieved quickly by using an index.", c:"Data can be deleted quickly by using a field."},
    correctAnswer:"b" },
    { type:"mc", 
    question:"5. A: How can we make the data easier to understand and work with? B: We can do that ................... it into tables.", 
    answers:{a:"putting", b:"by putting", c:"by puting"}, 
    correctAnswer:"b" },
    { type:"mc", question:"6. How does the system find a record so quickly? It finds it ................... the primary key.",
    answers:{a:"for using", b:"to using", c:"by using"},
    correctAnswer:"c" },
    { type:"mc", 
    question:"7. A: What's the best way to get accurate results from the database? B: ... correct fields and table, of course.", 
    answers:{a:"By selecting a", b:"To select a", c:"By selecting the"}, 
    correctAnswer:"c" },
    { type:"mc", question:"8. They improved report accuracy ____________ a unique record for each object.",
    answers:{a:"by creating", b:"in creating", c:"by create"},
    correctAnswer:"a" },
    { type:"mc", question:"9. What is one advantage of a relational database?",
    passage:`Many companies use relational databases because they help connect information across tables. For example, an employee table can link to a department table using a shared ID number. This makes it faster to find related records and reduces data entry mistakes. Relational databases are more flexible than simple flat files. They also allow businesses to manage complex data relationships more efficiently and securely.`,
    answers:{a:"It requires less memory.", b:"It only stores names and numbers.", c:"It is used to connect information across tables."},
    correctAnswer:"c" },
    { type:"mc", question:"10. How are relational databases organized?",
    passage:`Relational databases are the most common type of database used today. They store data in tables that are linked by relationships. For example, a customer table can be connected to an orders table through a customer ID. This structure makes it easy to find related information and reduce duplication. Popular relational database systems include MySQL, PostgreSQL, and Oracle Database.`,
    answers:{a:"They use tables connected by relationships.", b:"They store data in a single list.", c:"They arrange data randomly."},
    correctAnswer:"a" },
    { type:"mc", question:"11. What is one role of a database administrator?",
    passage:`Database administrators, or DBAs, are responsible for maintaining databases. They make sure the data is secure, backed up, and available to users when needed. DBAs also monitor performance and fix any problems. Without their work, companies could lose important data or experience slow systems. In addition, DBAs help plan for future growth and ensure the system meets business needs.`,
    answers:{a:"Designing video games", b:"Selling databases to customers", c:"Keeping data secure and available"},
    correctAnswer:"c" },
    { type:"mc", question:"12. What is one function of a DBMS?",
    passage:`A database management system (DBMS) is software that helps users create, maintain, and control databases. It provides tools for adding new records, updating existing data, and generating reports. Security features in a DBMS protect sensitive information by controlling user access. Some DBMS programs are open-source, while others require a license. Choosing the right DBMS depends on the size of the data and the organization's needs.`,
    answers:{a:"Designing websites", b:"Printing documents automatically", c:"Controlling access to sensitive data"},
    correctAnswer:"c" },
    { type:"mc", question:"13. What is one advantage of a database over a spreadsheet?",
    passage:`A database is an organized collection of information that can be easily accessed, managed, and updated. Businesses use databases to store customer records, inventory data, and sales transactions. Unlike spreadsheets, databases can handle large amounts of information and support multiple users at the same time. Most databases use a language called SQL, which allows users to create queries to retrieve specific data quickly and accurately.`,
    answers:{a:"It requires no special language to operate.", b:"It can manage large data and support many users.", c:"It is easier to print data."},
    correctAnswer:"b" },
    { type:"fill", question:"14. Summary: Cloud databases are accessed online and hosted on.......... They reduce costs, improve security, and offer scalability.",
    passage:`Cloud databases are stored on remote servers that you can access through the internet. They offer scalability and save costs because companies don't need to buy hardware. Cloud databases also improve performance and security. Examples are Amazon RDS, Microsoft Azure SQL, and Google Cloud SQL. In addition, they support automatic backups, easy updates, and flexible storage options, making them ideal for businesses of all sizes and industries.`,
    answer:"remote servers" },
    { type:"fill", question:"15. Summary: Relational databases use tables which have ................... to keep data organized and can be managed easily with SQL commands.",
    passage:`Relational databases arrange data into tables made of rows and columns. Each row represents a single record, and each column stores one type of data. People use SQL to search and update these tables. Relational databases are popular because they are reliable and simple to organize for many applications. They are widely used in business, education, healthcare, and government systems.`,
    answer:"rows and columns" },
    { type:"fill", question:"16. Summary: A database stores organized information for easy access and management. Companies use it to track customers, products, and transactions. ................... systems software helps manage, update, and protect data efficiently.",
    passage:`A database is a system that stores information in an organized way. Companies use databases to keep records of customers, products, and transactions. Modern databases are managed by software called Database Management Systems (DBMS). This software such as MySQL, Oracle, and Microsoft SQL Server helps users add, edit, or delete data quickly. These systems also support data analysis, allow multiple users to work at once, and help prevent data loss or errors.`,
    answer:"Database management" },
    { type:"mc", question:"17. Keys are used to link related data between tables. (True/False)",
    passage:`Relational databases use tables to organize information. Each table has columns for different fields and rows for records. Tables can be connected using keys, which link related data. This design allows users to find information quickly and see how different pieces of data are related. It also helps reduce duplication and supports more accurate, efficient data management in large systems.`,
    answers:{a:"False", b:"True"},
    correctAnswer:"b" },
    { type:"mc", question:"18. Cloud databases can be accessed only from the office. (True/False)",
    passage:`Cloud databases allow companies to store information online instead of on local servers. This means employees can access data from anywhere with an internet connection. Cloud services also provide automatic updates and backups, reducing the work needed to maintain the system. Moreover, cloud databases can easily scale as business needs grow and often offer better security features.`,
    answers:{a:"True", b:"False"},
    correctAnswer:"b" },
    { type:"mc", question:"19. Digital databases are usually slower to search than paper records. (True/False)",
    passage:`A database is an organized collection of data that can be easily accessed and managed. Many companies use databases to track sales, store customer information, and monitor inventory. Compared to paper records, digital databases are faster to update and search, making daily work more efficient. They also support data analysis, improve accuracy, and help teams make better business decisions.`,
    answers:{a:"True", b:"False"},
    correctAnswer:"b" },
    { type:"fill", question:"20. She keeps the records organized…………clear categories. (by/create)", answer:"by creating" },
    { type:"fill", question:"21. I created a better layout ……………...... into useful forms and tables. (by / organize / data)", answer:"by organizing data" },
    { type:"fill", question:"22. We reduced mistakes in our reports ................... before saving them. (by / check / values)", answer:"by checking values" },
    { type:"fill", question:"23. We can avoid duplicate records .......... key for each entry. (by / use)", answer:"by using a unique" },
    { type:"fill", question:"24. They keep the data safe .......... information stored in the system. ( by / encrypt / sensitive)", answer:"by encrypting the sensitive" },
    { type:"fill", question:"25. We solved the problem quickly .................... the database. (by/query)", answer:"by querying" },
    { type:"order", question:"26. Sắp xếp để thành câu hoàn chỉnh:",
    correctSentence:"You can protect your data by backing up regularly.",
    tokens:["You","can","protect","your","data","by","backing","up","regularly."] },
    { type:"order", question:"27. Sắp xếp để thành câu hoàn chỉnh:",
    correctSentence:"Teams can improve their information sharing by using video conferences.",
    tokens:["Teams","can","improve","their information","sharing","by","using","video","conferences."] },
    { type:"order", question:"28. Sắp xếp để thành câu hoàn chỉnh:",
    correctSentence:"If the manager reviews the report, he can make better decisions.",
    tokens:["If","the","manager","reviews","the report,","he","can","make","better decisions."] },
    { type:"order", question:"29. Sắp xếp để thành câu hoàn chỉnh:",
    correctSentence:"You will need to create a report to summarize monthly sales.",
    tokens:["You","will","need","to create a","report","to","summarize","monthly sales."] },
    { type:"order", question:"30. Sắp xếp để thành câu hoàn chỉnh:",
    correctSentence:"They use different forms to create data entry easily and efficiently.",
    tokens:["They","use","different forms","to","create","data entry","easily and","efficiently."] }
],
"BÀI 5":[
    { type:"mc", question:"1. Màn hình độ phân giải cao rất cần thiết để xem rõ hình ảnh.",
    answers:{a:"High-definition microphones create images clearly.",
             b:"Low-definition monitors are essential for clear images.",
             c:"High-definition monitors are essential to see images clearly."},
    correctAnswer:"c" },
    { type:"mc", question:"2. Cuộc họp video giúp mọi người cộng tác dễ dàng hơn.",
    answers:{a:"Video meetings help people collaborate more easily.",
             b:"Video meetings don't help people work together easily.",
             c:"Video meetings reduce the number of participants easily."},
    correctAnswer:"a" },
    { type:"mc", question:"3. Người tham gia cuộc họp nên tắt micro khi không nói.",
    answers:{a:"Participants should turn on the microphone when not speaking.",
             b:"Participants should mute their microphone when not speaking.",
             c:"Participants should leave the meeting when not speaking."},
    correctAnswer:"b" },
    { type:"mc", question:"4. Internet tốc độ cao giúp cải thiện chất lượng cuộc gọi.",
    answers:{a:"Low-speed internet improves security.",
             b:"High-speed internet helps improve call quality.",
             c:"High-speed internet increases delays."},
    correctAnswer:"b" },
    { type:"mc", question:"5. If she ................... a better microphone, her voice would be clearer.",
    answers:{a:"had", b:"has", c:"have"}, correctAnswer:"a" },
    { type:"mc", question:"6. Their team ................... remotely every week if they ................... conferencing technology.",
    answers:{a:"would meet/ use", b:"would meet/ used", c:"met/ would use"},
    correctAnswer:"b" },
    { type:"mc", question:"7. A: How would things change if we ___________ better screen sharing tools? B: We ___________ data and charts more clearly in our presentations.",
    answers:{a:"use/ will see", b:"used/ would see", c:"used/ saw"},
    correctAnswer:"b" },
    { type:"mc", question:"8. A: If the company ................. in better equipment, meetings would be more effective. B: That's right.",
    answers:{a:"Invested", b:"invests", c:"invest"}, correctAnswer:"a" },
    { type:"mc", question:"9. A: What would you do if your conferencing platform suddenly stopped working? B: I ................. the IT team to fix the issue quickly.",
    answers:{a:"called", b:"call", c:"would call"}, correctAnswer:"c" },
    { type:"mc", 
    question:"10. Why do companies record video conferences?", 
    passage:`Many companies record video conferences so employees who cannot attend can watch later. These recordings are useful for training and reviewing important discussions. Privacy is important, so participants are usually informed when a session is being recorded. Some platforms automatically notify everyone with a message or sound alert at the start of recording.`,
    answers:{a:"To save internet bandwidth", b:"To let people review meetings later", c:"To punish employees"},
    correctAnswer:"b" },
    { type:"mc", 
    question:"11. What should you do before a video conference?", 
    passage:`Before joining a video conference, it is recommended to test your camera and microphone. Poor audio or video quality can make communication difficult. Participants should also check the lighting in their room so their faces are clear on camera. If there is background noise, using headphones or muting the microphone when not speaking can help. These steps make online meetings more professional and productive.`,
    answers:{a:"Make sure your lighting and audio work well", b:"Ignore camera and microphone settings", c:"Leave your microphone on all the time"},
    correctAnswer:"a" },
    { type:"mc", 
    question:"12. Why do people use virtual backgrounds in video calls?", 
    passage:`Some video conferencing software allows virtual backgrounds. This feature lets users display an image or blur their real background. It can help maintain privacy and create a more professional look. For example, a person working from home may use a company logo or a plain color as their background. However, virtual backgrounds may require more computer processing power and a good camera to work smoothly.`,
    answers:{a:"To improve internet speed", b:"To hide their real environment", c:"To reduce computer memory use"},
    correctAnswer:"b" },
    { type:"mc", 
    question:"13. What do you need to join a video conference?", 
    passage:`Video conferencing allows people to communicate in real time using audio and video over the internet. It is commonly used for remote meetings, interviews, and online classes. Participants can share their screens, send messages, and record sessions for future reference. To join a video conference, users usually need a webcam, microphone, and stable internet connection. Popular platforms include Zoom, Microsoft Teams, and Google Meet.`,
    answers:{a:"A printed invitation", b:"A webcam and internet connection", c:"A special computer with extra memory"},
    correctAnswer:"b" },
    { type:"mc", 
    question:"14. What is one advantage of video conferencing?", 
    passage:`One benefit of video conferencing is saving time and travel costs. Instead of flying to another city for a meeting, people can connect instantly online. This also helps reduce carbon emissions from transportation. However, video conferencing can have challenges like poor audio quality or unstable connections. To avoid problems, participants should test their equipment and internet before important calls.`,
    answers:{a:"It reduces the need to travel.", b:"It guarantees perfect audio every time.", c:"It always works without preparation."},
    correctAnswer:"a" },
    { type:"fill", 
    question:"15. Summary: A well-equipped room has good devices for meetings. It makes a professional environment and saves time, making ................... more effective.", 
    passage:`A high-quality video conferencing room is equipped with proper lighting, cameras and audio devices. It provides a professional environment and minimizes technical issues. Because everything is set up in advance, meetings can begin on time without delays caused by adjusting equipment. This setup also improves the communication quality and helps participants stay focused, making discussions more productive and efficient overall.`,
    answer:"the discussions" },
    { type:"fill", 
    question:"16. Summary: Security tools like ................... or waiting rooms keep meetings safe. They help protect sensitive information and important discussions.", 
    passage:`Many video conferencing platforms offer security features like passwords, encryption, and waiting rooms. These protect meetings from unauthorized access and help keep sensitive information safe. Organizations should use these features to maintain confidentiality during important discussions.`,
    answer:"passwords, encryption" },
    { type:"fill", 
    question:"17. Summary: Webcams add real-time video and build visual connection, ........ during discussions. It also makes professional communication more effective.", 
    passage:`Webcams make virtual meetings more personal by allowing participants to see each other in real time. This visual connection builds trust and increases engagement during discussions. Using a high-quality webcam also enhances image clarity, making professional communication more effective.`,
    answer:"increases engagement" },
    { type:"mc", 
    question:"18. Video conferencing is only used in the business sector. (True/False)", 
    passage:`Video conferencing helps people communicate face-to-face without being in the same room. It is widely used in business, education, and healthcare. Doctors can speak with patients remotely, and teachers can hold virtual classes. One major advantage is that it saves time and travel costs. However, users need a stable internet connection and compatible devices to participate effectively.`,
    answers:{a:"True", b:"False"}, correctAnswer:"b" },
    { type:"mc", 
    question:"19. Breakout rooms are used for smaller group discussions during video calls. (True/False)", 
    passage:`Many video conferencing tools offer features like screen sharing, chat, and breakout rooms. Screen sharing allows presenters to show slides or documents during meetings. Breakout rooms are useful for group discussions in online classes or workshops. These tools make virtual meetings more interactive and organized, helping participants stay engaged and collaborate better.`,
    answers:{a:"True", b:"False"}, correctAnswer:"a" },
    { type:"mc", 
    question:"20. It's better to sit with your back to a light source during a video call. (True/False)", 
    passage:`Good lighting and clear audio are important for professional video conferencing. People should sit facing a light source, like a window or lamp, so their face is visible. Background noise should be reduced, and participants should mute their microphones when not speaking. These simple steps improve communication and reduce distractions during meetings.`,
    answers:{a:"True", b:"False"}, correctAnswer:"b" },
    { type:"fill", question:"21. If I....... more money, I wouldn’t buy a new webcam. (not/have)", answer:"didn’t have" },
    { type:"fill", question:"22. If the internet is very slow and unstable, the video ................... (freeze)", answer:"would freeze" },
    { type:"fill", question:"23. A: How would things change if we ................... the system earlier? B: The video call wouldn't start right on time without delays. (not/set)", answer:"didn’t set up" },
    { type:"fill", question:"24. A: What .............................. we didn't record our meetings each week? B: We would miss important details. (if/ happen)", answer:"would happen if" },
    { type:"fill", question:"25. If you………. your microphone, no one will hear you. (not/check)", answer:"don’t check" },
    { type:"order", question:"26. What would you do if your conferencing platform suddenly stopped working?",
    correctSentence:"What would you do if your conferencing platform suddenly stopped working?",
    tokens:["What","would","you","do","if","your","conferencing","platform","suddenly stopped","working?"] },
    { type:"order", question:"27. If we had secure logins, our video conferencing would be more protected.",
    correctSentence:"If we had secure logins, our video conferencing would be more protected.",
    tokens:["If we","had","secure logins,","our video","conferencing","would","be more","protected."] },
    { type:"order", question:"28. If the company didn't train the team, they wouldn't use the control panel correctly.",
    correctSentence:"If the company didn't train the team, they wouldn't use the control panel correctly.",
    tokens:["If","the company","didn't","train","the team,","they","wouldn't","use","the control","panel","correctly."] },
    { type:"order", question:"29. If I knew how to share files, I would collaborate better with my remote team.",
    correctSentence:"If I knew how to share files, I would collaborate better with my remote team.",
    tokens:["If I","knew","how to","share files,","I","would","collaborate","better with","my remote","team."] },
    { type:"order", question:"30. If she were the manager, she would buy better equipment.",
    correctSentence:"If she were the manager, she would buy better equipment.",
    tokens:["If","she","were","the manager,","she would","buy","better","equipment."] }
],
"BÀI 6":[
    { type:"mc", question:"1. Nhà khoa học dữ liệu thu thập và xử lý thông tin từ dữ liệu lớn.",
    answers:{a:"The data scientist gathers and processes information from big data.",
             b:"The data scientist tests machine learning tools from big data.",
             c:"The data analyst stores customer profiles in a secure internal database."},
    correctAnswer:"a" },
    { type:"mc", question:"2. Nhà phân tích an ninh mạng bảo vệ hệ thống khỏi tấn công mạng.",
    answers:{a:"The cybersecurity analyst protects systems using internal security software.",
             b:"The cybersecurity analyst protects systems from cyberattacks.",
             c:"The cybersecurity analyst manages access and installs software on systems"},
    correctAnswer:"b" },
    { type:"mc", question:"3. Nhà phát triển phần mềm viết mã và tạo các ứng dụng hữu ích.",
    answers:{a:"The Al engineer designs useful tools for mobile security.",
             b:"The support technician helps users reset useful applications.",
             c:"The software developer writes code and creates useful applications."},
    correctAnswer:"c" },
    { type:"mc", question:"4. Nhà thiết kế giao diện tập trung vào trải nghiệm người dùng trên ứng dụng.",
    answers:{a:"The UI designer writes system code for mobile app displays.",
             b:"The UI designer tests user passwords during application login.",
             c:"The UI designer focuses on user experience in applications."},
    correctAnswer:"c" },
    { type:"mc", question:"5. Do they test the network manually or rely on automation……………?",
    answers:{a:"every time", b:"often", c:"always"},
    correctAnswer:"a" },
    { type:"mc", question:"6. Our team has project updates.................to keep everyone aligned.",
    answers:{a:"rarely", b:"once a week", c:"normally"},
    correctAnswer:"b" },
    { type:"mc", question:"7. …………….he forgets to document code, which causes confusion during testing.",
    answers:{a:"from time to time", b:"usually", c:"often"},
    correctAnswer:"a" },
    { type:"mc", question:"8. She……………joins the team call unless there's a system error to report.",
    answers:{a:"always", b:"every week", c:"hardly ever"},
    correctAnswer:"c" },
    { type:"mc", question:"9. I……………..check emails first in the morning to make the plan for the whole day.",
    answers:{a:"all the time", b:"always", c:"once a day"},
    correctAnswer:"b" },
    { type:"mc",
    question:"10. What does David do to prevent data issues from affecting company systems?",
    passage:`David works as a database administrator at a medical software company. Every morning, he logs into the system to check performance reports. He is responsible for designing databases that store important information, such as patient records and appointments. David also maintains databases by running regular updates and backups. From time to time, he works with software developers to test new features. If there is a problem with data access, David helps diagnose the problem quickly to avoid delays.`,
    answers:{a:"He maintains and backs up the databases",
             b:"He writes scripts to analyze user traffic",
             c:"He upgrades devices used by developers"},
    correctAnswer:"a" },
    { type:"mc",
    question:"11. How often does Amira evaluate her team's support work?",
    passage:`Amira is a helpdesk supervisor at a university IT department. She supervises a team of technical support people who assist staff and students. When someone has a problem with their computer, they contact Amira's team. She assigns the cases and checks that solutions are delivered on time. Amira often joins team calls to review performance and discuss common issues. Sometimes she trains new team members or writes short guides for solving simple problems.`,
    answers:{a:"Daily, by joining service calls",
             b:"Sometimes, during team performance reviews",
             c:"Often, through meetings and performance checks"},
    correctAnswer:"c" },
    { type:"mc",
    question:"12. What does Amira do to make sure users receive good support?",
    passage:`Amira is a helpdesk supervisor at a university IT department. She supervises a team of technical support people who assist staff and students. When someone has a problem with their computer, they contact Amira's team. She assigns the cases and checks that solutions are delivered on time. Amira often joins team calls to review performance and discuss common issues. Sometimes she trains new team members or writes short guides for solving simple problems.`,
    answers:{a:"She contacts users directly to solve every technical problem.",
             b:"She upgrades all software and hardware herself during the weekends.",
             c:"She creates simple instructions, manages her team, and tracks their support quality."},
    correctAnswer:"c" },
    { type:"mc",
    question:"13. Why is David's role important for the company's daily operations?",
    passage:`David works a database administrator at a medical software company. Every morning, he logs into the system to check performance reports. He is responsible for designing databases that store important information, such as patient records and appointments. David also maintains databases by running regular updates and backups. From time to time, he works with software developers to test new features. If there is a problem with data access, David helps diagnose the problem quickly to avoid delays.`,
    answers:{a:"He ensures data systems run well and helps fix data issues quickly.",
             b:"He manages the entire IT department and writes backup codes.",
             c:"He tests new medical equipment with doctors."},
    correctAnswer:"a" },
    { type:"mc",
    question:"14. What kind of problems does Anna mostly handle?",
    passage:`Anna works as an IT support technician in a busy office. She usually arrives at 8:00 a.m. and starts her day by checking support tickets. She normally handles hardware problems, but sometimes deals with software issues. Occasionally, she receives urgent calls from sales staff who are having problems during a client meeting. From time to time, Anna leads short training sessions to help employees learn new tools. She rarely visits other departments unless the issue requires her to be there physically.`,
    answers:{a:"network problems",
             b:"software crashes",
             c:"hardware issues"},
    correctAnswer:"c" },
    { type:"mc",
    question:"15. Hana installs new software and fixes broken computers when needed.",
    passage:`Hana works as a support technician at a small business. She looks after all the computers in the office and makes sure everything is working well. Each morning, she checks emails for reported issues. If someone’s computer isn’t working, she visits their desk to diagnose the problem. Hana also sets up new computers for new employees and installs software when needed. From time to time, she gives advice on how to avoid common tech problems.`,
    answers:{a:"True", b:"False"},
    correctAnswer:"a" },
    { type:"mc",
    question:"16. She writes software specifications so developers can build what clients need.",
    passage:`Sophie is a systems analyst at a company that builds software for businesses. When clients need a new system, Sophie visits them to understand their needs. She writes specifications for the software so the developers can build exactly what the client wants. Sophie often attends meetings with both the client and the software team. Sometimes she tests new software features to see if they match the requirements. She plays an important role in connecting business needs with technical solutions.`,
    answers:{a:"True", b:"False"},
    correctAnswer:"a" },
    { type:"mc",
    question:"17. Amira directly solves most of the support orders herself.",
    passage:`Amira is a helpdesk supervisor at a university IT department. She supervises a team of technical support people who assist staff and students. When someone has a problem with their computer, they contact Amira's team. She assigns the cases and checks that solutions are delivered on time. Amira often joins team calls to review performance and discuss common issues. Sometimes she trains new team members or writes short guides for solving simple problems.`,
    answers:{a:"True", b:"False"},
    correctAnswer:"b" },
    { type:"fill",
    question:"18. Summary: Hana is responsible for maintaining computers and supporting employees with tech issues. She installs software, sets up devices, and diagnoses problems on-site. She checks emails for reported issues and responds....................",
    passage:`Hana works as a support technician at a small business. She looks after all the computers in the office and makes sure everything is working well. Each morning, she checks emails for reported issues. If someone's computer isn't working, she visits their desk to diagnose the problem. Hana also sets up new computers for new employees and installs software when needed. From time to time, she gives advice on how to avoid common tech problems.`,
    answer:"when needed" },
    { type:"fill",
    question:"19. Summary: Freddy manages IT projects and reviews timelines and tasks every morning. He regularly meets clients and his team to solve problems or check progress. He also updates senior managers by writing…………….",
    passage:`Freddy is a project manager for an international software company. He is responsible for IT projects that involve building custom applications for clients. Freddy usually starts his day by reviewing project timelines and task lists. He meets with developers, designers, and clients to make sure everything is going as planned. If someone has a problem with deadlines or tools, Freddy helps solve it. He also writes reports to update senior managers on the team's progress.`,
    answer:"progress reports" },
    { type:"fill",
    question:"20. Summary: Sophie meets with clients to understand their software needs and writes detailed plans. These specifications help developers build the correct system. She also tests new features to check if they…………….",
    passage:`Sophie is a systems analyst at a company that builds software for businesses. When clients need a new system, Sophie visits them to understand their needs. She writes specifications for the software so the developers can build exactly what the client wants. Sophie often attends meetings with both the client and the software team. Sometimes she tests new software features to see if they match the requirements.`,
    answer:"match the requirements" },
    { type:"fill", question:"21. He………after he talks with the projectmanager. <b>( write / specifications / usually )</b>", answer:"usually writes the specifications" },
    { type:"fill", question:"22. Ahmed.....................all of our IT projects. <b>( responsible / always )</b>", answer:"is always responsible for" },
    { type:"fill", question:"23. We…………….unless we can't fix it ourselves. <b>( call/ support/ rarely )</b>", answer:"rarely call for support" },
    { type:"fill", question:"24. He…………in the morning before the office gets busy. <b>( write/ software/ usually )</b>", answer:"usually writes software" },
    { type:"fill", question:"25. She………every Monday to plan her weekly tasks. <b>( attend/ always/ meeting )</b>", answer:"always attends a meeting" },
    { type:"order",
    question:"26. Rearrange to make a complete sentence:",
    correctSentence:"We are responsible for our IT projects, so we rarely leave tasks unfinished.",
    tokens:["We","are responsible","for our","IT projects,","so we","rarely","leave","tasks","unfinished."] },
    { type:"order",
    question:"27. Rearrange to make a question:",
    correctSentence:"Does your team diagnose the problem quickly when users report system errors by email?",
    tokens:["Does","your team","diagnose the","problem quickly","when","users","report","system errors","by email?"] },
    { type:"order",
    question:"28. Rearrange to make a complete sentence:",
    correctSentence:"When I look after all the computers, I sometimes forget to update the antivirus.",
    tokens:["When","I look","after","all the","computers,","I sometimes","forget","to","update","the","antivirus."] },
    { type:"order",
    question:"29. Rearrange to make a complete sentence:",
    correctSentence:"Anna often sets up new computers when her company hires new staff.",
    tokens:["Anna often","sets","up","new","computers","when","her company","hires","new","staff."] },
    { type:"order",
    question:"30. Rearrange to make a complete sentence:",
    correctSentence:"I usually write software in the morning before attending the team meeting.",
    tokens:["I usually","write","software","in","the morning","before","attending","the team","meeting."] }
],
"BÀI 7":[
    { type:"mc", question:"1. CV của bạn nên trình bày rõ ràng kinh nghiệm làm việc.",
    answers:{a:"Your CV should focus only on personal hobbies.",
             b:"Your CV should be written in different colors.",
             c:"Your CV should clearly present work experience."},
    correctAnswer:"c" },
    { type:"mc", question:"2. Chúng ta có thể sử dụng trí tuệ nhân tạo để phân tích dữ liệu.",
    answers:{a:"We can use artificial intelligence to analyze data.",
             b:"We can send data without using artificial intelligence.",
             c:"We can use artificial intelligence in data science."},
    correctAnswer:"a" },
    { type:"mc", question:"3. Bạn nên nói về kĩ năng và điểm mạnh trong buổi phỏng vấn.",
    answers:{a:"You should talk about your address and ID card.",
             b:"You should talk about skills and strengths in the interview.",
             c:"You should talk about hobbies and pets in the interview."},
    correctAnswer:"b" },
    { type:"mc", question:"4. Tôi từng làm việc tại một công ty phần mềm nổi tiếng.",
    answers:{a:"I will work at a famous software company.",
             b:"I used to work at a famous software company.",
             c:"I work at a famous software company now."},
    correctAnswer:"b" },
    { type:"mc", question:"5. …………she ever…………a strong curriculum vitae for the internship program yet?",
    answers:{a:"Has/ written", b:"Has/ wrote", c:"Have/ written"},
    correctAnswer:"a" },
    { type:"mc", question:"6. ...............you…………..in teams to develop a university scheduling system?",
    answers:{a:"has/ used to work", b:"did/ use to work", c:"did/ used to work"},
    correctAnswer:"b" },
    { type:"mc", question:"7. He…………….in hackathons, but now he prefers solo projects.",
    answers:{a:"used to participate", b:"used to participating", c:"have participated"},
    correctAnswer:"a" },
    { type:"mc", question:"8. I………..send handwritten CVs, but now everything is online.",
    answers:{a:"was used", b:"used to", c:"have used"},
    correctAnswer:"b" },
    { type:"mc", question:"9. She…………as a support technician before becoming a developer.",
    answers:{a:"has work", b:"have worked", c:"has worked"},
    correctAnswer:"c" },
    { type:"mc", 
    question:"10. What does the intern's experience suggest about learning in real projects?", 
    passage:`Last summer, I worked as an intern Software Developer at a local tech startup. I was responsible for small coding tasks and duties such as writing backend functions and fixing bugs. I used to struggle with teamwork, but during the internship I have improved my communication skills. I also participated in daily meetings and project planning. This experience helped me understand my strengths in problem-solving and the importance of being open to feedback.`,
    answers:{a:"Technical skills are more important than communication",
             b:"Real work experiences help improve soft skills like teamwork",
             c:"Writing code alone is enough to succeed in a company"},
    correctAnswer:"b" },
    { type:"mc", 
    question:"11. When did Linh become more confident in her communication?", 
    passage:`Linh is applying for her first job as an IT Support Technician. She has written her curriculum vitae carefully, listing her skills in troubleshooting, customer service, and basic networking. She has taken part in several university IT projects and has always enjoyed helping friends fix computer problems. Although she used to be shy, she has become more confident by practicing interview questions with her classmates. She hopes her strengths will help her succeed in the job.`,
    answers:{a:"After working for two years in IT customer service",
             b:"After participating in interviews at several companies",
             c:"When she practiced interview questions with classmates"},
    correctAnswer:"c" },
    { type:"mc", 
    question:"12. How does InsightPro expect the candidate to support the company's goals?", 
    passage:`InsightPro is hiring a Data Analyst to join our expanding team. The ideal candidate must have strong analytical skills, attention to detail, and be able to work under pressure. Responsibilities include analyzing large data sets, creating visuals, and preparing reports for management. Prior experience with Python or R is preferred. Applicants must submit a curriculum vitae with relevant qualifications and examples of previous projects. This is a full-time role with competitive salary and performance bonuses.`,
    answers:{a:"By working with customers to improve sales strategies",
             b:"By preparing reports and creating tools to present data clearly",
             c:"By designing websites and managing social media platforms"},
    correctAnswer:"b" },
    { type:"mc", 
    question:"13. What challenge did the applicant face during the job application process?", 
    passage:`I have just completed my application for a junior Software Developer role at FutureTech. I wrote a detailed curriculum vitae, describing my qualifications, work experience, and skills. I mentioned that I have developed several mobile apps during university and taken part in open-source projects. The most difficult part was the salary negotiation, as I didn't have much experience with that. However, I explained my strengths and potential to grow. I hope to hear back soon and be invited to an interview.`,
    answers:{a:"Submitting a late curriculum vitae to the company",
             b:"Writing a cover letter for an foreign company",
             c:"Negotiating a salary without much prior experience"},
    correctAnswer:"c" },
    { type:"mc", 
    question:"14. What does Duy's part-time job as a help desk assistant show about him?", 
    passage:`Duy has recently been invited to an IT Support Technician job interview. He submitted his curriculum vitae last week and was selected as a strong candidate. During the interview, he was asked about his communication skills, how he handles technical issues, and whether he can work under pressure. Duy spoke confidently about his experience and strengths and even mentioned a past weakness-time management-which he has improved. He also shared that he used to work part-time as a help desk assistant while studying.`,
    answers:{a:"He lacks teamwork experience and prefers personal projects.",
             b:"He has real-world experience supporting users in technical environments.",
             c:"He prefers working independently without supervision"},
    correctAnswer:"b" },
    { type:"mc", 
    question:"15. Linh practiced interview questions with professionals at the company. (True/False)",
    passage:`Linh is applying for her first job as an IT Support Technician. She has written her curriculum vitae carefully, listing her skills in troubleshooting, customer service, and basic networking. She has taken part in several university IT projects and has always enjoyed helping friends fix computer problems. Although she used to be shy, she has become more confident by practicing interview questions with her classmates. She hopes her strengths will help her succeed in the job.`,
    answers:{a:"True", b:"False"}, correctAnswer:"b" },
    { type:"mc", 
    question:"16. The intern worked on bug fixes and backend code during the internship. (True/False)",
    passage:`Last summer, I worked as an intern Software Developer at a local tech startup. I was responsible for small coding tasks and duties such as writing backend functions and fixing bugs. I used to struggle with teamwork, but during the internship I have improved my communication skills. I also participated in daily meetings and project planning. This experience helped me understand my strengths in problem-solving and the importance of being open to feedback.`,
    answers:{a:"True", b:"False"}, correctAnswer:"a" },
    { type:"mc", 
    question:"17. The applicant has developed mobile apps and participated in open-source projects. (True/False)",
    passage:`I have just completed my application for a junior Software Developer role at FutureTech. I wrote a detailed curriculum vitae, describing my qualifications, work experience, and skills. I mentioned that I have developed several mobile apps during university and taken part in open-source projects. The most difficult part was the salary negotiation, as I didn't have much experience with that. However, I explained my strengths and potential to grow. I hope to hear back soon and be invited to an interview.`,
    answers:{a:"True", b:"False"}, correctAnswer:"a" },
    { type:"fill", 
    question:"18. Summary: The applicant included past development experience in their application and discussed the difficulty of....................., which he managed by explaining his strengths.", 
    passage:`I have just completed my application for a junior Software Developer role at FutureTech. I wrote a detailed curriculum vitae, describing my qualifications, work experience, and skills. I mentioned that I have developed several mobile apps during university and taken part in open-source projects. The most difficult part was the salary negotiation, as I didn't have much experience with that. However, I explained my strengths and potential to grow. I hope to hear back soon and be invited to an interview.`,
    answer:"salary negotiation" },
    { type:"fill", 
    question:"19. Summary: The writer completed a summer internship as a Software Developer at a tech startup. They handled various coding…………and gradually improved their teamwork and communication. The experience helped them recognize their strengths and the value of feedback.", 
    passage:`Last summer, I worked as an intern Software Developer at a local tech startup. I was responsible for small coding tasks and duties such as writing backend functions and fixing bugs. I used to struggle with teamwork, but during the internship I have improved my communication skills. I also participated in daily meetings and project planning. This experience helped me understand my strengths in problem-solving and the importance of being open to feedback.`,
    answer:"tasks and duties" },
    { type:"fill", 
    question:"20. Summary: Duy was selected for an interview and confidently discussed his background, including a …………….., which he has worked to overcome.", 
    passage:`Duy has recently been invited to an IT Support Technician job interview. He submitted his curriculum vitae last week and was selected as a strong candidate. During the interview, he was asked about his communication skills, how he handles technical issues, and whether he can work under pressure. Duy spoke confidently about his experience and strengths and even mentioned a past weakness-time management-which he has improved. He also shared that he used to work part-time as a help desk assistant while studying.`,
    answer:"past weakness-time" },
    { type:"fill", question:"21. Have they……………technical interviews since they graduated from the IT program? (take)", answer:"taken part in" },
    { type:"fill", question:"22. We.........................tasks and duties listed in a real IT job description. (never/ analyze/ have)", answer:"have never analyzed" },
    { type:"fill", question:"23. Ahmed…………….teamwork because he used to prefer working on his own. (learn/ recently)", answer:"has recently learned about" },
    { type:"fill", question:"24. I ………….. curriculum vitae for at least five different job positions. (already / write)", answer:"have already written a" },
    { type:"fill", question:"25. Anna ……….. group interviews, but now she enjoys speaking with other candidates. (not / use to / enjoy)", answer:"didn't use to enjoy" },
    { type:"order", question:"26. He used to ignore his weaknesses, but he has worked hard to improve them during training.",
    correctSentence:"He used to ignore his weaknesses, but he has worked hard to improve them during training.",
    tokens:["He","used","to ignore","his weaknesses,","but","he","has","worked","hard","to","improve","them","during training."] },
    { type:"order", question:"27. Ahmed has developed strong communication skills because he has worked on some group projects.",
    correctSentence:"Ahmed has developed strong communication skills because he has worked on some group projects.",
    tokens:["Ahmed","has","developed","strong communication","skills","because","he has","worked on","some group projects."] },
    { type:"order", question:"28. They used to study different majors, but now they all work as IT support technicians.",
    correctSentence:"They used to study different majors, but now they all work as IT support technicians.",
    tokens:["They","used","to","study","different majors,","but now","they all","work","as","IT support","technicians."] },
    { type:"order", question:"29. Have you ever taken part in a coding competition at your university or online?",
    correctSentence:"Have you ever taken part in a coding competition at your university or online?",
    tokens:["Have","you","ever","taken","part","in","a","coding","competition","at your","university","or online?"] },
    { type:"order", question:"30. I haven't written any detailed curriculum vitae to apply for a part-time internship position.",
    correctSentence:"I haven't written any detailed curriculum vitae to apply for a part-time internship position.",
    tokens:["I","haven't","written","any","detailed","curriculum","vitae","to apply","for","a","part-time","internship","position."] }
],
"BÀI 8":[
    { type:"mc", question:"1. Nhân viên phải thay đổi mật khẩu của họ mỗi tháng một lần.",
      answers:{a:"Staff must change their passwords once a month.", b:"Staff can skip updating their passwords once a month.", c:"Staff should share passwords with others once a month."},
      correctAnswer:"a" },
    { type:"mc", question:"2. Phòng Nhân sự chịu trách nhiệm tuyển dụng và hỗ trợ nhân viên.",
      answers:{a:"The IT Department installs operating systems and firewalls.", b:"The Sales Department builds and repairs computer equipment.", c:"The H.R Department is responsible for hiring and supporting employees."},
      correctAnswer:"c" },
    { type:"mc", question:"3. Bạn không được phép kết nối thiết bị cá nhân vào máy tính văn phòng.",
      answers:{a:"You must not save your personal files on the office server.", b:"You must not connect personal phones to office computers.", c:"You must not connect personal devices to office computers."},
      correctAnswer:"c" },
    { type:"mc", question:"4. Bộ phận R&D phát triển sản phẩm mới và cải tiến công nghệ.",
      answers:{a:"The R&D Department works with new customers every day.", b:"The R&D Department develops new products and improves technology.", c:"The R&D Department arranges team meetings and interviews."},
      correctAnswer:"b" },
    { type:"mc", question:"5. Interns ________________ install software without permission from the person in charge of IT.",
      answers:{a:"don't have to", b:"mustn't", c:"have to"},
      correctAnswer:"b" },
    { type:"mc", question:"6. Staff in the Human Resources Department ________________ be responsible for hiring new employees.",
      answers:{a:"mustn't", b:"have to", c:"don't have to"},
      correctAnswer:"b" },
    { type:"mc", question:"7. Anna ______________ manage customer issues directly because she's in Customer Service.",
      answers:{a:"mustn't", b:"must", c:"doesn't have to"},
      correctAnswer:"b" },
    { type:"mc", question:"8. ............. follow IT Department rules to keep company networks secure.",
      answers:{a:"mustn't", b:"don't have to", c:"must"},
      correctAnswer:"c" },
    { type:"mc", question:"9. ........ employees ... all meetings organized by the Board of Directors?",
      answers:{a:"Do/ have attend", b:"Do/ have to attend", c:"Does/ have to attend"},
      correctAnswer:"b" },
    { type:"mc", question:"10. Why is the IT Department important at Panasonic Vietnam?",
      passage:`Panasonic Vietnam sells electronics through its Sales Department and promotes them via Marketing. The IT Department supports employees and protects data. Workers must follow rules, including not installing software. The Accounting Department records financial data, and Customer Services assists customers. Human Resources hires staff and manages training. The company must keep its operations secure and efficient.`,
      answers:{a:"It trains new staff and hires technical workers", b:"It supports staff and ensures systems are securely managed", c:"It helps the Accounting Department record company finances"},
      correctAnswer:"b" },
    { type:"mc", question:"11. How does Foxconn ensure workplace safety and communication across departments?",
      passage:`Foxconn has global factories producing electronics. The IT Department manages networks and keeps systems safe. The R&D Department researches production methods. Staff must wear ID badges and mustn't use personal devices. Human Resources hires and trains employees. The Board of Directors plans strategy. The Communication Department coordinates updates and supports internal communication across departments.`,
      answers:{a:"By selling products globally and training its sales team", b:"By promoting employee sales and increasing factory output", c:"By coordinating updates through Communication and enforcing IT rules"},
      correctAnswer:"c" },
    { type:"mc", question:"12. What must Canon employees do to follow company technology and teamwork policies?",
      passage:`Canon Vietnam has factories producing printers. The R&D Department develops new technology. Human Resources hires staff and coordinates training. The IT Department manages the network and ensures safety. Employees must follow strict rules and should attend all meetings. They also have to connect to secure networks. Marketing promotes products, while Communication coordinates messages. The person in charge of IT helps maintain secure operations.`,
      answers:{a:"Wear badges, attend meetings, and avoid system updates", b:"Follow safety rules, attend meetings, and use secure networks", c:"Use personal USB drives and skip communication meetings"},
      correctAnswer:"b" },
    { type:"mc", question:"13. What do Canon Vietnam employees have to do to follow company safety and teamwork rules?",
      passage:`Canon Vietnam has factories producing printers. The R&D Department develops new technology. Human Resources hires staff and coordinates training. The IT Department manages the network and ensures safety. Employees must follow strict rules and should attend all meetings. They also have to connect to secure networks. Marketing promotes products, while Communication coordinates messages. The person in charge of IT helps maintain secure operations.`,
      answers:{a:"Install marketing software and promote new products", b:"Skip meetings and use open wireless networks", c:"Connect to secure networks and attend scheduled meetings"},
      correctAnswer:"c" },
    { type:"mc", question:"14. What responsibilities does the R&D Department at Microsoft have?",
      passage:`Microsoft develops global software solutions. The R&D Department creates tools like AI and cloud platforms. IT Department supports systems and keeps data safe. Staff mustn't share passwords and should report problems. The Board of Directors manages strategy. Sales sells products to clients. Marketing promotes services worldwide. Customer Services assists millions of customers daily with technical support.`,
      answers:{a:"Selling services to international clients", b:"Assisting customers with product support", c:"Creating tools like AI and cloud platforms"},
      correctAnswer:"c" },
    { type:"mc", question:"15. Employees are expected to attend team meetings regularly.",
      passage:`Canon Vietnam has factories producing printers. The R&D Department develops new technology. Human Resources hires staff and coordinates training. The IT Department manages the network and ensures safety. Employees must follow strict rules and should attend all meetings.`,
      answers:{a:"True", b:"False"},
      correctAnswer:"a" },
    { type:"mc", question:"16. Employees must report system problems and avoid sharing passwords.",
      passage:`Microsoft develops global software solutions. The R&D Department creates tools like AI and cloud platforms. IT Department supports systems and keeps data safe. Staff mustn't share passwords and should report problems.`,
      answers:{a:"True", b:"False"},
      correctAnswer:"a" },
    { type:"mc", question:"17. Customer Services is responsible for managing company sales and advertising.",
      passage:`Panasonic Vietnam sells electronics through its Sales Department and promotes them via Marketing. The IT Department supports employees and protects data. Workers must follow rules, including not installing software. The Accounting Department records financial data, and Customer Services assists customers. Human Resources hires staff and manages training.`,
      answers:{a:"True", b:"False"},
      correctAnswer:"b" },
    { type:"fill", question:"18. Summary: Foxconn produces electronics in global factories and ensures system safety through its IT Department. Staff must follow security rules like wearing ID badges and not using ____________ . Communication supports internal messaging, while the Board guides overall company strategy.",
      passage:`Foxconn has global factories producing electronics for leading technology brands. The IT Department manages complex networks and ensures systems remain secure. The R&D Department researches advanced production methods. Employees must wear ID badges and mustn't use personal devices at work. Human Resources hires and trains staff. The Board of Directors sets company strategy, while Communication coordinates updates and supports internal communication.`,
      answer:"personal devices" },
    { type:"fill", question:"19. Samsung Electronics Vietnam produces electronics and has strict workplace rules. The Communication Department handles updates, and the ____________ ensures staff are trained for smooth factory operations.",
      passage:`Samsung Electronics Vietnam has large factories producing electronic devices for global markets. The Human Resources Department hires and trains staff to ensure efficient operations. Employees must follow safety rules and mustn't bring food near equipment. The Accounting Department records financial transactions, while Customer Services assists users. Marketing promotes new products, and Communication manages internal updates. IT supports staff and secures company networks daily.`,
      answer:"Human Resources Department" },
    { type:"fill", question:"20. Canon Vietnam produces printers in its factories. The IT Department ensures network safety and is supported by the ................ of IT. Employees follow strict rules, attend meetings, and use secure connections.",
      passage:`Canon Vietnam has factories producing printers. The R&D Department develops new technology. Human Resources hires staff and coordinates training. The IT Department manages the network and ensures safety. Employees must follow strict rules and should attend all meetings. They also have to connect to secure networks. Marketing promotes products, while Communication coordinates messages. The person in charge of information technology helps maintain secure operations.`,
      answer:"person in charge" },
    { type:"fill", question:"21. Anna _______________ quickly when she receives urgent updates from the Marketing Department. (have to/ coordinate)",
      answer:"has to coordinate" },
    { type:"fill", question:"22. Staff _______________ to maintain a good relationship in the IT workplace. (must/ support/ other)",
      answer:"must support each other" },
    { type:"fill", question:"23. ............... all financial transactions by the end of each working day. (not/ have to/ record)",
      answer:"doesn't have to record" },
    { type:"fill", 
    question:"24. Do we .................. when the client database is down? (have/support/ customers)", 
    answer:"have to support customers" },
    { type:"fill", question:"25. You _______________ to the Board of Directors when planning the company's IT projects to get useful directions. ( must/ report/ problem )",
      answer:"must report problems" },
    { type:"order", question:"26. Arrange to form a correct sentence:",
      correctSentence:"Do employees in the Sales Department have to record every transaction accurately and promptly?",
      tokens:["Do","employees in the","Sales Department","have to","record","every transaction accurately","and promptly?"] },
    { type:"order", question:"27. Arrange to form a correct sentence:",
      correctSentence:"Anna doesn't have to coordinate with the Communication Department to promote the new software products.",
      tokens:["Anna","doesn't","have to","coordinate with","the Communication Department","to promote","the new","software","products."] },
    { type:"order", question:"28. Arrange to form a correct sentence:",
      correctSentence:"Must he attend meetings regularly with the Board of Directors and present financial reports?",
      tokens:["Must","he","attend","meetings","regularly with","the Board","of Directors","and present","financial reports?"] },
    { type:"order", question:"29. Arrange to form a correct sentence:",
      correctSentence:"Do we have to support our colleagues in the R&D Department during new product research projects?",
      tokens:["Do","we","have to","support","our colleagues","in the R&D","Department","during","new product","research projects?"] },
    { type:"order", question:"30. Arrange to form a correct sentence:",
      correctSentence:"You mustn't share your password with people you work with because of IT security regulations.",
      tokens:["You","mustn't","share","your password","with","people you","work with","because of","IT security","regulations."] }
],
"BÀI 9":[
    { type:"mc", question:"1. Học sâu và mạng nơ-ron giúp máy hiểu hình ảnh phức tạp.",
    answers:{a:"Deep learning and neural networks help machines understand complex images.",
             b:"Deep learning and neural networks stop machines from learning new images.",
             c:"Deep learning and neural networks clean the images before using them."},
    correctAnswer:"a" },
    { type:"mc", question:"2. Tự động hóa giúp giảm công việc thủ công và cải thiện thuật toán.",
    answers:{a:"Automation helps people walk and talk better with robots.",
             b:"Automation writes long emails and sends them to the boss.",
             c:"Automation reduces manual work and improves the algorithm."},
    correctAnswer:"c" },
    { type:"mc", question:"3. ………….we……………the algorithm before starting the next test?",
    answers:{a:"Shall/ improving", b:"Shall/ improve", c:"What about/ improving"},
    correctAnswer:"b" },
    { type:"mc", question:"4. …………..we…………….unsupervised learning for this task first?",
    answers:{a:"Could/ try", b:"What about/ trying", c:"Could/ trying"},
    correctAnswer:"a" },
    { type:"mc", question:"5. How about……………..neural networks for better image recognition?",
    answers:{a:"using", b:"to use", c:"used"},
    correctAnswer:"a" },
    { type:"mc", question:"6. What is one suggested use of machine learning in the passage?",
    passage:`Machine learning is a powerful method where systems learn from data instead of being manually programmed. It uses supervised or unsupervised learning, algorithms, and models to make predictions. Neural networks improve their performance, but overfitting remains a concern. How about using machine learning to personalize user experiences in apps and websites by analyzing behavior and adjusting content in real time?`,
    answers:{a:"To help users write code more efficiently.",
             b:"To replace manual programming in all business software.",
             c:"To personalize app and website content based on user behavior."},
    correctAnswer:"c" },
    { type:"mc", question:"7. What benefit does NLP offer to professionals reading long reports?",
    passage:`Natural Language Processing (NLP) helps machines understand and respond to human language. It's widely used in applications like chatbots, virtual assistants, and speech-to-text tools. NLP combines deep learning and massive training data to enhance accuracy. Besides, it often works with computer vision in AI systems. What about using NLP to scan long reports and generate short, clear summaries for busy professionals?`,
    answers:{a:"It scans and generates short summaries for quicker understanding.",
             b:"It helps by highlighting every grammar mistake in the text.",
             c:"It translates the reports into different languages automatically."},
    correctAnswer:"a" },
    { type:"mc", question:"8. What makes Grok especially effective in customer service interactions?",
    passage:`Grok is a smart and witty chatbot created by xAI. It uses supervised learning and huge training data to understand language and provide clever answers. It also adapts quickly using natural language processing and neural networks. Its humorous responses make it engaging for users. What about using Grok for customer service to make support more friendly, interactive, and informative?`,
    answers:{a:"It ignores context and relies only on pre-written answers.",
             b:"It gives long technical responses that only experts understand.",
             c:"It responds to questions with helpful, fast, and humorous language."},
    correctAnswer:"c" },
    { type:"mc", question:"9. DeepSeek uses four types of technologies to generate predictions. (True/False)",
    passage:`DeepSeek is a cutting-edge AI model known for its ability to analyze large and complex datasets. It uses unsupervised learning, neural networks, and machine learning to find patterns in data. Its predictions are often accurate because it improves with every new training cycle. Could we use DeepSeek in healthcare systems to help doctors make better and faster decisions during diagnoses?`,
    answers:{a:"True", b:"False"},
    correctAnswer:"b" },
    { type:"mc", question:"10. Grok adapts its responses using natural language processing. (True/False)",
    passage:`Grok is a smart and witty chatbot created by xAI. It uses supervised learning and huge training data to understand language and provide clever answers. It also adapts quickly using natural language processing and neural networks. Its humorous responses make it engaging for users.`,
    answers:{a:"True", b:"False"},
    correctAnswer:"a" },
    { type:"mc", question:"11. ChatGPT cannot improve over time although it learns from large datasets. (True/False)",
    passage:`ChatGPT is a powerful and intelligent chatbot developed using generative AI. It combines deep learning and natural language processing to produce human-like conversations. It continuously learns from large datasets and becomes smarter over time. Besides text, it supports speech-to-text interactions.`,
    answers:{a:"True", b:"False"},
    correctAnswer:"b" },
    { type:"fill", question:"12. Summary: DeepSeek is an advanced model that analyzes large and complex datasets. It applies three technologies to discover patterns. Its predictions become more accurate with each………………….", 
    answer:"new training cycle",
    passage:`DeepSeek is a cutting-edge AI model known for its ability to analyze large and complex datasets. It uses unsupervised learning, neural networks, and machine learning to find patterns in data. Its predictions are often accurate because it improves with every new training cycle. Could we use DeepSeek in healthcare systems to help doctors make better and faster decisions during diagnoses?`
    },
    { type:"fill", question:"13. Summary: Grok uses supervised learning and large datasets to generate replies. It responds to questions with humor and adjusts quickly to new input. Its replies are powered by…………………, which makes Grok ideal for training support.",
    answer:"natural language processing",
    passage:`Grok is a smart and witty chatbot created by xAI. It uses supervised learning and huge training data to understand language and provide clever answers. It also adapts quickly using natural language processing and neural networks. Its humorous responses make it engaging for users. What about using Grok for customer service to make support more friendly, interactive, and informative?`
    },
    { type:"fill", question:"14. How about……………….so that the deep learning system works more efficiently? (use/ clean/ data)", 
      answer:"using clean data" },
    { type:"fill", question:"15. We couldn't finish the model yesterday, so shall we…………………today instead? (continue/ data analysis)", 
      answer:"continue the data analysis" },
    { type:"fill", question:"16. What about…………………because the current app lacks voice recognition? (add/ speech-to-text)", 
      answer:"adding speech-to-text technology" },
    { type:"order", question:"17. Sắp xếp để thành câu hoàn chỉnh:", 
      correctSentence:"The team hasn't had many ideas, so how about creating content drafts by using generative AI?", 
      tokens:["The team","hasn't had","many ideas,","so how","about","creating","content drafts","by using","generative AI?"] },
    { type:"order", question:"18. Sắp xếp để thành câu hoàn chỉnh:", 
      correctSentence:"I noticed the prediction model is inaccurate, so could you check the training data again?", 
      tokens:["I","noticed","the prediction","model","is","inaccurate,","so could","you","check","the training","data again?"] },
    { type:"order", question:"19. Sắp xếp để thành câu hoàn chỉnh:", 
      correctSentence:"How about using expert systems to manage the growing number of user requests efficiently?", 
      tokens:["How about","using","expert","systems","to manage","the growing","number of","user requests","efficiently?"] },
    { type:"order", question:"20. Sắp xếp để thành câu hoàn chỉnh:", 
      correctSentence:"She fixed the bug, but what about testing the chatbot before deploying it to users?", 
      tokens:["She","fixed","the bug,","but what"," about","testing","the chatbot","before","deploying it","to users?"] }
]
};

    // =====================================================================
    // CÁC HÀM XỬ LÝ
    // =====================================================================
    const quizForm = document.getElementById('quizForm');
    const resultsPage = document.getElementById('page-results');
    const quizPage = document.getElementById('page-quiz');
    const quizTitleEl = document.getElementById('quiz-title');
    const exerciseListEl = document.getElementById('exercise-list');

    const fireworksContainer = document.getElementById('fireworksContainer');
    const finalScoreEl = document.getElementById('finalScore');
    const okButton = document.getElementById('okButton');
    let fireworks;

    let quizData = {}; // Biến toàn cục lưu trữ dữ liệu bài quiz hiện tại
    let detailedResultsHTML = ''; // Biến toàn cục lưu kết quả

    /**
     * Tự động tạo danh sách bài tập trên trang chủ
     */
    function buildHomepage() {
        let listHTML = '';
        let index = 1;
        for (const key in quizzes) {
            const quizNum = key.replace('BÀI ', '');
            listHTML += `
                <li style="margin: 15px 0;">
                    <a href="#bai${quizNum}" class="exercise-link">Bài tập ${quizNum}</a>
                </li>
            `;
            index++;
        }
        exerciseListEl.innerHTML = listHTML;
    }


    /**
     * Hàm tạo form trắc nghiệm
     */
    function buildQuiz() {
        let formHTML = '';
        if (!quizData || !quizData.questions) {
            quizForm.innerHTML = "<p>Không thể tải dữ liệu bài tập. Vui lòng thử lại.</p>";
            return;
        }

        quizData.questions.forEach((q, index) => {
            formHTML += `<div class="question-block" style="animation-delay: ${index * 100}ms">`; 
            
            // Thêm câu hỏi, thay thế <b> và <i>
            let questionText = q.question.replace(/<b>/g, '<strong>').replace(/<\/b>/g, '</strong>');
            questionText = questionText.replace(/<i>/g, '<em>').replace(/<\/i>/g, '</em>');
            formHTML += `<p>${index + 1}. ${questionText}</p>`;

            if (q.passage) {
                formHTML += `<div class="passage">${q.passage}</div>`;
            }
            formHTML += `<div class="options">`;

            if (q.type === 'mc') {
                for (const optionKey in q.answers) {
                    formHTML += `<label><input type="radio" name="answers[${index}]" value="${optionKey}"> <strong>${optionKey.toUpperCase()}.</strong> ${q.answers[optionKey]}</label>`;
                }
            } else if (q.type === 'fill') {
                formHTML += `<input type="text" name="answers[${index}]" class="fill-in-blank-input" placeholder="Nhập câu trả lời của bạn...">`;
            } else if (q.type === 'order') {
                formHTML += `<div><b>Sắp xếp các từ/cụm từ sau:</b><div class="tokens-container">`;
                q.tokens.forEach(token => {
                    formHTML += `<span class="token-drag">${token}</span>`;
                });
                formHTML += `</div></div>`;
                formHTML += `<input type="text" name="answers[${index}]" class="fill-in-blank-input" placeholder="Nhập câu trả lời đã sắp xếp...">`;
            }

            formHTML += `</div></div>`;
        });
        formHTML += `<hr><button type="submit" class="quiz-submit-btn">Nộp Bài</button>`;
        quizForm.innerHTML = formHTML;
    }

    /**
     * Hàm chuẩn hóa câu trả lời để so sánh
     */
    function normalizeAnswer(answer) {
        if (typeof answer !== 'string') return "";
        return answer.trim().toLowerCase().replace(/[.?!,]/g, '');
    }

    /**
     * Hàm xử lý nộp bài và chấm điểm
     */
    quizForm.addEventListener('submit', function(event) {
        event.preventDefault(); 

        const formData = new FormData(quizForm);
        let score = 0;
        detailedResultsHTML = `
            <a class="back-link" href="#${quizData.pageId}">← Làm lại bài</a>
            <h1>📝 Kết quả ${quizData.title}</h1>
            <div class="result-detail">`;

        quizData.questions.forEach((q, index) => {
            const userAnswer = formData.get(`answers[${index}]`);
            const cleanedUserAnswer = normalizeAnswer(userAnswer);
            let isCorrect = false;
            let correctAnswerText = '';

            if (q.type === 'mc') {
                isCorrect = userAnswer === q.correctAnswer;
                correctAnswerText = `${q.correctAnswer.toUpperCase()}. ${q.answers[q.correctAnswer]}`;
            } else if (q.type === 'fill') {
                const correctAnswers = Array.isArray(q.answer) ? q.answer : [String(q.answer)];
                isCorrect = correctAnswers.some(ans => normalizeAnswer(ans) === cleanedUserAnswer);
                correctAnswerText = correctAnswers.join(' / ');
            } else if (q.type === 'order') {
                isCorrect = normalizeAnswer(q.correctSentence) === cleanedUserAnswer;
                correctAnswerText = q.correctSentence;
            }

            if (isCorrect) score++;
            
            // Xây dựng HTML chi tiết cho từng câu
            detailedResultsHTML += `<div class="question-block ${isCorrect ? 'correct' : 'incorrect'}" style="animation-delay: ${index * 50}ms">`;
            
            // Thêm câu hỏi
            let questionText = q.question.replace(/<b>/g, '<strong>').replace(/<\/b>/g, '</strong>');
            questionText = questionText.replace(/<i>/g, '<em>').replace(/<\/i>/g, '</em>');
            detailedResultsHTML += `<p>${index + 1}. ${questionText}</p>`;
            
            if (q.passage) {
                detailedResultsHTML += `<div class="passage">${q.passage}</div>`;
            }

            let userAnswerText = '<i>Chưa trả lời</i>';
            if (userAnswer) {
                if (q.type === 'mc') {
                    userAnswerText = `<strong>${userAnswer.toUpperCase()}.</strong> ${q.answers[userAnswer]}`;
                } else {
                    userAnswerText = `<strong>${userAnswer}</strong>`;
                }
            }
            
            detailedResultsHTML += `<p>Câu trả lời của bạn: <span class="user-answer">${userAnswerText}</span></p>`;
            detailedResultsHTML += `<p class="correct-answer-text-box">✔ Đáp án đúng: ${correctAnswerText}</p>`;
            detailedResultsHTML += `</div>`;
        });
        
        const totalQuestions = quizData.questions.length;
        const score10 = ((score / totalQuestions) * 10).toFixed(2);

        const summaryHTML = `
            <div class="result-summary">
                <h2>Điểm của bạn: ${score10} / 10</h2>
                <p>(Trả lời đúng ${score} / ${totalQuestions} câu)</p>
            </div>`;
        
        detailedResultsHTML = summaryHTML + detailedResultsHTML + '</div>';

        // --- Kích hoạt hiệu ứng pháo hoa ---
        finalScoreEl.textContent = `${score10} / 10`;
        fireworksContainer.style.display = 'block';
        setTimeout(() => fireworksContainer.classList.add('active'), 10); 
        
        if (!fireworks) {
            fireworks = new Fireworks.default(fireworksContainer);
        }
        fireworks.start();
    });

    // --- Xử lý nút OK trên modal ---
    okButton.addEventListener('click', () => {
        fireworks.stop();
        fireworksContainer.classList.remove('active');
        setTimeout(() => fireworksContainer.style.display = 'none', 300); 
        window.location.hash = '#results';
    });

    // --- LOGIC XỬ LÝ FULLSCREEN ---
    function enterFullscreen(element) {
        if (element.requestFullscreen) {
            element.requestFullscreen();
        } else if (element.mozRequestFullScreen) { // Firefox
            element.mozRequestFullScreen();
        } else if (element.webkitRequestFullscreen) { // Chrome, Safari, Opera
            element.webkitRequestFullscreen();
        } else if (element.msRequestFullscreen) { // IE/Edge
            element.msRequestFullscreen();
        }
    }

    function exitFullscreen() {
        if (document.exitFullscreen) {
            document.exitFullscreen();
        } else if (document.mozCancelFullScreen) {
            document.mozCancelFullScreen();
        } else if (document.webkitExitFullscreen) {
            document.webkitExitFullscreen();
        } else if (document.msExitFullscreen) {
            document.msExitFullscreen();
        }
    }

    // --- Logic chuyển trang (Router) ---
    function showPage() {
        const pageId = window.location.hash.substring(1) || 'home';
        
        const currentActive = document.querySelector('.page-content.active');
        if (currentActive) {
            currentActive.classList.remove('active');
        }

        setTimeout(() => {
            if (currentActive) currentActive.style.display = 'none';

            let pageToShowId = 'page-home'; // Mặc định là trang chủ
            let activePage;

            if (pageId.startsWith('bai')) {
                // Đây là một trang quiz
                const quizKey = "BÀI " + pageId.substring(3);
                if (quizzes[quizKey]) {
                    quizData = {
                        pageId: pageId, // Lưu lại hash (vd: 'bai1')
                        title: `Bài tập ${pageId.substring(3)}`,
                        questions: quizzes[quizKey]
                    };
                    quizTitleEl.textContent = `📝 ${quizData.title}`;
                    buildQuiz();
                    quizForm.reset();
                    pageToShowId = 'page-quiz';
                }
            } else if (pageId === 'results') {
                // Đây là trang kết quả
                resultsPage.innerHTML = detailedResultsHTML;
                pageToShowId = 'page-results';
            }
            // Mọi trường hợp khác (bao gồm 'home' hoặc hash không hợp lệ) sẽ hiển thị 'page-home'

            activePage = document.getElementById(pageToShowId);
            
            if (activePage) {
                activePage.style.display = 'block';
                setTimeout(() => activePage.classList.add('active'), 10); 
                document.title = activePage.querySelector('h1').textContent.replace(/[📝📚]/g, '').trim();
            }
            
            window.scrollTo(0, 0); // Cuộn lên đầu trang

        }, 200); // Đợi hiệu ứng mờ kết thúc
    }

    window.addEventListener('hashchange', showPage);
    
    // --- JAVASCRIPT CHO DARK MODE & EVENTS ---
    document.addEventListener('DOMContentLoaded', () => {
        const preloader = document.getElementById('preloader');
        preloader.classList.add('hidden');
        
        buildHomepage(); // Tạo danh sách bài tập khi tải trang
        showPage();  // Hiển thị trang chính xác dựa trên hash (nếu có)

        const themeToggle = document.getElementById('themeToggle');
        const body = document.body;

        function applyTheme(theme) {
            body.classList.toggle('dark-mode', theme === 'dark');
        }

        themeToggle.addEventListener('click', () => {
            let newTheme = body.classList.contains('dark-mode') ? 'light' : 'dark';
            applyTheme(newTheme);
            localStorage.setItem('theme', newTheme);
        });

        const savedTheme = localStorage.getItem('theme') || 'light';
        applyTheme(savedTheme);

        // Bắt sự kiện click trên toàn bộ body để xử lý fullscreen
        document.body.addEventListener('click', function(event) {
            // Dùng event delegation để bắt link bài tập
            const exerciseLink = event.target.closest('.exercise-link');
            if (exerciseLink) {
                // Không cần preventDefault vì chúng ta muốn hash thay đổi
                enterFullscreen(document.documentElement);
            }

            if (event.target.matches('.back-link')) {
                // Không cần preventDefault vì chúng ta muốn hash thay đổi
                exitFullscreen();
            }
        });
    });

</script>

</body>
</html> GUI lets users interact with computers easily by clicking icons and menus, making it **.................** for users. It helps people work efficiently without needing to learn commands.", 
      answer:"faster and more comfortable", 
      passage:`A GUI, or Graphical User Interface, makes it easy for users to interact with computers. Instead of typing commands, users can click icons, select items from menus, and drag windows. This visual method is faster and more comfortable for most people, especially beginners. Popular operating systems like Windows and macOS use GUIs to help users access files and applications easily. With a good GUI, users spend less time learning commands and more time working productively.`
    },

    { type:"mc", question:"19. The system specifications are only important for playing games. (True/False)", answers:{a:"True", b:"False", c:"Not sure"}, correctAnswer:"b", passage:"When buying a computer, it is important to check the system specifications. These include the processor speed, RAM size, hard drive capacity, and operating system. A faster processor helps the computer run programs quickly. More RAM allows for smoother multitasking. The operating system must support the software you plan to use. If you want to play games or edit videos, you’ll need a more powerful system."},
    { type:"mc", question:"20. GUIs are harder to use than command-line interfaces. (True/False)", answers:{a:"True", b:"False", c:"Depends"}, correctAnswer:"b", passage:"A Graphical User Interface (GUI) allows users to interact with a computer using images, icons, and windows. It is easier for beginners than a command-line interface, which requires typing commands. Most modern operating systems like Windows and macOS use a GUI."},

    { type:"fill", question:"21. A: The window is too big and covers the screen. B: _______ the side to make room. (move)", answer:"Move it to"},
    { type:"fill", question:"22. A: I'm going to close the file. B: Wait! _______ first. (save)", answer:"Save it"},
    { type:"fill", question:"23. A: This screen looks strange on my monitor. B: _______ display resolution in the settings. (adjust)", answer:"Adjust the"},
    { type:"fill", question:"24. A: I’m not sure if this program will run on my laptop. B: _______ system requirements first. (check)", answer:"Check the"},
    { type:"fill", question:"25. A: I want to open the settings, but I don’t know how to do. B: _______ icon on the taskbar. (click)", answer:"Click on the gear"},

    { type:"order", question:"26. Sắp xếp để thành câu hoàn chỉnh:", correctSentence:"Always check the system specifications carefully before buying a new computer.", tokens:["Always","check","the system","specifications","carefully before","buying","a new","computer."] },
    { type:"order", question:"27. Sắp xếp để thành câu hoàn chỉnh:", correctSentence:"To open the file, simply click on the icon in the Graphical User Interface (GUI).", tokens:["To","open","the file, simply","click on","the icon","in the","Graphical","User","Interface (GUI)."] },
    { type:"order", question:"28. Sắp xếp để thành câu hoàn chỉnh:", correctSentence:"Carefully select software that matches your system specifications before installing it.", tokens:["Carefully","select","software","that matches","your system","specifications","before installing","it."] },
    { type:"order", question:"29. Sắp xếp để thành câu hoàn chỉnh:", correctSentence:"Keep your system and software updated regularly for better performance.", tokens:["Keep","your system","and software","updated regularly","for better","performance."] },
    { type:"order", question:"30. Sắp xếp để thành câu hoàn chỉnh:", correctSentence:"Do not install programs if your system doesn't meet the requirements.", tokens:["Do not","install","programs","if your","system doesn't","meet","the requirements."] }
  ],


  "BÀI 2":[
  { type:"mc", question:"1. Trình duyệt của tôi lưu mật khẩu để đăng nhập nhanh hơn.",
    answers:{a:"My browser hides passwords to protect data.",
             b:"My browser deletes passwords to log in faster.",
             c:"My browser saves passwords for quicker logins."},
    correctAnswer:"c" },

  { type:"mc", question:"2. Máy tính của tôi tự động cài đặt bản cập nhật khi khởi động lại.",
    answers:{a:"My computer automatically installs updates when shutting down.",
             b:"My computer automatically installs updates when restarting.",
             c:"My computer manually installs updates when restarting."},
    correctAnswer:"b" },

  { type:"mc", question:"3. Trình duyệt web này hỗ trợ nhiều thẻ cùng lúc.",
    answers:{a:"This web browser deletes multiple tabs at once.",
             b:"This web browser downloads multiple files at once.",
             c:"This web browser supports multiple tabs at once."},
    correctAnswer:"c" },

  { type:"mc", question:"4. Hệ điều hành Windows 10 đã phát hành năm 2015.",
    answers:{a:"The Windows 10 operating system will release in 2015.",
             b:"The Windows 10 operating system was released in 2015.",
             c:"The Windows 10 browser was updated in 2015."},
    correctAnswer:"b" },

  { type:"mc", question:"5. Tôi cần đánh dấu trang web này để đọc sau.",
    answers:{a:"I need to sync this website to read later.",
             b:"I need to refresh this website to read later.",
             c:"I need to bookmark this website to read later."},
    correctAnswer:"c" },

  { type:"mc", question:"6. Many people ............... private Browse when they visit websites on public computers.",
    answers:{a:"use", b:"is using", c:"are use"}, correctAnswer:"a" },

  { type:"mc", question:"7. She ............... her laptop to the internet through a mobile hotspot at the moment.",
    answers:{a:"connect", b:"is connecting", c:"connects"}, correctAnswer:"b" },

  { type:"mc", question:"8. Right now, the system ............... for updates to install the latest version.",
    answers:{a:"is checking", b:"checks", c:"checking"}, correctAnswer:"a" },

  { type:"mc", question:"9. My computer ................ slowly because too many programs are running in the background.",
    answers:{a:"is running", b:"running", c:"works"}, correctAnswer:"a" },

  { type:"mc", question:"10. The user usually .......... the browser before checking emails or visiting websites.",
    answers:{a:"is opening", b:"opens", c:"open"}, correctAnswer:"b" },

  { type:"mc", 
    question:"11. Why are updates important?", 
    passage:`Updates to operating systems and web browsers fix bugs, improve performance, and strengthen security. These regular updates may also include useful new features or tools and help protect your device from malware, viruses, or hackers. If you ignore updates, your system might become slow, outdated, or unsafe. Most modern devices allow automatic updates to save time and effort.`,
    answers:{a:"They make the device slower",
             b:"They fix bugs and improve security.",
             c:"They delete your files"},
    correctAnswer:"b" },

  { type:"mc", 
    question:"12. What is one purpose of cookies?", 
    passage:`Cookies are small files that websites store on your computer to collect and remember information. They help websites remember user settings, login details, and shopping cart items. While cookies can be helpful for a smoother browsing experience, having too many may slow down your browser. You can delete cookies anytime from your browser's settings menu to improve speed and performance.`,
    answers:{a:"To upgrade the browser", b:"To store user settings", c:"To increase download speed. lưu ý kiểm tra lại là C mới đúng"},
    correctAnswer:"b" },

  { type:"mc", 
    question:"13. Why is multitasking useful?", 
    passage:`An operating system that supports multitasking lets you run several programs at the same time. For example, you can listen to music, write a document, and browse the internet without closing any of them. Without multitasking, only one application could run at a time, which slows down productivity.`,
    answers:{a:"It increases screen brightness.", b:"It allows many programs to run at once.", c:"It makes the computer smaller."},
    correctAnswer:"b" },

  { type:"mc", 
    question:"14. What does private browsing do?", 
    passage:`Internet browsing is done through applications called web browsers. These include Chrome, Firefox, Safari, and Edge. Browsers allow users to visit websites, watch videos, and download files. They also save passwords and browsing history if allowed. To protect privacy, users can enable private browsing, which does not store history or cookies.`,
    answers:{a:"It stores passwords", b:"It does not store history or cookies", c:"It saves browsing history"},
    correctAnswer:"b" },

  { type:"mc", 
    question:"15. What does an operating system help manage?", 
    passage:`Operating systems control the basic functions of a computer. They manage hardware like the CPU, memory, and storage devices. Without an operating system, a computer cannot run programs or communicate with connected devices. Windows, macOS, and Linux are examples of operating systems used on personal computers and laptops.`,
    answers:{a:"Hardware and basic functions.", b:"Only software.", c:"Only the display screen."},
    correctAnswer:"a" },

  { type:"mc", 
    question:"16. A computer can work properly without an operating system. (True/False)", 
    passage:`Operating systems provide a platform for running software and managing hardware devices like printers, keyboards, and storage drives. Without an operating system, most computers cannot function. Operating systems also offer tools for file management and security. Users often choose an OS based on personal needs and the type of software they plan to use.`,
    answers:{a:"True", b:"False"}, correctAnswer:"b" },

  { type:"mc", 
    question:"17. Built-in security features in modern operating systems protect them against online threats. (True/False)", 
    passage:`Most modern operating systems come with built-in security features such as firewalls and automatic updates. These tools help protect the system from viruses, malware, and other online threats. Users should keep these features enabled and regularly update their system to ensure the highest level of protection.`,
    answers:{a:"True", b:"False"}, correctAnswer:"a" },

  { type:"fill", 
    question:"18. Summary: A search engine finds websites based on keywords and is an important tool for ........................ and online research.", 
    passage:`A search engine helps users find information online by showing a list of websites that match the keywords entered. Google, Bing, and Yahoo are some of the most popular search engines. Using clear and specific keywords gives better results. Search engines are essential tools for internet browsing and learning, making it easier and faster to access knowledge anytime.`,
    answer:"internet browsing" },

  { type:"fill", 
    question:"19. Summary: Updating the operating system helps improve security, fixes bugs, updates new features and makes the system work ..............", 
    passage:`An operating system update improves the security and performance of a computer. Updates may include bug fixes, new features, or protection against the latest viruses and threats. Keeping your OS up to date ensures the system runs efficiently and safely. Most systems can be set to update automatically in the background.`,
    answer:"efficiently and safely" },

  { type:"fill", 
    question:"20. Summary: Private browsing prevents the browser from saving history or cookies, helping users protect their ........... on shared devices.", 
    passage:`Most internet browsers offer a feature called private browsing or incognito mode. When using this mode, the browser does not save your history, cookies, or login details. This is useful when using a shared computer or when users want to keep their browsing activity private. However, private browsing does not make users completely anonymous online.`,
    answer:"browsing activity" },

  { type:"fill", question:"21. The app runs in the background ......................... manage battery usage. (help)", answer:"to help" },

  { type:"fill", question:"22. You should restart the computer .................... the installation. (complete)", answer:"in order to complete" },

  { type:"fill", question:"23. He is clearing the browser cache ............... faster. (website/load)", answer:"so that websites load" },

  { type:"fill", question:"24. The technician is installing a new firewall ........ a system. (protect)", answer:"to protect" },

  { type:"fill", question:"25. The user updates the browser regularly ........ safe online. (stay)", answer:"to stay" },

 { type:"order", 
  question:"26. Arrange the words to make a correct sentence.", 
  correctSentence:"The user is clicking on the link to open the settings page.", 
  tokens:["The user","is","clicking","on the","link","to","open","the settings page."] },

{ type:"order", 
  question:"27. Arrange the words to make a correct sentence.", 
  correctSentence:"The browser is loading several tabs with video content right now.", 
  tokens:["The browser","is loading","several tabs","with video","content","right now."] },

{ type:"order", 
  question:"28. Arrange the words to make a correct sentence.", 
  correctSentence:"Browsers store data so that websites load faster during future visits.", 
  tokens:["Browsers store","data so that","websites load","faster during","future visits."] },

{ type:"order", 
  question:"29. Arrange the words to make a correct sentence.", 
  correctSentence:"The system usually installs updates automatically when a new version is available.", 
  tokens:["The system","usually","installs updates automatically","when","a new version","is available."] },

{ type:"order", 
  question:"30. Arrange the words to make a correct sentence.", 
  correctSentence:"Most computers use operating systems to support multiple users and applications.", 
  tokens:["Most computers","use operating","systems to","support multiple","users and applications."] },
],


  "BÀI 3":[
  { type:"mc", question:"1. Máy tính bảng của tôi không thể kết nối với mạng Bluetooth.",
    answers:{a:"My tablet cannot connect to the Bluetooth network.", b:"My laptop connects to Wi-Fi faster than it does to Ethernet cables.", c:"My phone doesn't need Wi-Fi because it uses Bluetooth for everything."},
    correctAnswer:"a" },

  { type:"mc", question:"2. Máy tính xách tay của anh ấy mất nhiều thời gian để kết nối với mạng không dây.",
    answers:{a:"His tablet instantly connects to the internet via mobile data.", b:"His desktop doesn't support any kind of network connection.", c:"His laptop takes longer to connect to the wireless network."},
    correctAnswer:"c" },

  { type:"mc", question:"3. Bạn nên sử dụng dữ liệu di động nếu không có Wi-Fi.",
    answers:{a:"You should use your mobile data when there is no available Wi-Fi.", b:"You might need to charge the phone before using mobile data again.", c:"You must switch off mobile data whenever you see a public Wi-Fi."},
    correctAnswer:"a" },

  { type:"mc", question:"4. Mạng Wi-Fi ở đây rất yếu và hay bị mất kết nối.",
    answers:{a:"The Wi-Fi connection in this place is weak and often gets disconnected.", b:"The mobile data is stronger than the Wi-Fi in this building.", c:"The Wi-Fi network here is extremely fast and never gets disconnected."},
    correctAnswer:"a" },

  { type:"mc", question:"5. Tớ không thể gửi tin nhắn vì không có tín hiệu di động.",
    answers:{a:"I can't send any text messages because there is no mobile signal.", b:"I can't open mobile apps because my phone has no storage left.", c:"I can't read the messages because my phone battery is fully charged."},
    correctAnswer:"a" },

  { type:"mc", question:"6. This is the application .............. allows users to share files over mobile networks.",
    answers:{a:"who", b:"where", c:"which"},
    correctAnswer:"c" },

  { type:"mc", question:"7. I know an engineer .............. designs network systems for mobile service providers.",
    answers:{a:"where", b:"who", c:"which"},
    correctAnswer:"b" },

  { type:"mc", question:"8. We installed a system .............. connects all smartphones to a secured local network.",
    answers:{a:"that", b:"where", c:"who"},
    correctAnswer:"a" },

  { type:"mc", question:"9. The technician ……………… helped us fix the internet issue was very professional.",
    answers:{a:"which", b:"where", c:"who"},
    correctAnswer:"c" },

  { type:"mc", question:"10. What can affect Wi-Fi signal strength?",
    passage:`Wi-Fi is a wireless network technology that connects devices to the internet without cables. Most homes and offices have a Wi-Fi router that sends signals to smartphones, laptops, and other devices. The signal strength depends on the distance from the router and walls or obstacles. A weak signal can cause slow connections or lost internet access.`,
    answers:{a:"The number of apps installed.", b:"The size of the hard drive.", c:"The distance from the router and obstacles."},
    correctAnswer:"c" },

  { type:"mc", question:"11. What is one disadvantage of using a mobile hotspot?",
    passage:`A hotspot is a physical location where people can access the internet through Wi-Fi. Some smartphones can also create personal hotspots by sharing mobile data. Hotspots are useful when Wi-Fi is not available. However, using mobile data as a hotspot can use a lot of data quickly and drain battery power fast.`,
    answers:{a:"It improves battery life.", b:"It cannot connect other devices.", c:"It uses a lot of data and battery."},
    correctAnswer:"c" },

  { type:"mc", question:"12. Why is mobile computing useful?",
    passage:`Mobile computing allows people to use computing devices while moving. Smartphones, tablets, and laptops are examples of mobile devices. These devices connect to networks using Wi-Fi, mobile data (3G/4G/5G), or Bluetooth. Mobile computing helps users send emails, attend online meetings, or browse the internet from almost anywhere. This makes work and communication more flexible.`,
    answers:{a:"It only works with desktop computers.", b:"It limits users to work in one place.", c:"It allows communication from almost anywhere."},
    correctAnswer:"c" },

  { type:"mc", question:"13. What type of network is not commonly used in homes and public places?",
    passage:`A computer network is a group of connected devices that can communicate with each other. These devices include computers, printers, phones, and servers. Networks can be wired, using cables, or wireless, using radio signals. Wireless networks like Wi-Fi are common in homes and public places. Networks allow users to share files, access the internet, and use printers or servers from different devices.`,
    answers:{a:"Satellite network", b:"Wired network", c:"Wireless network like Wi-Fi"},
    correctAnswer:"b" },

  { type:"mc", question:"14. What does Bluetooth help with?",
    passage:`Bluetooth is a wireless technology that allows short-range communication between devices. You can use Bluetooth to connect headphones, speakers, or share files between phones. Unlike Wi-Fi, Bluetooth works over shorter distances and uses less power. It is ideal for simple tasks like playing music or transferring small files without needing internet access.`,
    answers:{a:"Connecting nearby devices wirelessly", b:"High-speed internet connections", c:"Sharing large files online"},
    correctAnswer:"a" },

  { type:"mc", question:"15. What does an operating system help manage?",
    passage:`Operating systems control the basic functions of a computer. They manage hardware like the CPU, memory, and storage devices. Without an operating system, a computer cannot run programs or communicate with connected devices. Windows, macOS, and Linux are examples of operating systems used on personal computers and laptops.`,
    answers:{a:"Only software", b:"Hardware and basic functions", c:"Only the display screen"},
    correctAnswer:"b" },

  { type:"mc", question:"16. Wireless networks always provide strong signal strength, even if the distance is near or far. (True/False)",
    passage:`A wireless network allows devices to connect to the internet without using cables. It uses radio waves to send signals between devices and a router. Wi-Fi is the most common type of wireless network found in homes, schools, and offices. Wireless networks offer flexibility and convenience, but signal strength can drop if there are walls or distance between the device and router.`,
    answers:{a:"True", b:"False", c:"Not sure"},
    correctAnswer:"b" },

  { type:"mc", question:"17. Mobile data is unlimited for all users by default. (True/False)",
    passage:`Mobile data allows users to connect to the internet without Wi-Fi by using a cellular network. It is commonly used on smartphones and tablets. People use mobile data to browse websites, watch videos, and send emails on the go. However, streaming or downloading large files can use up data quickly, especially if the user has a limited data plan.`,
    answers:{a:"True", b:"False", c:"Not sure"},
    correctAnswer:"b" },

  { type:"fill", question:"18. Summary: Networks allow devices to share data and access the internet. They can ...................., depending on the connection type.",
    passage:`A computer network is a group of devices connected together to share data, files, and various resources. These devices include computers, printers, smartphones, tablets, and servers. Networks can be either wired, using physical cables, or wireless, using radio signals such as Wi-Fi. They are essential for communication, collaboration, internet access, and smooth operation within homes, schools, and workplaces.`,
    answer:"be wired or wireless" },

  { type:"fill", question:"19. Summary: Wi-Fi connects devices without cables and is widely used in homes and businesses. ...................... depends on distance and obstacles.",
    passage:`Wi-Fi is a wireless networking technology that uses radio signals to connect devices such as laptops, smartphones, and tablets to the internet. Most homes, offices, schools, and public places use Wi-Fi routers to provide internet access. However, signal strength can become weaker in areas far from the router or when blocked by thick walls, furniture, or electronic devices.`,
    answer:"Signal strength" },

  { type:"fill", question:"20. Summary: Mobile computing uses wireless technologies to connect devices and allows users to work from .................",
    passage:`Mobile computing allows people to use computers and access important data while on the move. Devices such as laptops, smartphones, and tablets connect to networks through Wi-Fi, mobile data, or Bluetooth. This flexibility gives users the freedom to work, study, browse the internet, or communicate with others from nearly any location, whether at home, outdoors, or while traveling.`,
    answer:"nearly any location" },

  { type:"fill", question:"21. The technician ................................ fixed the Wi-Fi in less than an hour. (come/yesterday)",
    answer:"who came yesterday" },

  { type:"fill", question:"22. The student .................... how computer networks work spoke very clearly and confidently. (explain)",
    answer:"who explained" },

  { type:"fill", question:"23. The laptop .................... strong wireless adapter connects to the network easily. (have)",
    answer:["which has a","that has a"] },

  { type:"fill", question:"24. The new modem.............  provides a faster internet connection. (install/you)",
    answer:["which you installed","that you installed"] },

  { type:"fill", question:"25. The app........... mobile data is useful when traveling abroad. (monitor)",
    answer:"which monitors" },

  { type:"order", question:"26. Sắp xếp để thành câu hoàn chỉnh:", 
    correctSentence:"I need to replace the old modem that keeps losing the connection.", 
    tokens:["I need","to replace","the old","modem that","keeps losing","the connection."] },

  { type:"order", question:"27. Sắp xếp để thành câu hoàn chỉnh:", 
    correctSentence:"Devices that are connected to a network can share files and printers.", 
    tokens:["Devices","that are","connected to","a network","can share files","and printers."] },

  { type:"order", question:"28. Sắp xếp để thành câu hoàn chỉnh:", 
    correctSentence:"The smartphone which you paired with Bluetooth speakers is working very well.", 
    tokens:["The smartphone","which","you paired","with Bluetooth","speakers is","working","very well."] },

  { type:"order", question:"29. Sắp xếp để thành câu hoàn chỉnh:", 
    correctSentence:"A wireless hotspot is a location that provides internet access without using cables.", 
    tokens:["A wireless","hotspot","is a location","that provides","internet access","without using cables."] },

  { type:"order", question:"30. Sắp xếp để thành câu hoàn chỉnh:", 
    correctSentence:"The technician who fixed the router also updated the firewall settings today.", 
    tokens:["The technician","who","fixed the router","also updated","the firewall","settings today."] }
],

"BÀI 4":[
  // 1-4: MC cơ bản
  { type:"mc", question:"1. Hệ quản trị cơ sở dữ liệu giúp bạn lưu trữ và quản lý dữ liệu dễ dàng.",
    answers:{a:"The database management system helps you store and manage data easily.", b:"The primary key helps you store and manage data easily.", c:"The database form helps you store and manage data easily."},
    correctAnswer:"a" },

  { type:"mc", question:"2. Một báo cáo có thể hiển thị dữ liệu từ nhiều bảng cùng lúc.",
    answers:{a:"A query can display data from many tables at the same time.", b:"A record can display data from many tables at the same time.", c:"A report can display data from many tables at the same time."},
    correctAnswer:"c" },

  { type:"mc", question:"3. Mỗi bản ghi chứa nhiều trường thông tin khác nhau.",
    answers:{a:"Each report contains many different tables.", b:"Each field contains many different records.", c:"Each record contains many different fields."},
    correctAnswer:"c" },

  { type:"mc", question:"4. Dữ liệu có thể được tìm kiếm nhanh chóng bằng cách sử dụng chỉ mục.",
    answers:{a:"Data can be recorded quickly by using an index.", b:"Data can be retrieved quickly by using an index.", c:"Data can be deleted quickly by using a field."},
    correctAnswer:"b" },

  // 5-8: MC ngữ pháp
  { type:"mc", 
  question:"5. A: How can we make the data easier to understand and work with? B: We can do that ................... it into tables.", 
  answers:{a:"putting", b:"by putting", c:"by puting"}, 
  correctAnswer:"b" },


  { type:"mc", question:"6. How does the system find a record so quickly? It finds it ................... the primary key.",
    answers:{a:"for using", b:"to using", c:"by using"},
    correctAnswer:"c" },

  { type:"mc", 
  question:"7. A: What's the best way to get accurate results from the database? B: ... correct fields and table, of course.", 
  answers:{a:"By selecting a", b:"To select a", c:"By selecting the"}, 
  correctAnswer:"c" },


  { type:"mc", question:"8. They improved report accuracy ____________ a unique record for each object.",
    answers:{a:"by creating", b:"in creating", c:"by create"},
    correctAnswer:"a" },

  // 9-13: MC reading có passage
  { type:"mc", question:"9. What is one advantage of a relational database?",
    passage:`Many companies use relational databases because they help connect information across tables. For example, an employee table can link to a department table using a shared ID number. This makes it faster to find related records and reduces data entry mistakes. Relational databases are more flexible than simple flat files. They also allow businesses to manage complex data relationships more efficiently and securely.`,
    answers:{a:"It requires less memory.", b:"It only stores names and numbers.", c:"It is used to connect information across tables."},
    correctAnswer:"c" },

  { type:"mc", question:"10. How are relational databases organized?",
    passage:`Relational databases are the most common type of database used today. They store data in tables that are linked by relationships. For example, a customer table can be connected to an orders table through a customer ID. This structure makes it easy to find related information and reduce duplication. Popular relational database systems include MySQL, PostgreSQL, and Oracle Database.`,
    answers:{a:"They use tables connected by relationships.", b:"They store data in a single list.", c:"They arrange data randomly."},
    correctAnswer:"a" },

  { type:"mc", question:"11. What is one role of a database administrator?",
    passage:`Database administrators, or DBAs, are responsible for maintaining databases. They make sure the data is secure, backed up, and available to users when needed. DBAs also monitor performance and fix any problems. Without their work, companies could lose important data or experience slow systems. In addition, DBAs help plan for future growth and ensure the system meets business needs.`,
    answers:{a:"Designing video games", b:"Selling databases to customers", c:"Keeping data secure and available"},
    correctAnswer:"c" },

  { type:"mc", question:"12. What is one function of a DBMS?",
    passage:`A database management system (DBMS) is software that helps users create, maintain, and control databases. It provides tools for adding new records, updating existing data, and generating reports. Security features in a DBMS protect sensitive information by controlling user access. Some DBMS programs are open-source, while others require a license. Choosing the right DBMS depends on the size of the data and the organization's needs.`,
    answers:{a:"Designing websites", b:"Printing documents automatically", c:"Controlling access to sensitive data"},
    correctAnswer:"c" },

  { type:"mc", question:"13. What is one advantage of a database over a spreadsheet?",
    passage:`A database is an organized collection of information that can be easily accessed, managed, and updated. Businesses use databases to store customer records, inventory data, and sales transactions. Unlike spreadsheets, databases can handle large amounts of information and support multiple users at the same time. Most databases use a language called SQL, which allows users to create queries to retrieve specific data quickly and accurately.`,
    answers:{a:"It requires no special language to operate.", b:"It can manage large data and support many users.", c:"It is easier to print data."},
    correctAnswer:"b" },

  // 14-16: Fill summary
  { type:"fill", question:"14. Summary: Cloud databases are accessed online and hosted on.......... They reduce costs, improve security, and offer scalability.",
    passage:`Cloud databases are stored on remote servers that you can access through the internet. They offer scalability and save costs because companies don't need to buy hardware. Cloud databases also improve performance and security. Examples are Amazon RDS, Microsoft Azure SQL, and Google Cloud SQL. In addition, they support automatic backups, easy updates, and flexible storage options, making them ideal for businesses of all sizes and industries.`,
    answer:"remote servers" },

  { type:"fill", question:"15. Summary: Relational databases use tables which have ................... to keep data organized and can be managed easily with SQL commands.",
    passage:`Relational databases arrange data into tables made of rows and columns. Each row represents a single record, and each column stores one type of data. People use SQL to search and update these tables. Relational databases are popular because they are reliable and simple to organize for many applications. They are widely used in business, education, healthcare, and government systems.`,
    answer:"rows and columns" },

  { type:"fill", question:"16. Summary: A database stores organized information for easy access and management. Companies use it to track customers, products, and transactions. ................... systems software helps manage, update, and protect data efficiently.",
    passage:`A database is a system that stores information in an organized way. Companies use databases to keep records of customers, products, and transactions. Modern databases are managed by software called Database Management Systems (DBMS). This software such as MySQL, Oracle, and Microsoft SQL Server helps users add, edit, or delete data quickly. These systems also support data analysis, allow multiple users to work at once, and help prevent data loss or errors.`,
    answer:"Database management" },

  // 17-19: True/False với passage
  { type:"mc", question:"17. Keys are used to link related data between tables. (True/False)",
    passage:`Relational databases use tables to organize information. Each table has columns for different fields and rows for records. Tables can be connected using keys, which link related data. This design allows users to find information quickly and see how different pieces of data are related. It also helps reduce duplication and supports more accurate, efficient data management in large systems.`,
    answers:{a:"False", b:"True"},
    correctAnswer:"b" },

  { type:"mc", question:"18. Cloud databases can be accessed only from the office. (True/False)",
    passage:`Cloud databases allow companies to store information online instead of on local servers. This means employees can access data from anywhere with an internet connection. Cloud services also provide automatic updates and backups, reducing the work needed to maintain the system. Moreover, cloud databases can easily scale as business needs grow and often offer better security features.`,
    answers:{a:"True", b:"False"},
    correctAnswer:"b" },

  { type:"mc", question:"19. Digital databases are usually slower to search than paper records. (True/False)",
    passage:`A database is an organized collection of data that can be easily accessed and managed. Many companies use databases to track sales, store customer information, and monitor inventory. Compared to paper records, digital databases are faster to update and search, making daily work more efficient. They also support data analysis, improve accuracy, and help teams make better business decisions.`,
    answers:{a:"True", b:"False"},
    correctAnswer:"b" },

  // 20-25: Fill
  { type:"fill", question:"20. She keeps the records organized…………clear categories. (by/create)", answer:"by creating" },
  { type:"fill", question:"21. I created a better layout ……………...... into useful forms and tables. (by / organize / data)", answer:"by organizing data" },
  { type:"fill", question:"22. We reduced mistakes in our reports ................... before saving them. (by / check / values)", answer:"by checking values" },
  { type:"fill", question:"23. We can avoid duplicate records .......... key for each entry. (by / use)", answer:"by using a unique" },
  { type:"fill", question:"24. They keep the data safe .......... information stored in the system. ( by / encrypt / sensitive)", answer:"by encrypting the sensitive" },
  { type:"fill", question:"25. We solved the problem quickly .................... the database. (by/query)", answer:"by querying" },

  // 26-30: Order
  { type:"order", question:"26. Sắp xếp để thành câu hoàn chỉnh:",
    correctSentence:"You can protect your data by backing up regularly.",
    tokens:["You","can","protect","your","data","by","backing","up","regularly."] },

  { type:"order", question:"27. Sắp xếp để thành câu hoàn chỉnh:",
    correctSentence:"Teams can improve their information sharing by using video conferences.",
    tokens:["Teams","can","improve","their information","sharing","by","using","video","conferences."] },

  { type:"order", question:"28. Sắp xếp để thành câu hoàn chỉnh:",
    correctSentence:"If the manager reviews the report, he can make better decisions.",
    tokens:["If","the","manager","reviews","the report,","he","can","make","better decisions."] },

  { type:"order", question:"29. Sắp xếp để thành câu hoàn chỉnh:",
    correctSentence:"You will need to create a report to summarize monthly sales.",
    tokens:["You","will","need","to create a","report","to","summarize","monthly sales."] },

  { type:"order", question:"30. Sắp xếp để thành câu hoàn chỉnh:",
    correctSentence:"They use different forms to create data entry easily and efficiently.",
    tokens:["They","use","different forms","to","create","data entry","easily and","efficiently."] }
],

"BÀI 5":[
  { type:"mc", question:"1. Màn hình độ phân giải cao rất cần thiết để xem rõ hình ảnh.",
    answers:{a:"High-definition microphones create images clearly.",
             b:"Low-definition monitors are essential for clear images.",
             c:"High-definition monitors are essential to see images clearly."},
    correctAnswer:"c" },

  { type:"mc", question:"2. Cuộc họp video giúp mọi người cộng tác dễ dàng hơn.",
    answers:{a:"Video meetings help people collaborate more easily.",
             b:"Video meetings don't help people work together easily.",
             c:"Video meetings reduce the number of participants easily."},
    correctAnswer:"a" },

  { type:"mc", question:"3. Người tham gia cuộc họp nên tắt micro khi không nói.",
    answers:{a:"Participants should turn on the microphone when not speaking.",
             b:"Participants should mute their microphone when not speaking.",
             c:"Participants should leave the meeting when not speaking."},
    correctAnswer:"b" },

  { type:"mc", question:"4. Internet tốc độ cao giúp cải thiện chất lượng cuộc gọi.",
    answers:{a:"Low-speed internet improves security.",
             b:"High-speed internet helps improve call quality.",
             c:"High-speed internet increases delays."},
    correctAnswer:"b" },

  { type:"mc", question:"5. If she ................... a better microphone, her voice would be clearer.",
    answers:{a:"had", b:"has", c:"have"}, correctAnswer:"a" },

  { type:"mc", question:"6. Their team ................... remotely every week if they ................... conferencing technology.",
    answers:{a:"would meet/ use", b:"would meet/ used", c:"met/ would use"},
    correctAnswer:"b" },

  { type:"mc", question:"7. A: How would things change if we ___________ better screen sharing tools? B: We ___________ data and charts more clearly in our presentations.",
    answers:{a:"use/ will see", b:"used/ would see", c:"used/ saw"},
    correctAnswer:"b" },

  { type:"mc", question:"8. A: If the company ................. in better equipment, meetings would be more effective. B: That's right.",
    answers:{a:"Invested", b:"invests", c:"invest"}, correctAnswer:"a" },

  { type:"mc", question:"9. A: What would you do if your conferencing platform suddenly stopped working? B: I ................. the IT team to fix the issue quickly.",
    answers:{a:"called", b:"call", c:"would call"}, correctAnswer:"c" },

  { type:"mc", 
    question:"10. Why do companies record video conferences?", 
    passage:`Many companies record video conferences so employees who cannot attend can watch later. These recordings are useful for training and reviewing important discussions. Privacy is important, so participants are usually informed when a session is being recorded. Some platforms automatically notify everyone with a message or sound alert at the start of recording.`,
    answers:{a:"To save internet bandwidth", b:"To let people review meetings later", c:"To punish employees"},
    correctAnswer:"b" },

  { type:"mc", 
    question:"11. What should you do before a video conference?", 
    passage:`Before joining a video conference, it is recommended to test your camera and microphone. Poor audio or video quality can make communication difficult. Participants should also check the lighting in their room so their faces are clear on camera. If there is background noise, using headphones or muting the microphone when not speaking can help. These steps make online meetings more professional and productive.`,
    answers:{a:"Make sure your lighting and audio work well", b:"Ignore camera and microphone settings", c:"Leave your microphone on all the time"},
    correctAnswer:"a" },

  { type:"mc", 
    question:"12. Why do people use virtual backgrounds in video calls?", 
    passage:`Some video conferencing software allows virtual backgrounds. This feature lets users display an image or blur their real background. It can help maintain privacy and create a more professional look. For example, a person working from home may use a company logo or a plain color as their background. However, virtual backgrounds may require more computer processing power and a good camera to work smoothly.`,
    answers:{a:"To improve internet speed", b:"To hide their real environment", c:"To reduce computer memory use"},
    correctAnswer:"b" },

  { type:"mc", 
    question:"13. What do you need to join a video conference?", 
    passage:`Video conferencing allows people to communicate in real time using audio and video over the internet. It is commonly used for remote meetings, interviews, and online classes. Participants can share their screens, send messages, and record sessions for future reference. To join a video conference, users usually need a webcam, microphone, and stable internet connection. Popular platforms include Zoom, Microsoft Teams, and Google Meet.`,
    answers:{a:"A printed invitation", b:"A webcam and internet connection", c:"A special computer with extra memory"},
    correctAnswer:"b" },

  { type:"mc", 
    question:"14. What is one advantage of video conferencing?", 
    passage:`One benefit of video conferencing is saving time and travel costs. Instead of flying to another city for a meeting, people can connect instantly online. This also helps reduce carbon emissions from transportation. However, video conferencing can have challenges like poor audio quality or unstable connections. To avoid problems, participants should test their equipment and internet before important calls.`,
    answers:{a:"It reduces the need to travel.", b:"It guarantees perfect audio every time.", c:"It always works without preparation."},
    correctAnswer:"a" },

  { type:"fill", 
    question:"15. Summary: A well-equipped room has good devices for meetings. It makes a professional environment and saves time, making ................... more effective.", 
    passage:`A high-quality video conferencing room is equipped with proper lighting, cameras and audio devices. It provides a professional environment and minimizes technical issues. Because everything is set up in advance, meetings can begin on time without delays caused by adjusting equipment. This setup also improves the communication quality and helps participants stay focused, making discussions more productive and efficient overall.`,
    answer:"the discussions" },

  { type:"fill", 
    question:"16. Summary: Security tools like ................... or waiting rooms keep meetings safe. They help protect sensitive information and important discussions.", 
    passage:`Many video conferencing platforms offer security features like passwords, encryption, and waiting rooms. These protect meetings from unauthorized access and help keep sensitive information safe. Organizations should use these features to maintain confidentiality during important discussions.`,
    answer:"passwords, encryption" },

  { type:"fill", 
    question:"17. Summary: Webcams add real-time video and build visual connection, ........ during discussions. It also makes professional communication more effective.", 
    passage:`Webcams make virtual meetings more personal by allowing participants to see each other in real time. This visual connection builds trust and increases engagement during discussions. Using a high-quality webcam also enhances image clarity, making professional communication more effective.`,
    answer:"increases engagement" },

  { type:"mc", 
    question:"18. Video conferencing is only used in the business sector. (True/False)", 
    passage:`Video conferencing helps people communicate face-to-face without being in the same room. It is widely used in business, education, and healthcare. Doctors can speak with patients remotely, and teachers can hold virtual classes. One major advantage is that it saves time and travel costs. However, users need a stable internet connection and compatible devices to participate effectively.`,
    answers:{a:"True", b:"False"}, correctAnswer:"b" },

  { type:"mc", 
    question:"19. Breakout rooms are used for smaller group discussions during video calls. (True/False)", 
    passage:`Many video conferencing tools offer features like screen sharing, chat, and breakout rooms. Screen sharing allows presenters to show slides or documents during meetings. Breakout rooms are useful for group discussions in online classes or workshops. These tools make virtual meetings more interactive and organized, helping participants stay engaged and collaborate better.`,
    answers:{a:"True", b:"False"}, correctAnswer:"a" },

  { type:"mc", 
    question:"20. It's better to sit with your back to a light source during a video call. (True/False)", 
    passage:`Good lighting and clear audio are important for professional video conferencing. People should sit facing a light source, like a window or lamp, so their face is visible. Background noise should be reduced, and participants should mute their microphones when not speaking. These simple steps improve communication and reduce distractions during meetings.`,
    answers:{a:"True", b:"False"}, correctAnswer:"b" },

  { type:"fill", question:"21. If I....... more money, I wouldn’t buy a new webcam. (not/have)", answer:"didn’t have" },

  { type:"fill", question:"22. If the internet is very slow and unstable, the video ................... (freeze)", answer:"would freeze" },

  { type:"fill", question:"23. A: How would things change if we ................... the system earlier? B: The video call wouldn't start right on time without delays. (not/set)", answer:"didn’t set up" },

  { type:"fill", question:"24. A: What .............................. we didn't record our meetings each week? B: We would miss important details. (if/ happen)", answer:"would happen if" },

  { type:"fill", question:"25. If you………. your microphone, no one will hear you. (not/check)", answer:"don’t check" },

  { type:"order", question:"26. What would you do if your conferencing platform suddenly stopped working?",
    correctSentence:"What would you do if your conferencing platform suddenly stopped working?",
    tokens:["What","would","you","do","if","your","conferencing","platform","suddenly stopped","working?"] },

  { type:"order", question:"27. If we had secure logins, our video conferencing would be more protected.",
    correctSentence:"If we had secure logins, our video conferencing would be more protected.",
    tokens:["If we","had","secure logins,","our video","conferencing","would","be more","protected."] },

  { type:"order", question:"28. If the company didn't train the team, they wouldn't use the control panel correctly.",
    correctSentence:"If the company didn't train the team, they wouldn't use the control panel correctly.",
    tokens:["If","the company","didn't","train","the team,","they","wouldn't","use","the control","panel","correctly."] },

  { type:"order", question:"29. If I knew how to share files, I would collaborate better with my remote team.",
    correctSentence:"If I knew how to share files, I would collaborate better with my remote team.",
    tokens:["If I","knew","how to","share files,","I","would","collaborate","better with","my remote","team."] },

  { type:"order", question:"30. If she were the manager, she would buy better equipment.",
    correctSentence:"If she were the manager, she would buy better equipment.",
    tokens:["If","she","were","the manager,","she would","buy","better","equipment."] }
],


"BÀI 6":[
  { type:"mc", question:"1. Nhà khoa học dữ liệu thu thập và xử lý thông tin từ dữ liệu lớn.",
    answers:{a:"The data scientist gathers and processes information from big data.",
             b:"The data scientist tests machine learning tools from big data.",
             c:"The data analyst stores customer profiles in a secure internal database."},
    correctAnswer:"a" },

  { type:"mc", question:"2. Nhà phân tích an ninh mạng bảo vệ hệ thống khỏi tấn công mạng.",
    answers:{a:"The cybersecurity analyst protects systems using internal security software.",
             b:"The cybersecurity analyst protects systems from cyberattacks.",
             c:"The cybersecurity analyst manages access and installs software on systems"},
    correctAnswer:"b" },

  { type:"mc", question:"3. Nhà phát triển phần mềm viết mã và tạo các ứng dụng hữu ích.",
    answers:{a:"The Al engineer designs useful tools for mobile security.",
             b:"The support technician helps users reset useful applications.",
             c:"The software developer writes code and creates useful applications."},
    correctAnswer:"c" },

  { type:"mc", question:"4. Nhà thiết kế giao diện tập trung vào trải nghiệm người dùng trên ứng dụng.",
    answers:{a:"The UI designer writes system code for mobile app displays.",
             b:"The UI designer tests user passwords during application login.",
             c:"The UI designer focuses on user experience in applications."},
    correctAnswer:"c" },

  { type:"mc", question:"5. Do they test the network manually or rely on automation……………?",
    answers:{a:"every time", b:"often", c:"always"},
    correctAnswer:"a" },

  { type:"mc", question:"6. Our team has project updates.................to keep everyone aligned.",
    answers:{a:"rarely", b:"once a week", c:"normally"},
    correctAnswer:"b" },

  { type:"mc", question:"7. …………….he forgets to document code, which causes confusion during testing.",
    answers:{a:"from time to time", b:"usually", c:"often"},
    correctAnswer:"a" },

  { type:"mc", question:"8. She……………joins the team call unless there's a system error to report.",
    answers:{a:"always", b:"every week", c:"hardly ever"},
    correctAnswer:"c" },

  { type:"mc", question:"9. I……………..check emails first in the morning to make the plan for the whole day.",
    answers:{a:"all the time", b:"always", c:"once a day"},
    correctAnswer:"b" },

  { type:"mc",
    question:"10. What does David do to prevent data issues from affecting company systems?",
    passage:`David works as a database administrator at a medical software company. Every morning, he logs into the system to check performance reports. He is responsible for designing databases that store important information, such as patient records and appointments. David also maintains databases by running regular updates and backups. From time to time, he works with software developers to test new features. If there is a problem with data access, David helps diagnose the problem quickly to avoid delays.`,
    answers:{a:"He maintains and backs up the databases",
             b:"He writes scripts to analyze user traffic",
             c:"He upgrades devices used by developers"},
    correctAnswer:"a" },

  { type:"mc",
    question:"11. How often does Amira evaluate her team's support work?",
    passage:`Amira is a helpdesk supervisor at a university IT department. She supervises a team of technical support people who assist staff and students. When someone has a problem with their computer, they contact Amira's team. She assigns the cases and checks that solutions are delivered on time. Amira often joins team calls to review performance and discuss common issues. Sometimes she trains new team members or writes short guides for solving simple problems.`,
    answers:{a:"Daily, by joining service calls",
             b:"Sometimes, during team performance reviews",
             c:"Often, through meetings and performance checks"},
    correctAnswer:"c" },

  { type:"mc",
    question:"12. What does Amira do to make sure users receive good support?",
    passage:`Amira is a helpdesk supervisor at a university IT department. She supervises a team of technical support people who assist staff and students. When someone has a problem with their computer, they contact Amira's team. She assigns the cases and checks that solutions are delivered on time. Amira often joins team calls to review performance and discuss common issues. Sometimes she trains new team members or writes short guides for solving simple problems.`,
    answers:{a:"She contacts users directly to solve every technical problem.",
             b:"She upgrades all software and hardware herself during the weekends.",
             c:"She creates simple instructions, manages her team, and tracks their support quality."},
    correctAnswer:"c" },

  { type:"mc",
    question:"13. Why is David's role important for the company's daily operations?",
    passage:`David works a database administrator at a medical software company. Every morning, he logs into the system to check performance reports. He is responsible for designing databases that store important information, such as patient records and appointments. David also maintains databases by running regular updates and backups. From time to time, he works with software developers to test new features. If there is a problem with data access, David helps diagnose the problem quickly to avoid delays.`,
    answers:{a:"He ensures data systems run well and helps fix data issues quickly.",
             b:"He manages the entire IT department and writes backup codes.",
             c:"He tests new medical equipment with doctors."},
    correctAnswer:"a" },

  { type:"mc",
    question:"14. What kind of problems does Anna mostly handle?",
    passage:`Anna works as an IT support technician in a busy office. She usually arrives at 8:00 a.m. and starts her day by checking support tickets. She normally handles hardware problems, but sometimes deals with software issues. Occasionally, she receives urgent calls from sales staff who are having problems during a client meeting. From time to time, Anna leads short training sessions to help employees learn new tools. She rarely visits other departments unless the issue requires her to be there physically.`,
    answers:{a:"network problems",
             b:"software crashes",
             c:"hardware issues"},
    correctAnswer:"c" },

  { type:"mc",
    question:"15. Hana installs new software and fixes broken computers when needed.",
    passage:`Hana works as a support technician at a small business. She looks after all the computers in the office and makes sure everything is working well. Each morning, she checks emails for reported issues. If someone’s computer isn’t working, she visits their desk to diagnose the problem. Hana also sets up new computers for new employees and installs software when needed. From time to time, she gives advice on how to avoid common tech problems.`,
    answers:{a:"True", b:"False"},
    correctAnswer:"a" },

  { type:"mc",
    question:"16. She writes software specifications so developers can build what clients need.",
    passage:`Sophie is a systems analyst at a company that builds software for businesses. When clients need a new system, Sophie visits them to understand their needs. She writes specifications for the software so the developers can build exactly what the client wants. Sophie often attends meetings with both the client and the software team. Sometimes she tests new software features to see if they match the requirements. She plays an important role in connecting business needs with technical solutions.`,
    answers:{a:"True", b:"False"},
    correctAnswer:"a" },

  { type:"mc",
    question:"17. Amira directly solves most of the support orders herself.",
    passage:`Amira is a helpdesk supervisor at a university IT department. She supervises a team of technical support people who assist staff and students. When someone has a problem with their computer, they contact Amira's team. She assigns the cases and checks that solutions are delivered on time. Amira often joins team calls to review performance and discuss common issues. Sometimes she trains new team members or writes short guides for solving simple problems.`,
    answers:{a:"True", b:"False"},
    correctAnswer:"b" },

  { type:"fill",
    question:"18. Summary: Hana is responsible for maintaining computers and supporting employees with tech issues. She installs software, sets up devices, and diagnoses problems on-site. She checks emails for reported issues and responds....................",
    passage:`Hana works as a support technician at a small business. She looks after all the computers in the office and makes sure everything is working well. Each morning, she checks emails for reported issues. If someone's computer isn't working, she visits their desk to diagnose the problem. Hana also sets up new computers for new employees and installs software when needed. From time to time, she gives advice on how to avoid common tech problems.`,
    answer:"when needed" },

  { type:"fill",
    question:"19. Summary: Freddy manages IT projects and reviews timelines and tasks every morning. He regularly meets clients and his team to solve problems or check progress. He also updates senior managers by writing…………….",
    passage:`Freddy is a project manager for an international software company. He is responsible for IT projects that involve building custom applications for clients. Freddy usually starts his day by reviewing project timelines and task lists. He meets with developers, designers, and clients to make sure everything is going as planned. If someone has a problem with deadlines or tools, Freddy helps solve it. He also writes reports to update senior managers on the team's progress.`,
    answer:"progress reports" },

  { type:"fill",
    question:"20. Summary: Sophie meets with clients to understand their software needs and writes detailed plans. These specifications help developers build the correct system. She also tests new features to check if they…………….",
    passage:`Sophie is a systems analyst at a company that builds software for businesses. When clients need a new system, Sophie visits them to understand their needs. She writes specifications for the software so the developers can build exactly what the client wants. Sophie often attends meetings with both the client and the software team. Sometimes she tests new software features to see if they match the requirements.`,
    answer:"match the requirements" },

  { type:"fill", question:"21. He………after he talks with the projectmanager. <b>( write / specifications / usually )</b>", answer:"usually writes the specifications" },

{ type:"fill", question:"22. Ahmed.....................all of our IT projects. <b>( responsible / always )</b>", answer:"is always responsible for" },

{ type:"fill", question:"23. We…………….unless we can't fix it ourselves. <b>( call/ support/ rarely )</b>", answer:"rarely call for support" },

{ type:"fill", question:"24. He…………in the morning before the office gets busy. <b>( write/ software/ usually )</b>", answer:"usually writes software" },

{ type:"fill", question:"25. She………every Monday to plan her weekly tasks. <b>( attend/ always/ meeting )</b>", answer:"always attends a meeting" },


  { type:"order",
    question:"26. Rearrange to make a complete sentence:",
    correctSentence:"We are responsible for our IT projects, so we rarely leave tasks unfinished.",
    tokens:["We","are responsible","for our","IT projects,","so we","rarely","leave","tasks","unfinished."] },

  { type:"order",
    question:"27. Rearrange to make a question:",
    correctSentence:"Does your team diagnose the problem quickly when users report system errors by email?",
    tokens:["Does","your team","diagnose the","problem quickly","when","users","report","system errors","by email?"] },

  { type:"order",
    question:"28. Rearrange to make a complete sentence:",
    correctSentence:"When I look after all the computers, I sometimes forget to update the antivirus.",
    tokens:["When","I look","after","all the","computers,","I sometimes","forget","to","update","the","antivirus."] },

  { type:"order",
    question:"29. Rearrange to make a complete sentence:",
    correctSentence:"Anna often sets up new computers when her company hires new staff.",
    tokens:["Anna often","sets","up","new","computers","when","her company","hires","new","staff."] },

  { type:"order",
    question:"30. Rearrange to make a complete sentence:",
    correctSentence:"I usually write software in the morning before attending the team meeting.",
    tokens:["I usually","write","software","in","the morning","before","attending","the team","meeting."] }
],

"BÀI 7":[
  { type:"mc", question:"1. CV của bạn nên trình bày rõ ràng kinh nghiệm làm việc.",
    answers:{a:"Your CV should focus only on personal hobbies.",
             b:"Your CV should be written in different colors.",
             c:"Your CV should clearly present work experience."},
    correctAnswer:"c" },

  { type:"mc", question:"2. Chúng ta có thể sử dụng trí tuệ nhân tạo để phân tích dữ liệu.",
    answers:{a:"We can use artificial intelligence to analyze data.",
             b:"We can send data without using artificial intelligence.",
             c:"We can use artificial intelligence in data science."},
    correctAnswer:"a" },

  { type:"mc", question:"3. Bạn nên nói về kĩ năng và điểm mạnh trong buổi phỏng vấn.",
    answers:{a:"You should talk about your address and ID card.",
             b:"You should talk about skills and strengths in the interview.",
             c:"You should talk about hobbies and pets in the interview."},
    correctAnswer:"b" },

  { type:"mc", question:"4. Tôi từng làm việc tại một công ty phần mềm nổi tiếng.",
    answers:{a:"I will work at a famous software company.",
             b:"I used to work at a famous software company.",
             c:"I work at a famous software company now."},
    correctAnswer:"b" },

  { type:"mc", question:"5. …………she ever…………a strong curriculum vitae for the internship program yet?",
    answers:{a:"Has/ written", b:"Has/ wrote", c:"Have/ written"},
    correctAnswer:"a" },

  { type:"mc", question:"6. ...............you…………..in teams to develop a university scheduling system?",
    answers:{a:"has/ used to work", b:"did/ use to work", c:"did/ used to work"},
    correctAnswer:"b" },

  { type:"mc", question:"7. He…………….in hackathons, but now he prefers solo projects.",
    answers:{a:"used to participate", b:"used to participating", c:"have participated"},
    correctAnswer:"a" },

  { type:"mc", question:"8. I………..send handwritten CVs, but now everything is online.",
    answers:{a:"was used", b:"used to", c:"have used"},
    correctAnswer:"b" },

  { type:"mc", question:"9. She…………as a support technician before becoming a developer.",
    answers:{a:"has work", b:"have worked", c:"has worked"},
    correctAnswer:"c" },

  { type:"mc", 
    question:"10. What does the intern's experience suggest about learning in real projects?", 
    passage:`Last summer, I worked as an intern Software Developer at a local tech startup. I was responsible for small coding tasks and duties such as writing backend functions and fixing bugs. I used to struggle with teamwork, but during the internship I have improved my communication skills. I also participated in daily meetings and project planning. This experience helped me understand my strengths in problem-solving and the importance of being open to feedback.`,
    answers:{a:"Technical skills are more important than communication",
             b:"Real work experiences help improve soft skills like teamwork",
             c:"Writing code alone is enough to succeed in a company"},
    correctAnswer:"b" },

  { type:"mc", 
    question:"11. When did Linh become more confident in her communication?", 
    passage:`Linh is applying for her first job as an IT Support Technician. She has written her curriculum vitae carefully, listing her skills in troubleshooting, customer service, and basic networking. She has taken part in several university IT projects and has always enjoyed helping friends fix computer problems. Although she used to be shy, she has become more confident by practicing interview questions with her classmates. She hopes her strengths will help her succeed in the job.`,
    answers:{a:"After working for two years in IT customer service",
             b:"After participating in interviews at several companies",
             c:"When she practiced interview questions with classmates"},
    correctAnswer:"c" },

  { type:"mc", 
    question:"12. How does InsightPro expect the candidate to support the company's goals?", 
    passage:`InsightPro is hiring a Data Analyst to join our expanding team. The ideal candidate must have strong analytical skills, attention to detail, and be able to work under pressure. Responsibilities include analyzing large data sets, creating visuals, and preparing reports for management. Prior experience with Python or R is preferred. Applicants must submit a curriculum vitae with relevant qualifications and examples of previous projects. This is a full-time role with competitive salary and performance bonuses.`,
    answers:{a:"By working with customers to improve sales strategies",
             b:"By preparing reports and creating tools to present data clearly",
             c:"By designing websites and managing social media platforms"},
    correctAnswer:"b" },

  { type:"mc", 
    question:"13. What challenge did the applicant face during the job application process?", 
    passage:`I have just completed my application for a junior Software Developer role at FutureTech. I wrote a detailed curriculum vitae, describing my qualifications, work experience, and skills. I mentioned that I have developed several mobile apps during university and taken part in open-source projects. The most difficult part was the salary negotiation, as I didn't have much experience with that. However, I explained my strengths and potential to grow. I hope to hear back soon and be invited to an interview.`,
    answers:{a:"Submitting a late curriculum vitae to the company",
             b:"Writing a cover letter for an foreign company",
             c:"Negotiating a salary without much prior experience"},
    correctAnswer:"c" },

  { type:"mc", 
    question:"14. What does Duy's part-time job as a help desk assistant show about him?", 
    passage:`Duy has recently been invited to an IT Support Technician job interview. He submitted his curriculum vitae last week and was selected as a strong candidate. During the interview, he was asked about his communication skills, how he handles technical issues, and whether he can work under pressure. Duy spoke confidently about his experience and strengths and even mentioned a past weakness-time management-which he has improved. He also shared that he used to work part-time as a help desk assistant while studying.`,
    answers:{a:"He lacks teamwork experience and prefers personal projects.",
             b:"He has real-world experience supporting users in technical environments.",
             c:"He prefers working independently without supervision"},
    correctAnswer:"b" },

  { type:"mc", 
    question:"15. Linh practiced interview questions with professionals at the company. (True/False)",
    passage:`Linh is applying for her first job as an IT Support Technician. She has written her curriculum vitae carefully, listing her skills in troubleshooting, customer service, and basic networking. She has taken part in several university IT projects and has always enjoyed helping friends fix computer problems. Although she used to be shy, she has become more confident by practicing interview questions with her classmates. She hopes her strengths will help her succeed in the job.`,
    answers:{a:"True", b:"False"}, correctAnswer:"b" },

  { type:"mc", 
    question:"16. The intern worked on bug fixes and backend code during the internship. (True/False)",
    passage:`Last summer, I worked as an intern Software Developer at a local tech startup. I was responsible for small coding tasks and duties such as writing backend functions and fixing bugs. I used to struggle with teamwork, but during the internship I have improved my communication skills. I also participated in daily meetings and project planning. This experience helped me understand my strengths in problem-solving and the importance of being open to feedback.`,
    answers:{a:"True", b:"False"}, correctAnswer:"a" },

  { type:"mc", 
    question:"17. The applicant has developed mobile apps and participated in open-source projects. (True/False)",
    passage:`I have just completed my application for a junior Software Developer role at FutureTech. I wrote a detailed curriculum vitae, describing my qualifications, work experience, and skills. I mentioned that I have developed several mobile apps during university and taken part in open-source projects. The most difficult part was the salary negotiation, as I didn't have much experience with that. However, I explained my strengths and potential to grow. I hope to hear back soon and be invited to an interview.`,
    answers:{a:"True", b:"False"}, correctAnswer:"a" },

  { type:"fill", 
    question:"18. Summary: The applicant included past development experience in their application and discussed the difficulty of....................., which he managed by explaining his strengths.", 
    passage:`I have just completed my application for a junior Software Developer role at FutureTech. I wrote a detailed curriculum vitae, describing my qualifications, work experience, and skills. I mentioned that I have developed several mobile apps during university and taken part in open-source projects. The most difficult part was the salary negotiation, as I didn't have much experience with that. However, I explained my strengths and potential to grow. I hope to hear back soon and be invited to an interview.`,
    answer:"salary negotiation" },

  { type:"fill", 
    question:"19. Summary: The writer completed a summer internship as a Software Developer at a tech startup. They handled various coding…………and gradually improved their teamwork and communication. The experience helped them recognize their strengths and the value of feedback.", 
    passage:`Last summer, I worked as an intern Software Developer at a local tech startup. I was responsible for small coding tasks and duties such as writing backend functions and fixing bugs. I used to struggle with teamwork, but during the internship I have improved my communication skills. I also participated in daily meetings and project planning. This experience helped me understand my strengths in problem-solving and the importance of being open to feedback.`,
    answer:"tasks and duties" },

  { type:"fill", 
    question:"20. Summary: Duy was selected for an interview and confidently discussed his background, including a …………….., which he has worked to overcome.", 
    passage:`Duy has recently been invited to an IT Support Technician job interview. He submitted his curriculum vitae last week and was selected as a strong candidate. During the interview, he was asked about his communication skills, how he handles technical issues, and whether he can work under pressure. Duy spoke confidently about his experience and strengths and even mentioned a past weakness-time management-which he has improved. He also shared that he used to work part-time as a help desk assistant while studying.`,
    answer:"past weakness-time" },

  { type:"fill", question:"21. Have they……………technical interviews since they graduated from the IT program? (take)", answer:"taken part in" },

  { type:"fill", question:"22. We.........................tasks and duties listed in a real IT job description. (never/ analyze/ have)", answer:"have never analyzed" },

  { type:"fill", question:"23. Ahmed…………….teamwork because he used to prefer working on his own. (learn/ recently)", answer:"has recently learned about" },

  { type:"fill", question:"24. I ………….. curriculum vitae for at least five different job positions. (already / write)", answer:"have already written a" },

  { type:"fill", question:"25. Anna ……….. group interviews, but now she enjoys speaking with other candidates. (not / use to / enjoy)", answer:"didn't use to enjoy" },

  { type:"order", question:"26. He used to ignore his weaknesses, but he has worked hard to improve them during training.",
    correctSentence:"He used to ignore his weaknesses, but he has worked hard to improve them during training.",
    tokens:["He","used","to ignore","his weaknesses,","but","he","has","worked","hard","to","improve","them","during training."] },

  { type:"order", question:"27. Ahmed has developed strong communication skills because he has worked on some group projects.",
    correctSentence:"Ahmed has developed strong communication skills because he has worked on some group projects.",
    tokens:["Ahmed","has","developed","strong communication","skills","because","he has","worked on","some group projects."] },

  { type:"order", question:"28. They used to study different majors, but now they all work as IT support technicians.",
    correctSentence:"They used to study different majors, but now they all work as IT support technicians.",
    tokens:["They","used","to","study","different majors,","but now","they all","work","as","IT support","technicians."] },

  { type:"order", question:"29. Have you ever taken part in a coding competition at your university or online?",
    correctSentence:"Have you ever taken part in a coding competition at your university or online?",
    tokens:["Have","you","ever","taken","part","in","a","coding","competition","at your","university","or online?"] },

  { type:"order", question:"30. I haven't written any detailed curriculum vitae to apply for a part-time internship position.",
    correctSentence:"I haven't written any detailed curriculum vitae to apply for a part-time internship position.",
    tokens:["I","haven't","written","any","detailed","curriculum","vitae","to apply","for","a","part-time","internship","position."] }
],



  "BÀI 8":[
    { type:"mc", question:"1. Nhân viên phải thay đổi mật khẩu của họ mỗi tháng một lần.",
      answers:{a:"Staff must change their passwords once a month.", b:"Staff can skip updating their passwords once a month.", c:"Staff should share passwords with others once a month."},
      correctAnswer:"a" },

    { type:"mc", question:"2. Phòng Nhân sự chịu trách nhiệm tuyển dụng và hỗ trợ nhân viên.",
      answers:{a:"The IT Department installs operating systems and firewalls.", b:"The Sales Department builds and repairs computer equipment.", c:"The H.R Department is responsible for hiring and supporting employees."},
      correctAnswer:"c" },

    { type:"mc", question:"3. Bạn không được phép kết nối thiết bị cá nhân vào máy tính văn phòng.",
      answers:{a:"You must not save your personal files on the office server.", b:"You must not connect personal phones to office computers.", c:"You must not connect personal devices to office computers."},
      correctAnswer:"c" },

    { type:"mc", question:"4. Bộ phận R&D phát triển sản phẩm mới và cải tiến công nghệ.",
      answers:{a:"The R&D Department works with new customers every day.", b:"The R&D Department develops new products and improves technology.", c:"The R&D Department arranges team meetings and interviews."},
      correctAnswer:"b" },

    { type:"mc", question:"5. Interns ________________ install software without permission from the person in charge of IT.",
      answers:{a:"don't have to", b:"mustn't", c:"have to"},
      correctAnswer:"b" },

    { type:"mc", question:"6. Staff in the Human Resources Department ________________ be responsible for hiring new employees.",
      answers:{a:"mustn't", b:"have to", c:"don't have to"},
      correctAnswer:"b" },

    { type:"mc", question:"7. Anna ______________ manage customer issues directly because she's in Customer Service.",
      answers:{a:"mustn't", b:"must", c:"doesn't have to"},
      correctAnswer:"b" },

    { type:"mc", question:"8. ............. follow IT Department rules to keep company networks secure.",
      answers:{a:"mustn't", b:"don't have to", c:"must"},
      correctAnswer:"c" },

    { type:"mc", question:"9. ........ employees ... all meetings organized by the Board of Directors?",
      answers:{a:"Do/ have attend", b:"Do/ have to attend", c:"Does/ have to attend"},
      correctAnswer:"b" },

    { type:"mc", question:"10. Why is the IT Department important at Panasonic Vietnam?",
      passage:`Panasonic Vietnam sells electronics through its Sales Department and promotes them via Marketing. The IT Department supports employees and protects data. Workers must follow rules, including not installing software. The Accounting Department records financial data, and Customer Services assists customers. Human Resources hires staff and manages training. The company must keep its operations secure and efficient.`,
      answers:{a:"It trains new staff and hires technical workers", b:"It supports staff and ensures systems are securely managed", c:"It helps the Accounting Department record company finances"},
      correctAnswer:"b" },

    { type:"mc", question:"11. How does Foxconn ensure workplace safety and communication across departments?",
      passage:`Foxconn has global factories producing electronics. The IT Department manages networks and keeps systems safe. The R&D Department researches production methods. Staff must wear ID badges and mustn't use personal devices. Human Resources hires and trains employees. The Board of Directors plans strategy. The Communication Department coordinates updates and supports internal communication across departments.`,
      answers:{a:"By selling products globally and training its sales team", b:"By promoting employee sales and increasing factory output", c:"By coordinating updates through Communication and enforcing IT rules"},
      correctAnswer:"c" },

    { type:"mc", question:"12. What must Canon employees do to follow company technology and teamwork policies?",
      passage:`Canon Vietnam has factories producing printers. The R&D Department develops new technology. Human Resources hires staff and coordinates training. The IT Department manages the network and ensures safety. Employees must follow strict rules and should attend all meetings. They also have to connect to secure networks. Marketing promotes products, while Communication coordinates messages. The person in charge of IT helps maintain secure operations.`,
      answers:{a:"Wear badges, attend meetings, and avoid system updates", b:"Follow safety rules, attend meetings, and use secure networks", c:"Use personal USB drives and skip communication meetings"},
      correctAnswer:"b" },

    { type:"mc", question:"13. What do Canon Vietnam employees have to do to follow company safety and teamwork rules?",
      passage:`Canon Vietnam has factories producing printers. The R&D Department develops new technology. Human Resources hires staff and coordinates training. The IT Department manages the network and ensures safety. Employees must follow strict rules and should attend all meetings. They also have to connect to secure networks. Marketing promotes products, while Communication coordinates messages. The person in charge of IT helps maintain secure operations.`,
      answers:{a:"Install marketing software and promote new products", b:"Skip meetings and use open wireless networks", c:"Connect to secure networks and attend scheduled meetings"},
      correctAnswer:"c" },

    { type:"mc", question:"14. What responsibilities does the R&D Department at Microsoft have?",
      passage:`Microsoft develops global software solutions. The R&D Department creates tools like AI and cloud platforms. IT Department supports systems and keeps data safe. Staff mustn't share passwords and should report problems. The Board of Directors manages strategy. Sales sells products to clients. Marketing promotes services worldwide. Customer Services assists millions of customers daily with technical support.`,
      answers:{a:"Selling services to international clients", b:"Assisting customers with product support", c:"Creating tools like AI and cloud platforms"},
      correctAnswer:"c" },

    { type:"mc", question:"15. Employees are expected to attend team meetings regularly.",
      passage:`Canon Vietnam has factories producing printers. The R&D Department develops new technology. Human Resources hires staff and coordinates training. The IT Department manages the network and ensures safety. Employees must follow strict rules and should attend all meetings.`,
      answers:{a:"True", b:"False"},
      correctAnswer:"a" },

    { type:"mc", question:"16. Employees must report system problems and avoid sharing passwords.",
      passage:`Microsoft develops global software solutions. The R&D Department creates tools like AI and cloud platforms. IT Department supports systems and keeps data safe. Staff mustn't share passwords and should report problems.`,
      answers:{a:"True", b:"False"},
      correctAnswer:"a" },

    { type:"mc", question:"17. Customer Services is responsible for managing company sales and advertising.",
      passage:`Panasonic Vietnam sells electronics through its Sales Department and promotes them via Marketing. The IT Department supports employees and protects data. Workers must follow rules, including not installing software. The Accounting Department records financial data, and Customer Services assists customers. Human Resources hires staff and manages training.`,
      answers:{a:"True", b:"False"},
      correctAnswer:"b" },

    { type:"fill", question:"18. Summary: Foxconn produces electronics in global factories and ensures system safety through its IT Department. Staff must follow security rules like wearing ID badges and not using ____________ . Communication supports internal messaging, while the Board guides overall company strategy.",
      passage:`Foxconn has global factories producing electronics for leading technology brands. The IT Department manages complex networks and ensures systems remain secure. The R&D Department researches advanced production methods. Employees must wear ID badges and mustn't use personal devices at work. Human Resources hires and trains staff. The Board of Directors sets company strategy, while Communication coordinates updates and supports internal communication.`,
      answer:"personal devices" },

    { type:"fill", question:"19. Samsung Electronics Vietnam produces electronics and has strict workplace rules. The Communication Department handles updates, and the ____________ ensures staff are trained for smooth factory operations.",
      passage:`Samsung Electronics Vietnam has large factories producing electronic devices for global markets. The Human Resources Department hires and trains staff to ensure efficient operations. Employees must follow safety rules and mustn't bring food near equipment. The Accounting Department records financial transactions, while Customer Services assists users. Marketing promotes new products, and Communication manages internal updates. IT supports staff and secures company networks daily.`,
      answer:"Human Resources Department" },

    { type:"fill", question:"20. Canon Vietnam produces printers in its factories. The IT Department ensures network safety and is supported by the ................ of IT. Employees follow strict rules, attend meetings, and use secure connections.",
      passage:`Canon Vietnam has factories producing printers. The R&D Department develops new technology. Human Resources hires staff and coordinates training. The IT Department manages the network and ensures safety. Employees must follow strict rules and should attend all meetings. They also have to connect to secure networks. Marketing promotes products, while Communication coordinates messages. The person in charge of information technology helps maintain secure operations.`,
      answer:"person in charge" },

    { type:"fill", question:"21. Anna _______________ quickly when she receives urgent updates from the Marketing Department. (have to/ coordinate)",
      answer:"has to coordinate" },

    { type:"fill", question:"22. Staff _______________ to maintain a good relationship in the IT workplace. (must/ support/ other)",
      answer:"must support each other" },

    { type:"fill", question:"23. ............... all financial transactions by the end of each working day. (not/ have to/ record)",
      answer:"doesn't have to record" },

    { type:"fill", 
  question:"24. Do we .................. when the client database is down? (have/support/ customers)", 
  answer:"have to support customers" },

    { type:"fill", question:"25. You _______________ to the Board of Directors when planning the company's IT projects to get useful directions. ( must/ report/ problem )",
      answer:"must report problems" },

    { type:"order", question:"26. Arrange to form a correct sentence:",
      correctSentence:"Do employees in the Sales Department have to record every transaction accurately and promptly?",
      tokens:["Do","employees in the","Sales Department","have to","record","every transaction accurately","and promptly?"] },

    { type:"order", question:"27. Arrange to form a correct sentence:",
      correctSentence:"Anna doesn't have to coordinate with the Communication Department to promote the new software products.",
      tokens:["Anna","doesn't","have to","coordinate with","the Communication Department","to promote","the new","software","products."] },

    { type:"order", question:"28. Arrange to form a correct sentence:",
      correctSentence:"Must he attend meetings regularly with the Board of Directors and present financial reports?",
      tokens:["Must","he","attend","meetings","regularly with","the Board","of Directors","and present","financial reports?"] },

    { type:"order", question:"29. Arrange to form a correct sentence:",
      correctSentence:"Do we have to support our colleagues in the R&D Department during new product research projects?",
      tokens:["Do","we","have to","support","our colleagues","in the R&D","Department","during","new product","research projects?"] },

    { type:"order", question:"30. Arrange to form a correct sentence:",
      correctSentence:"You mustn't share your password with people you work with because of IT security regulations.",
      tokens:["You","mustn't","share","your password","with","people you","work with","because of","IT security","regulations."] }
  ],

  "BÀI 9":[
  { type:"mc", question:"1. Học sâu và mạng nơ-ron giúp máy hiểu hình ảnh phức tạp.",
    answers:{a:"Deep learning and neural networks help machines understand complex images.",
             b:"Deep learning and neural networks stop machines from learning new images.",
             c:"Deep learning and neural networks clean the images before using them."},
    correctAnswer:"a" },

  { type:"mc", question:"2. Tự động hóa giúp giảm công việc thủ công và cải thiện thuật toán.",
    answers:{a:"Automation helps people walk and talk better with robots.",
             b:"Automation writes long emails and sends them to the boss.",
             c:"Automation reduces manual work and improves the algorithm."},
    correctAnswer:"c" },

  { type:"mc", question:"3. ………….we……………the algorithm before starting the next test?",
    answers:{a:"Shall/ improving", b:"Shall/ improve", c:"What about/ improving"},
    correctAnswer:"b" },

  { type:"mc", question:"4. …………..we…………….unsupervised learning for this task first?",
    answers:{a:"Could/ try", b:"What about/ trying", c:"Could/ trying"},
    correctAnswer:"a" },

  { type:"mc", question:"5. How about……………..neural networks for better image recognition?",
    answers:{a:"using", b:"to use", c:"used"},
    correctAnswer:"a" },

  { type:"mc", question:"6. What is one suggested use of machine learning in the passage?",
    passage:`Machine learning is a powerful method where systems learn from data instead of being manually programmed. It uses supervised or unsupervised learning, algorithms, and models to make predictions. Neural networks improve their performance, but overfitting remains a concern. How about using machine learning to personalize user experiences in apps and websites by analyzing behavior and adjusting content in real time?`,
    answers:{a:"To help users write code more efficiently.",
             b:"To replace manual programming in all business software.",
             c:"To personalize app and website content based on user behavior."},
    correctAnswer:"c" },

  { type:"mc", question:"7. What benefit does NLP offer to professionals reading long reports?",
    passage:`Natural Language Processing (NLP) helps machines understand and respond to human language. It's widely used in applications like chatbots, virtual assistants, and speech-to-text tools. NLP combines deep learning and massive training data to enhance accuracy. Besides, it often works with computer vision in AI systems. What about using NLP to scan long reports and generate short, clear summaries for busy professionals?`,
    answers:{a:"It scans and generates short summaries for quicker understanding.",
             b:"It helps by highlighting every grammar mistake in the text.",
             c:"It translates the reports into different languages automatically."},
    correctAnswer:"a" },

  { type:"mc", question:"8. What makes Grok especially effective in customer service interactions?",
    passage:`Grok is a smart and witty chatbot created by xAI. It uses supervised learning and huge training data to understand language and provide clever answers. It also adapts quickly using natural language processing and neural networks. Its humorous responses make it engaging for users. What about using Grok for customer service to make support more friendly, interactive, and informative?`,
    answers:{a:"It ignores context and relies only on pre-written answers.",
             b:"It gives long technical responses that only experts understand.",
             c:"It responds to questions with helpful, fast, and humorous language."},
    correctAnswer:"c" },

  { type:"mc", question:"9. DeepSeek uses four types of technologies to generate predictions. (True/False)",
    passage:`DeepSeek is a cutting-edge AI model known for its ability to analyze large and complex datasets. It uses unsupervised learning, neural networks, and machine learning to find patterns in data. Its predictions are often accurate because it improves with every new training cycle. Could we use DeepSeek in healthcare systems to help doctors make better and faster decisions during diagnoses?`,
    answers:{a:"True", b:"False"},
    correctAnswer:"b" },

  { type:"mc", question:"10. Grok adapts its responses using natural language processing. (True/False)",
    passage:`Grok is a smart and witty chatbot created by xAI. It uses supervised learning and huge training data to understand language and provide clever answers. It also adapts quickly using natural language processing and neural networks. Its humorous responses make it engaging for users.`,
    answers:{a:"True", b:"False"},
    correctAnswer:"a" },

  { type:"mc", question:"11. ChatGPT cannot improve over time although it learns from large datasets. (True/False)",
    passage:`ChatGPT is a powerful and intelligent chatbot developed using generative AI. It combines deep learning and natural language processing to produce human-like conversations. It continuously learns from large datasets and becomes smarter over time. Besides text, it supports speech-to-text interactions.`,
    answers:{a:"True", b:"False"},
    correctAnswer:"b" },

  { type:"fill", question:"12. Summary: DeepSeek is an advanced model that analyzes large and complex datasets. It applies three technologies to discover patterns. Its predictions become more accurate with each………………….", 
  answer:"new training cycle",
  passage:`DeepSeek is a cutting-edge AI model known for its ability to analyze large and complex datasets. It uses unsupervised learning, neural networks, and machine learning to find patterns in data. Its predictions are often accurate because it improves with every new training cycle. Could we use DeepSeek in healthcare systems to help doctors make better and faster decisions during diagnoses?`
},

{ type:"fill", question:"13. Summary: Grok uses supervised learning and large datasets to generate replies. It responds to questions with humor and adjusts quickly to new input. Its replies are powered by…………………, which makes Grok ideal for training support.",
  answer:"natural language processing",
  passage:`Grok is a smart and witty chatbot created by xAI. It uses supervised learning and huge training data to understand language and provide clever answers. It also adapts quickly using natural language processing and neural networks. Its humorous responses make it engaging for users. What about using Grok for customer service to make support more friendly, interactive, and informative?`
},

  { type:"fill", question:"14. How about……………….so that the deep learning system works more efficiently? (use/ clean/ data)", 
    answer:"using clean data" },

  { type:"fill", question:"15. We couldn't finish the model yesterday, so shall we…………………today instead? (continue/ data analysis)", 
    answer:"continue the data analysis" },

  { type:"fill", question:"16. What about…………………because the current app lacks voice recognition? (add/ speech-to-text)", 
    answer:"adding speech-to-text technology" },

  { type:"order", question:"17. Sắp xếp để thành câu hoàn chỉnh:", 
    correctSentence:"The team hasn't had many ideas, so how about creating content drafts by using generative AI?", 
    tokens:["The team","hasn't had","many ideas,","so how","about","creating","content drafts","by using","generative AI?"] },

  { type:"order", question:"18. Sắp xếp để thành câu hoàn chỉnh:", 
    correctSentence:"I noticed the prediction model is inaccurate, so could you check the training data again?", 
    tokens:["I","noticed","the prediction","model","is","inaccurate,","so could","you","check","the training","data again?"] },

  { type:"order", question:"19. Sắp xếp để thành câu hoàn chỉnh:", 
    correctSentence:"How about using expert systems to manage the growing number of user requests efficiently?", 
    tokens:["How about","using","expert","systems","to manage","the growing","number of","user requests","efficiently?"] },

  { type:"order", question:"20. Sắp xếp để thành câu hoàn chỉnh:", 
    correctSentence:"She fixed the bug, but what about testing the chatbot before deploying it to users?", 
    tokens:["She","fixed","the bug,","but what"," about","testing","the chatbot","before","deploying it","to users?"] }
],

};


// Hiển thị trang
function showPage(id){
  document.querySelectorAll(".wrap > section").forEach(s=>s.classList.add("hidden"));
  const target = document.getElementById(id);
  if(target) target.classList.remove("hidden");
  if(id==="baitap") window.scrollTo({top:0,behavior:"smooth"});
}

// Tạo câu hỏi MC (Có thể có đoạn văn)
function buildMCQuestion(qObj, qIndex){
  const wrapper = document.createElement("div"); wrapper.className="question";
  
  if (qObj.passage) { // Thêm đoạn văn nếu có
    const readingDiv = document.createElement("div"); 
    readingDiv.className = "reading";
    readingDiv.innerText = qObj.passage;
    wrapper.appendChild(readingDiv);
  }

  const p = document.createElement("p"); p.innerText = (qIndex+1) + ". " + qObj.question; wrapper.appendChild(p);
  const answersDiv = document.createElement("div"); answersDiv.className="answers";
  for(const key of ["a","b","c"]){
    if(qObj.answers && qObj.answers[key]){
      const label = document.createElement("label");
      label.innerHTML = `<input type="radio" name="q${qIndex}" value="${key}"> ${key.toUpperCase()}. ${qObj.answers[key]}`;
      answersDiv.appendChild(label);
    }
  }
  const feedback = document.createElement("div"); feedback.className="feedback";
  wrapper.appendChild(answersDiv); wrapper.appendChild(feedback);
  answersDiv.addEventListener("change",(e)=>{
    const val = answersDiv.querySelector(`input[name="q${qIndex}"]:checked`)?.value;
    if(!val) return;
    if(val === qObj.correctAnswer){
      feedback.textContent = "Đúng! ✅"; feedback.className = "feedback ok";
    } else {
      feedback.innerHTML = `Sai ❌. Đáp án đúng: <span class="correct-answer">${qObj.answers[qObj.correctAnswer]}</span>`;
      feedback.className = "feedback err";
    }
  });
  return wrapper;
}

    // =====================================================================
    // CÁC HÀM XỬ LÝ
    // =====================================================================
    const quizForm = document.getElementById('quizForm');
    const resultsPage = document.getElementById('page-results');
    const quizPage = document.getElementById('page-bai7');
    const fireworksContainer = document.getElementById('fireworksContainer');
    const finalScoreEl = document.getElementById('finalScore');
    const okButton = document.getElementById('okButton');
    let fireworks;

    // --- Hàm tạo form trắc nghiệm ---
    function buildQuiz() {
        let formHTML = '';
        quizData.questions.forEach((q, index) => {
            formHTML += `<div class="question-block" style="animation-delay: ${index * 100}ms">`; 
            
            formHTML += `<p>${q.question}</p>`;
            if (q.passage) {
                formHTML += `<div class="passage">${q.passage}</div>`;
            }
            formHTML += `<div class="options">`;

            if (q.type === 'mc') {
                for (const optionKey in q.options) {
                    formHTML += `<label><input type="radio" name="answers[${index}]" value="${optionKey}"> <strong>${optionKey}.</strong> ${q.options[optionKey]}</label>`;
                }
            } else if (q.type === 'fill') {
                formHTML += `<input type="text" name="answers[${index}]" class="fill-in-blank-input" placeholder="Nhập câu trả lời của bạn...">`;
            }

            formHTML += `</div></div>`;
        });
        formHTML += `<hr><button type="submit" class="quiz-submit-btn">Nộp Bài</button>`;
        quizForm.innerHTML = formHTML;
    }

    // --- Hàm xử lý nộp bài và chấm điểm ---
    let detailedResultsHTML = '';
    quizForm.addEventListener('submit', function(event) {
        event.preventDefault(); 

        const formData = new FormData(quizForm);
        let score = 0;
        detailedResultsHTML = `
            <a class="back-link" href="#bai7">← Làm lại bài</a>
            <h1>📝 Kết quả ${quizData.title}</h1>
            <div class="result-detail">`;

        quizData.questions.forEach((q, index) => {
            const userAnswer = formData.get(`answers[${index}]`);
            let isCorrect = false;

            if (q.type === 'mc') {
                isCorrect = userAnswer === q.correct;
            } else if (q.type === 'fill') {
                isCorrect = userAnswer && userAnswer.trim().toLowerCase().replace(/[.?!,]/g, '') === q.correct.trim().toLowerCase().replace(/[.?!,]/g, '');
            }

            if (isCorrect) score++;

            detailedResultsHTML += `<div class="question-block ${isCorrect ? 'correct' : 'incorrect'}" style="animation-delay: ${index * 50}ms">`;
            detailedResultsHTML += `<p>${q.question}</p>`;
            if (q.passage) {
                detailedResultsHTML += `<div class="passage">${q.passage}</div>`;
            }

            if (q.type === 'mc') {
                const userAnswerText = userAnswer ? `${userAnswer}. ${q.options[userAnswer]}` : '<i>Chưa trả lời</i>';
                detailedResultsHTML += `<p>Câu trả lời của bạn: <span class="user-answer">${userAnswerText}</span></p>`;
                // Đáp án đúng nổi bật hơn
                detailedResultsHTML += `<p class="correct-answer-text" style="background:#e6ffe6;border:2px solid #28a745;padding:8px 12px;border-radius:6px;display:inline-block;margin-top:8px;font-weight:bold;color:#155724;">✔ Đáp án đúng: ${q.correct}. ${q.options[q.correct]}</p>`;
            } else if (q.type === 'fill') {
                const userAnswerText = userAnswer ? userAnswer : '<i>Chưa trả lời</i>';
                detailedResultsHTML += `<p>Câu trả lời của bạn: <span class="user-answer">${userAnswerText}</span></p>`;
                detailedResultsHTML += `<p class="correct-answer-text" style="background:#e6ffe6;border:2px solid #28a745;padding:8px 12px;border-radius:6px;display:inline-block;margin-top:8px;font-weight:bold;color:#155724;">✔ Đáp án đúng: ${q.correct}</p>`;
            }
            detailedResultsHTML += `</div>`;
        });
        
        const totalQuestions = quizData.questions.length;
        const score10 = ((score / totalQuestions) * 10).toFixed(2);

        const summaryHTML = `
            <div class="result-summary">
                <h2>Điểm của bạn: ${score10} / 10</h2>
                <p>(Trả lời đúng ${score} / ${totalQuestions} câu)</p>
            </div>`;
        
        detailedResultsHTML = summaryHTML + detailedResultsHTML + '</div>';

        // --- Kích hoạt hiệu ứng pháo hoa ---
        finalScoreEl.textContent = `${score10} / 10`;
        fireworksContainer.style.display = 'block';
        setTimeout(() => fireworksContainer.classList.add('active'), 10); 
        
        if (!fireworks) {
            fireworks = new Fireworks.default(fireworksContainer);
        }
        fireworks.start();
    });

    // --- Xử lý nút OK trên modal ---
    okButton.addEventListener('click', () => {
        fireworks.stop();
        fireworksContainer.classList.remove('active');
        setTimeout(() => fireworksContainer.style.display = 'none', 300); 

        // SỬA LỖI TẠI ĐÂY: Thay vì gọi showPage, ta set hash để router tự xử lý
        window.location.hash = '#results';
    });

    // --- LOGIC XỬ LÝ FULLSCREEN ---
    function enterFullscreen(element) {
        if (element.requestFullscreen) {
            element.requestFullscreen();
        } else if (element.mozRequestFullScreen) { // Firefox
            element.mozRequestFullScreen();
        } else if (element.webkitRequestFullscreen) { // Chrome, Safari, Opera
            element.webkitRequestFullscreen();
        } else if (element.msRequestFullscreen) { // IE/Edge
            element.msRequestFullscreen();
        }
    }

    function exitFullscreen() {
        if (document.exitFullscreen) {
            document.exitFullscreen();
        } else if (document.mozCancelFullScreen) {
            document.mozCancelFullScreen();
        } else if (document.webkitExitFullscreen) {
            document.webkitExitFullscreen();
        } else if (document.msExitFullscreen) {
            document.msExitFullscreen();
        }
    }

    // --- Logic chuyển trang (Router) ---
    function showPage() {
        const pageId = window.location.hash.substring(1) || 'home';
        
        const currentActive = document.querySelector('.page-content.active');
        if (currentActive) {
            currentActive.classList.remove('active');
        }

        setTimeout(() => {
            if (currentActive) currentActive.style.display = 'none';

            let pageToShow = 'page-' + pageId;
            let activePage = document.getElementById(pageToShow);

            if (pageId === 'bai7') {
                buildQuiz();
                quizForm.reset();
            }
            
            // Xử lý trang kết quả đặc biệt
            if (pageId === 'results') {
                resultsPage.innerHTML = detailedResultsHTML;
            }

            if (activePage) {
                activePage.style.display = 'block';
                setTimeout(() => activePage.classList.add('active'), 10); 
                document.title = activePage.querySelector('h1').textContent.replace(/[📝📚]/g, '').trim();
            } else {
                const homePage = document.getElementById('page-home');
                homePage.style.display = 'block';
                setTimeout(() => homePage.classList.add('active'), 10);
                document.title = 'Danh Sách Bài Tập';
            }
        }, 200);
    }

    window.addEventListener('hashchange', showPage);
    
    // --- JAVASCRIPT CHO DARK MODE & EVENTS ---
    document.addEventListener('DOMContentLoaded', () => {
        const preloader = document.getElementById('preloader');
        preloader.classList.add('hidden');
        
        showPage(); 

        const themeToggle = document.getElementById('themeToggle');
        const body = document.body;

        function applyTheme(theme) {
            body.classList.toggle('dark-mode', theme === 'dark');
        }

        themeToggle.addEventListener('click', () => {
            let newTheme = body.classList.contains('dark-mode') ? 'light' : 'dark';
            applyTheme(newTheme);
            localStorage.setItem('theme', newTheme);
        });

        const savedTheme = localStorage.getItem('theme') || 'light';
        applyTheme(savedTheme);

        document.querySelectorAll('.exercise-link').forEach(link => {
            link.addEventListener('click', () => {
                enterFullscreen(document.documentElement);
            });
        });

        document.body.addEventListener('click', function(event) {
            if (event.target.matches('.back-link')) {
                exitFullscreen();
            }
        });
    });
</script>

</body>
</html>
