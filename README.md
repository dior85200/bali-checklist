# bali-checklist
峇里島旅遊確認清單
[checklist.html](https://github.com/user-attachments/files/23738759/checklist.html)
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>峇里島旅遊個人確認清單</title>
    <style>
        /* --- 基礎樣式 --- */
        body {
            font-family: '微軟正黑體', 'Arial', sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f9;
        }
        .container {
            max-width: 700px;
            margin: 20px auto;
            background: #fff;
            padding: 25px;
            border-radius: 12px;
            box-shadow: 0 6px 15px rgba(0, 0, 0, 0.15);
        }
        h1 {
            color: #007bff;
            text-align: center;
            padding-bottom: 10px;
        }
        h3 {
            color: #333;
            margin-top: 30px;
            border-bottom: 1px dashed #ccc;
            padding-bottom: 5px;
        }
        /* --- 集合資訊區塊 --- */
        .info-box {
            background-color: #e9f7ff;
            border-left: 5px solid #007bff;
            padding: 15px;
            margin-bottom: 20px;
            border-radius: 4px;
            font-size: 15px;
            line-height: 1.6;
        }
        .info-box strong {
            display: block;
            margin-top: 5px;
            color: #0056b3;
        }

        /* --- 清單項目樣式 --- */
        .check-item {
            border: 1px solid #ddd;
            border-radius: 6px;
            margin-bottom: 10px;
            overflow: hidden;
            transition: box-shadow 0.2s;
        }
        .check-item.checked {
            background-color: #e6ffe6; /* 完成後顏色 */
            border-color: #28a745;
        }
        .item-header {
            display: flex;
            align-items: center;
            padding: 15px;
            cursor: pointer;
            background-color: #fff;
        }
        .check-item.checked .item-header {
            background-color: #f0fff0;
        }
        
        .check-item input[type="checkbox"] {
            min-width: 24px; 
            min-height: 24px;
            margin-right: 15px;
        }
        .check-item label {
            flex-grow: 1;
            font-size: 17px;
            font-weight: bold;
            color: #333;
        }
        .check-item.checked label {
            text-decoration: line-through;
            color: #666;
            font-weight: normal;
        }
        
        /* --- 解說內容樣式 --- */
        .item-content {
            padding: 10px 15px 15px 55px; /* 內縮配合 checkbox */
            background-color: #fafafa;
            border-top: 1px solid #eee;
            display: none; /* 預設隱藏 */
            font-size: 14px;
            color: #555;
            line-height: 1.5;
        }
        .item-content p {
            margin: 5px 0 0 0;
        }

        /* --- 鼓勵訊息樣式 --- */
        #completionMessage {
            display: none;
            text-align: center;
            margin-top: 40px;
            padding: 25px;
            background-color: #ffeeba;
            color: #664d03;
            border: 2px dashed #ffc720;
            border-radius: 8px;
            font-size: 22px;
            font-weight: bold;
        }
        /* --- 切換箭頭 --- */
        .toggle-icon {
            margin-left: 15px;
            font-size: 18px;
            color: #007bff;
            transition: transform 0.3s;
        }
        .toggle-icon.open {
            transform: rotate(90deg);
        }
    </style>
</head>
<body>

<div class="container">
    <h1>✈️ 峇里島啟程個人確認清單</h1>

    <div class="info-box">
        <p>各位貴賓早安！您的**峇里島旅程**即將開始。請利用以下清單，確認所有重要事項皆已在您手機上準備妥當！</p>
        
        <strong>【接送車集合資訊】</strong>
        <p>🕰️ **時間：** 05:00 <br>
        📍 **地點：** 高鐵站前門市 (414 臺中市烏日區站區二路 181 號 183 號)</p>
        
        <strong>【自行前往機場資訊】</strong>
        <p>🕰️ **時間：** 2025/11/26 AM 07:30 集合<br>
        📍 **地點：** 桃園第二航廈 - 長榮航空團體櫃台</p>
    </div>

    <h3>✅ 行前檢查項目</h3>

    <div id="checkList">
        </div>

    <div id="completionMessage">
        🎉 **太棒了！所有事項都已確認完畢！**<br>
        放下緊張感，一起好好的享受這趟闊別許久旅程吧！
    </div>
</div>

<script>
    const STORAGE_KEY = 'baliPersonalChecklist'; // 唯一的儲存鍵
    const checkListContainer = document.getElementById('checkList');
    const completionMessage = document.getElementById('completionMessage');

    // 整合後的清單資料
    const defaultTravelItems = [
        { 
            name: "護照與效期檢查 (效期需六個月以上)", 
            content: "出門前切記檢查護照是否攜帶，並且確認護照效期至少在六個月以上。", 
            checked: false 
        },
        { 
            name: "健康聲明書 QR Code 存檔", 
            content: "健康聲明書的 QR CODE 請記得存檔在手機裡唷，以備海關查驗。", 
            checked: false 
        },
        { 
            name: "行李重量檢查 (托運23kg/手提7kg)", 
            content: "托運行李一人限制 23 公斤，手提行李一件限制 7 公斤。", 
            checked: false 
        },
        { 
            name: "個人隨身藥品準備", 
            content: "個人隨身藥品、慢性處方箋、退熱貼、退燒藥、感冒藥、腸胃藥，請記得隨身準備。", 
            checked: false 
        },
        { 
            name: "衣物準備 (輕薄、好穿脫)", 
            content: "當地溫度約 20～26 度。建議以好穿脫衣物為主，並攜帶輕薄外套和雨具。", 
            checked: false 
        },
        { 
            name: "鞋子準備 (好走鞋 + 拖鞋/涼鞋)", 
            content: "請穿著好走的鞋子 (穿 1 雙帶 1 雙為佳)！拖鞋或涼鞋一定要帶喔！", 
            checked: false 
        },
        { 
            name: "印尼盾兌換用美元", 
            content: "導遊可以協助兌換印尼盾，建議帶美元前往。", 
            checked: false 
        },
        { 
            name: "電子用品：變壓器 (電壓 220V)", 
            content: "峇里島電壓為 220V。如果要帶吹風機等電器，請自行攜帶變壓器唷！", 
            checked: false 
        },
        { 
            name: "盥洗用品 (牙刷/牙膏/一次性用品)", 
            content: "牙刷、牙膏跟一次性用品要記得帶唷！(多數飯店不提供或需自費)。", 
            checked: false 
        },
        { 
            name: "酒精攜帶限制確認", 
            content: "隨身酒精限 100cc 以下並用夾鏈袋收納。超過 100cc 請放托運行李，單瓶不得超過 500cc。", 
            checked: false 
        },
        {
            name: "水壺準備 (方便機場裝水)",
            content: "建議帶個水壺，機場裝水較為方便。",
            checked: false
        }
    ];

    // 1. 從 localStorage 載入資料
    function loadItems() {
        const storedItems = localStorage.getItem(STORAGE_KEY);
        // 如果 localStorage 有資料，但長度不一致（代表清單有更新），則保留預設清單
        if (storedItems && JSON.parse(storedItems).length === defaultTravelItems.length) {
            return JSON.parse(storedItems);
        }
        return defaultTravelItems;
    }

    let currentItems = loadItems();

    // 2. 儲存資料到 localStorage
    function saveItems() {
        localStorage.setItem(STORAGE_KEY, JSON.stringify(currentItems));
    }

    // 3. 檢查所有項目是否都已完成
    function checkCompletion() {
        const allChecked = currentItems.every(item => item.checked);
        completionMessage.style.display = allChecked ? 'block' : 'none';
    }

    // 4. 切換勾選狀態
    function toggleCheck(index) {
        currentItems[index].checked = !currentItems[index].checked;
        saveItems();
        renderItems();
        checkCompletion();
    }

    // 5. 渲染清單
    function renderItems() {
        checkListContainer.innerHTML = ''; 
        currentItems.forEach((item, index) => {
            const itemDiv = document.createElement('div');
            itemDiv.className = `check-item ${item.checked ? 'checked' : ''}`;
            
            // 項目標題區塊
            const headerDiv = document.createElement('div');
            headerDiv.className = 'item-header';
            
            // Checkbox
            const checkbox = document.createElement('input');
            checkbox.type = 'checkbox';
            checkbox.checked = item.checked;
            checkbox.id = `item-${index}`;
            // 點擊 Checkbox 獨立切換
            checkbox.onclick = (e) => { 
                e.stopPropagation(); // 阻止事件冒泡到整個 item header
                toggleCheck(index); 
            }; 

            // Label (文字內容)
            const label = document.createElement('label');
            label.textContent = item.name;

            // 切換解說內容的箭頭圖示
            const icon = document.createElement('span');
            icon.className = 'toggle-icon';
            icon.innerHTML = '▶'; // 右箭頭

            // 解說內容區塊
            const contentDiv = document.createElement('div');
            contentDiv.className = 'item-content';
            contentDiv.innerHTML = `<p>${item.content}</p>`;
            
            // 點擊標題區塊（非 checkbox）或箭頭，切換解說內容顯示/隱藏
            headerDiv.onclick = (e) => {
                if (e.target.type !== 'checkbox') {
                    const isHidden = contentDiv.style.display === 'none' || contentDiv.style.display === '';
                    contentDiv.style.display = isHidden ? 'block' : 'none';
                    icon.classList.toggle('open', isHidden);
                }
            };

            // 組合元素
            headerDiv.appendChild(checkbox);
            headerDiv.appendChild(label);
            headerDiv.appendChild(icon);
            
            itemDiv.appendChild(headerDiv);
            itemDiv.appendChild(contentDiv);
            checkListContainer.appendChild(itemDiv);
        });

        checkCompletion(); // 每次渲染後檢查是否完成
    }

    // 首次載入頁面
    renderItems();
</script>

</body>
</html>
