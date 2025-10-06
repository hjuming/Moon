靜態網頁部署指南：從 GitHub 到 Cloudflare Pages
這份文件旨在記錄如何將一個單頁式的靜態 HTML 專案（例如電子賀卡），透過 GitHub 部署到 Cloudflare Pages，並成功綁定自訂網域。我們將涵蓋從檔案準備到最終除錯的所有關鍵步驟與注意事項。
核心觀念
部署一個純靜態 HTML 網站非常簡單，但需緊記以下兩個核心觀念：
1. 預設首頁檔案：網頁伺服器（包含 Cloudflare）在讀取一個網站目錄時，會自動尋找名為 index.html 的檔案作為預設首頁。這是網頁的標準規範。
2. 無需建置：純 HTML、CSS 和 JavaScript 檔案是瀏覽器可以直接讀取的「靜態檔案」，它們不需要像 React 或 Vue 等框架一樣經過「建置」(Build) 過程。因此，在部署設定中必須告知平台跳過此步驟。
部署流程教學
步驟一：準備 HTML 檔案
1. 確認程式碼：確保您的 HTML、CSS 和 JavaScript 程式碼都已經完成。
2. 最重要的步驟：重新命名檔案：將您的 HTML 檔案務必重新命名為 index.html (建議全小寫)。這是解決「部署後找不到頁面」問題最關鍵的一步。
步驟二：上傳至 GitHub
1. 在您的 GitHub 帳號中，建立一個新的儲存庫 (Repository)。
2. 將您命名好的 index.html 檔案上傳到這個儲存庫中。
步驟三：連接 Cloudflare Pages
1. 登入 Cloudflare 儀表板，在左側選單選擇 Workers & Pages。
2. 點擊 Create application > Pages > Connect to Git。
3. 選擇您剛剛建立的 GitHub 儲存庫並授權 Cloudflare 存取。
步驟四：設定建置選項
在 "Set up builds and deployments" 頁面，這是另一個絕對不能錯過的關鍵設定：
• 框架預設 (Framework preset)：選擇 無 (None)。
• 建置指令 (Build command)：完全留空，不要填寫任何指令。
• 建置輸出目錄 (Build output directory)：完全留空，或填寫根目錄 /。
設定完成後，點擊 Save and Deploy。Cloudflare 會開始您的第一次部署，通常在一分鐘內就會完成。
自訂網域設定 (可選)
若您想使用自己的網域（例如 moon.wedopr.com）而非 Cloudflare 提供的 *.pages.dev 網址，請依照以下步驟：
1. 在您的 Pages 專案中，前往 自訂網域 (Custom domains) 標籤頁。
2. 點擊 設定網域 (Set up a domain) 並輸入您想使用的完整網域名稱。
3. Cloudflare 會提供給您一個目標網址 (通常是您專案的 *.pages.dev 網址)，請將此網址複製下來。
4. 前往您的網域註冊商（例如 GoDaddy, Google Domains 等您購買網域的地方）的 DNS 管理介面。
5. 新增一筆 CNAME 紀錄：
• 類型 (Type): CNAME
• 名稱/主機 (Name/Host): 填寫子網域的部分 (例如: moon)
• 目標/指向 (Target/Points to): 貼上您剛剛從 Cloudflare 複製的 *.pages.dev 網址。
• TTL: 維持預設值即可。
6. 儲存紀錄。請注意，DNS 生效需要幾分鐘到數小時不等，請耐心等候。
常見問題與除錯 (Troubleshooting)
Q1: 部署日誌顯示成功，但網頁一片空白或 404 Not Found？
• A1: 檢查檔名。99% 的可能性是您的 HTML 檔名不叫 index.html。請回到 GitHub 將檔案更名為 index.html (全小寫) 並等待網站自動重新部署。
• A2: 清除快取 (Cache)。您的瀏覽器可能記住了舊的錯誤頁面。請使用 強制重新整理 (Ctrl+Shift+R 或 Cmd+Shift+R)，或用無痕模式開啟網頁來確認。
Q2: 自訂網域設定好了，但還是無法訪問？
• A1: 檢查 CNAME 紀錄。請再三確認您在 DNS 管理介面中設定的 CNAME 紀錄的「名稱」和「目標」是否完全正確，沒有多餘的空格或拼寫錯誤。
• A2: 等待 DNS 生效。DNS 全球傳播需要時間，請耐心等候 30 分鐘後再試。有時可能需要更長時間。
• A3: 檢查 SSL 憑證狀態。在 Cloudflare 的 SSL/TLS 頁面，確認您網域的 SSL 憑證是否已成功簽發 (狀態應為 Active)。
附錄：賀卡最終程式碼
