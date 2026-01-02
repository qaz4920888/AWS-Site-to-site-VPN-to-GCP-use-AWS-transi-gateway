# AWS Transit Gateway–Based Multi-VPC Architecture with Site-to-Site VPN to GCP


本文記錄 **AWS Site-to-Site VPN 透過 Transit Gateway 連接至 GCP HA VPN（BGP）** 的實作流程與注意事項。

---

## 架構說明

- AWS：
  - VPC
  - Transit Gateway (TGW)
  - Customer Gateway (CGW)
  - Site-to-Site VPN
- GCP：
  - VPC
  - HA VPN
  - Cloud Router (BGP)

---

## 1. 建立 AWS / GCP VPC Subnet（可路由、可通 IGW）

### 1.1 AWS / GCP VPC 與 Subnet

1.  
<img width="1725" height="647" alt="image" src="https://github.com/user-attachments/assets/de184969-0dc6-4be8-85df-506ecfeaf446" />

<img width="1610" height="830" alt="image" src="https://github.com/user-attachments/assets/2fca63b3-2f25-42e4-8aea-340715f652e6" />

---

## 2. 建立 GCP HA-VPN  
（為了稍後 AWS Custmon-Gateway 要填寫對端 GCP 的網路介面 IP）

1.  
<img width="986" height="800" alt="image" src="https://github.com/user-attachments/assets/8be6515a-82ef-4619-87ea-e46ca9fb51a5" />

---

## 3. 建立 Transi-Gateway  
（此次用 TGW，傳統使用 VGW）

### 3.1 建立 Transi Gateway

<img width="1571" height="825" alt="image" src="https://github.com/user-attachments/assets/8642e6bb-46de-4d97-b90c-16bf8eeb2ac0" />

---

### 3.2 建立 Transi Gateway Attachment  
（目的是為了讓 VPC 關連到 Transi Gateway）

<img width="1526" height="793" alt="image" src="https://github.com/user-attachments/assets/8c450ee0-e82e-4787-aeb4-35bb78acb807" />

---

### 3.3 建立 Custmon Gateway  
（這邊會需填寫 GCP 的網路介面 IP，目的是讓 AWS 知道 GCP 的 IP 配上 AWS 通道）

<img width="1375" height="816" alt="image" src="https://github.com/user-attachments/assets/9e0daf22-74cc-4624-bb96-ca79241dd75d" />

<img width="1537" height="700" alt="image" src="https://github.com/user-attachments/assets/986f47de-dd5f-462e-aac6-3c4591da2eaf" />

<img width="1533" height="639" alt="image" src="https://github.com/user-attachments/assets/4e3df798-feaa-470d-8bec-7ec75454415a" />

<img width="1527" height="824" alt="image" src="https://github.com/user-attachments/assets/3f5b7fd6-7d3f-41bf-a658-72a1c0359b6a" />

---

### 3.4 Transi Gateway Attachment（再次使用 VPN）

<img width="1392" height="832" alt="image" src="https://github.com/user-attachments/assets/7087cd9d-d49a-4b1d-bab8-0e0f7edc2bcc" />

<img width="1526" height="826" alt="image" src="https://github.com/user-attachments/assets/b359e14e-ac41-4eb4-9500-faa8e5eff938" />

---

### 3.5 Attachment 建立完成後  
即可查看 Site-to-Site VPN Connection 出現

<img width="1912" height="840" alt="image" src="https://github.com/user-attachments/assets/d16aa8fd-3d59-478f-9e96-dffc06e05c56" />

---

## 4. 回到 GCP 填寫介面並建立 Cloud Router

### 4.1 填寫 VPN 通道資訊

- 點選 VPN connection（site to site vpn connection）進入
- 勾選下方可看到通道詳細資訊
- 一個連接會有 **兩個通道 IP**
- 將通道 IP 填寫至 GCP VPN 通道

### 4.2 **須注意**

關於在 GCP 上填寫時候：

- 一定要先看好 AWS 上的 VPN connection  
- 對應到的 **客戶閘道 IP**

範例說明：
- 客戶閘道 IP：`34.55.55.55`
- 下方出現通道 IP：`3.3.3.3`、`4.4.4.4`
- 在 GCP 介面上就需要一一對應好  
👉 **否則會通不了**

<img width="1918" height="830" alt="image" src="https://github.com/user-attachments/assets/4af9d1f3-4faa-458b-a5e5-b1300d5a5e7c" />

<img width="1911" height="853" alt="image" src="https://github.com/user-attachments/assets/d77981eb-f301-499c-b4ec-4d2d1a499b26" />

<img width="1913" height="900" alt="image" src="https://github.com/user-attachments/assets/ac643c3b-ceca-4d41-b290-0a044d843546" />

<img width="1816" height="792" alt="image" src="https://github.com/user-attachments/assets/179cbbd0-8e35-4e5b-93b2-98aef172895a" />

---

### 4.3 回到 AWS CGW（Customer Gateway）

- 使用 IKEv2 編輯通道選項
- 選擇對應在 GCP 上的 IP
- 對應的 Key 必須完全正確
- 以此類推做 **四次**
  - 本次為 **兩個 CGW**
  - 對應 **四個通道**

<img width="1911" height="829" alt="image" src="https://github.com/user-attachments/assets/76fa8919-cf28-407b-98ba-b6f5d1dd19a3" />

<img width="1469" height="815" alt="image" src="https://github.com/user-attachments/assets/56ebdefe-871a-4080-ab7a-0c9d58aa9208" />

---

### 4.4 設定 BGP 工作階段

- 使用通道 IP 旁邊的 **私有 IP 位址**
- 舉例：- 往後加上兩位數，變成：
- `16.1.1.2`
- `16.1.1.3`

填寫方式：
- BGP 對等點 IPv4 位置：`16.1.1.2`
- Cloud Router BGP IPv4：`16.1.1.3`

<img width="1903" height="674" alt="image" src="https://github.com/user-attachments/assets/08e4ce6d-bb17-43c0-8fc7-bed93ee14e6b" />

<img width="519" height="921" alt="image" src="https://github.com/user-attachments/assets/ac5ddbe7-465b-4688-b1f9-8753826f154f" />

---

### 4.5 錯誤範例

<img width="1555" height="539" alt="image" src="https://github.com/user-attachments/assets/f2e599d5-4231-4b36-8e94-055558ef46a2" />

<img width="1760" height="837" alt="image" src="https://github.com/user-attachments/assets/9244e5e5-4f9a-4b56-a68c-5aafd9c21704" />

---

### 4.6 正確部署完成後

<img width="1309" height="230" alt="image" src="https://github.com/user-attachments/assets/f13ada28-4fcd-46f1-a187-0cc90d128711" />

---
### 4.7 注意最後要到AWS 使用的子網路的路由表 去添加 目的地ip GCP 子網往段  否則路由通 還是會ping不到
<img width="1901" height="753" alt="image" src="https://github.com/user-attachments/assets/b2125752-5068-4e52-9d01-6439cb658dec" />

### 4.8注意事項

- 請注意：
- AWS Security Group
- AWS Network ACL
- GCP Firewall

皆需放通  
最後可在雙方啟動 VM，進行 `ping` 測試確認連線

## 再次串AWS vpc to vpc
1.再次建立 TGW  attachment
<img width="1678" height="813" alt="image" src="https://github.com/user-attachments/assets/f6cbde53-c2fc-41e7-9dac-68c47d4e4613" />
2. 在兩個VPC都需添加對端路由，至路油表添加兩邊的 目的地 >　走ＴＧＷ
<img width="1918" height="813" alt="image" src="https://github.com/user-attachments/assets/c6024f47-662b-41cc-b86c-0bc294cbe3fc" />
<img width="1921" height="821" alt="image" src="https://github.com/user-attachments/assets/02031d18-9f82-4fe4-bf26-2748c9e0fd86" />

## 測試VPC 互ping以及 透過GCP Ping稍早新增的VPC 開機器 
<img width="640" height="245" alt="image" src="https://github.com/user-attachments/assets/b5a03da4-5f91-4b06-beb6-10d78020aa22" />
