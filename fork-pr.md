### Fork 專案

Fork 是複製別人的專案到你的帳號下。

#### 何時需要 Fork？

- 貢獻開源專案
- 沒有原專案的寫入權限
- 想要自己的副本進行實驗

#### Fork 流程

找另一位同學，扮演 A 和 B 兩個角色，角色 A 為公開專案的所有者，角色 B 為貢獻者。
角色 A 使用範例 Repository：`https://github.com/shinder/js-vid-class`（自己的也可以，在此使用公開 Repo 為範例說明），git clone 之後，移除 `.git` 資料夾，再 push 到自己的 Repository。

以下為角色 B 進行 Fork 操作：

1. **前往要 Fork 的專案**
   - 例如：`https://github.com/original-owner/awesome-project`

2. **點選右上角的「Fork」按鈕**

3. **選擇 Fork 到哪個帳號**
   - 通常是你的個人帳號，Repo 名稱通常不變更

4. **等待 Fork 完成**
   - 現在你有了一個副本：`https://github.com/your-username/awesome-project`

5. **Clone 你的 Fork**

```bash
git clone git@github.com:your-username/awesome-project.git
cd awesome-project
```

6. **加入原專案為上游（upstream）**

```bash
git remote add upstream git@github.com:original-owner/awesome-project.git

# 查看遠端
git remote -v
```

<img src="/images/fork01-after-fork.png" alt="fork 之後" /><br /><br />

定期從上游同步原始專案的變更。也可以在 Fork 而來的 Repo 主頁看到上游 Repo 變更的情況，有變更時可以按「Sync fork」和上游 Repo 同步。

<img src="/images/fork02-sync-fork.png" alt="同步上游內容" /><br /><br />

### 建立 Pull Request

#### 流程 1：準備程式碼

角色 B 進行：

```bash
# 1. 確保本地是最新的
git switch main
git pull origin main

# 2. 如果有 upstream，也同步
git fetch upstream
git merge upstream/main

# 3. 建立功能分支
git switch -c fix/practice

# 4. 做出變更
# ... 編輯檔案 ...

# 5. 提交
git add README.md
git commit -m "修改文字"

# 6. 推送到你的 Fork
git push -u origin fix/practice
# git push --set-upstream origin fix/practice
```

<img src="/images/fork03-after-push-branch.png" alt="推送你的分支之後" /><br /><br />

#### 流程 2：在 GitHub 建立 PR

角色 B 進行：

1. **前往你的 Fork**
   - `https://github.com/your-username/awesome-project`

2. **GitHub 會顯示提示**
   - 「fix/practice had recent pushes」
   - 點選「Compare & pull request」

3. **填寫 PR 資訊**
   - **Title**：簡短描述（大約 50 字元以內）

     ```text
     Fix 修正錯字
     ```

   - **Description**：詳細說明（使用 Markdown）

     ```markdown
     ## 變更內容
     修正 README 中的拼寫錯誤 ...
     ```

4. **點選「Create pull request」**

<img src="/images/fork04-create-pull-request.png" alt="建立 PR" /><br /><br />

<img src="/images/pr01-created-pull-request.png" alt="PR 建立完成" /><br /><br />

### PR 建議（給角色 B）

好的 PR 描述應該包含：

```markdown
## 目的
說明為什麼需要這個變更

## 變更內容
- 變更 1
- 變更 2
- 變更 3

## 測試方式
1. 執行 `npm test`
2. 手動測試登入功能
3. 確認錯誤訊息正確顯示

## 截圖
Before / After 截圖（如果有 UI 變更）

## 注意事項
需要注意的事項（如破壞性變更、需要更新文件）

## 相關連結
- Issue #123
- 相關 PR #124
```

**自動關閉 Issue**：
在 PR 描述中使用關鍵字：

```markdown
Fixes #123
Closes #124
Resolves #125
```

當 PR 合併時，這些 Issue 會自動關閉。

### Code Review 流程

#### 作為 Reviewer（審查者）

角色 A 進行：

1. **前往 PR 頁面**

<img src="/images/pr02-pull-requests.png" alt="Pull Requests 分頁" /><br /><br />

2. **查看「Files changed」頁籤**

<img src="/images/pr03-files-chaged.png" alt="Files changed 頁籤" /><br /><br />

3. **逐行審查程式碼**
   - 點選行號旁的「+」可以加入評論
   - 可以選擇多行一起評論

4. **給予回饋**

   **讚美好的程式碼**：

   ```text
   這個重構很棒，程式碼更清晰了！
   ```

   **建議改進**：

   ```text
   建議：這裡可以使用 Array.filter() 來簡化邏輯
   ```

   **指出問題**：

   ```text
   這裡有 SQL Injection 風險，建議使用參數化查詢
   ```

   **提問**：

   ```text
   為什麼選擇使用這個演算法？有考慮過效能嗎？
   ```

5. **完成審查**

   點選「Review changes」：

   - **Comment**：只是留言，不需要批准
   - **Approve**：批准變更，可以合併
   - **Request changes**：要求修改，不能合併

6. **提交審查**

   填寫整體評論，點選「Submit review」

#### 作為 Author（作者）

角色 B：

1. **回應評論**
   - 感謝有幫助的建議
   - 解釋你的設計決策
   - 承諾修改問題

2. **進行修改**

   ```bash
   # 在原本的分支上繼續修改
   git switch fix/practice

   # 修改程式碼
   # ...

   # 提交
   git add .
   git commit -m "Address review comments"

   # 推送（會自動更新 PR）
   git push
   ```

3. **標記為已解決**
   - 修改完成後，在評論下點選「Resolve conversation」

4. **請求重新審查**
   - 點選 Reviewer 旁的「🔄」圖示

### Code Review 最佳實踐

#### 給 Reviewer 的建議

**要做的**：

- 建設性的回饋
- 具體的建議和範例
- 讚美好的程式碼
- 提問而非命令
- 區分「必須修改」和「建議」

**不要做的**：

- 人身攻擊
- 只說「不好」但不說為什麼
- 過於吹毛求疵
- 要求按照你的風格寫

**良好的評論範例**：

```text
不好：「這段程式碼很爛」
好：「這個函數有點複雜，建議拆分成更小的函數來提高可讀性」

不好：「錯了」
好：「這裡的邏輯似乎有問題，當 x 為 null 時會拋出錯誤」

不好：「改成我的寫法」
好：「有考慮過使用 Map 嗎？在這個場景下查找效率會更好」
```

#### 給 Author 的建議

**要做的**：

- 虛心接受回饋
- 解釋你的設計決策
- 感謝 reviewer 的時間
- 及時回應和修改
- 保持 PR 簡小（容易審查）

**不要做的**：

- 防衛心態
- 忽略評論
- 提交過大的 PR（幾千行）
- 在 PR 中混入不相關的變更

**回應評論範例**：

```text
「感謝建議！我會改用參數化查詢來防止 SQL Injection」
「好問題！我選擇這個演算法是因為...，雖然效能差一點，但更容易維護」
「你說得對，這裡確實有 bug，我馬上修正」
```

### 合併 Pull Request

當 PR 通過審查後，可以合併到主分支。

#### 三種合併方式

<img src="/images/pr04-merge-pull-request.png" alt="三種合併方式" /><br /><br />

**1. Merge Commit（預設）**

```text
Before:
main    A---B
         \
feature   C---D

After:
main    A---B---M
         \     /
feature   C---D
```

- 保留所有提交歷史
- 建立一個合併 commit
- 分支結構清晰

```bash
# 相當於
git merge --no-ff feature
```

**2. Squash and Merge**

```text
Before:
main    A---B
         \
feature   C---D---E

After:
main    A---B---CDE'
```

- 將所有提交壓縮成一個
- 歷史更簡潔
- 適合有很多小提交的 PR

```bash
# 相當於
git merge --squash feature
git commit
```

**3. Rebase and Merge**

```text
Before:
main    A---B
         \
feature   C---D

After:
main    A---B---C'---D'
```

- 將提交重新套用到主分支
- 線性歷史
- 看起來像直接在 main 上開發

```bash
# 相當於
git rebase main
git merge --ff-only
```

#### 選擇合併方式

| 方式 | 優點 | 缺點 | 適合場景 |
|-----|------|------|---------|
| **Merge Commit** | 保留完整歷史 | 歷史複雜 | 想保留分支結構 |
| **Squash** | 歷史簡潔 | 失去詳細歷史 | 有很多小提交 |
| **Rebase** | 線性歷史 | 改寫歷史 | 想要乾淨的歷史 |

**團隊建議**：

- 開源專案：通常使用 Squash（保持主線簡潔）
- 企業專案：根據團隊規範
- 個人專案：看個人喜好

#### 在 GitHub 上合併 PR

1. **確認 PR 可以合併**
   - 所有 CI 檢查通過
   - 沒有衝突
   - 獲得足夠的批准

2. **點選「Merge pull request」**

3. **選擇合併方式**（下拉選單）
   - Merge commit
   - Squash and merge
   - Rebase and merge

4. **確認合併訊息**

5. **點選「Confirm merge」**

6. **刪除分支**（選擇性）
   - 合併後會提示「Delete branch」
   - 建議刪除已合併的分支

### Draft Pull Request

草稿 PR，表示還在開發中，不要合併。

#### 建立 Draft PR

1. 建立 PR 時，點選「Create draft pull request」
2. 或在現有 PR 中，右側選單點選「Convert to draft」

#### 用途

- 尚未完成，但想先展示進度
- 想提早獲得回饋
- 讓 CI 測試
- 追蹤進度

#### 標記為 Ready

完成後點選「Ready for review」
